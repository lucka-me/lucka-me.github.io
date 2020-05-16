---
title: Android 开发笔记
tags:
  - Tech
  - Programming
date: 2018-08-27 16:14:56
updated: 2018-08-27 16:14:56
---


随着作为某大赛参赛作品提交，[巡检应用 Patroute](https://github.com/lucka-me/Patroute-android "lucka-me/Patroute-android | GitHub") 的制作终于暂告一段落了。

紧接着又开了一个新坑，名为 [RoundO](https://github.com/lucka-me/RoundO-android "lucka-me/RoundO-android | GitHub") 的所谓「城市定向越野」应用。虽然组长称其为定向越野，我个人觉得其实和 Patroute 一样本质上都是「LBS 打卡应用」，所以一开始直接套用了 Patroute 的代码框架进行开发。
不过在开发过程中还是一点一点地做了不少修改，到现在已经完成了当初设想的大部分功能，代码和 Patroute 的有了很大的差异。结果现在再看 Patroute 的代码会有种「完蛋了这种东西绝对会被评委乱棍打到死无音讯」的感觉……

在这里以 RoundO 为例子，把开发过程中学到的东西和解决的问题记一记。

<!-- more -->

## Activity 生命周期
在 Android 开发中， Activity 的生命周期是一个基础概念，很多应用需要应对前后台切换、保存/加载关键数据，这些都必须在正确的生命周期节点（所对应的回调）中完成。个人觉得比较关键的有 `onCreate()`、`onPause()` 和 `onResume()` 这三个节点。

`onCreate()` 是整个生命周期的开始，数据会从这里开始一直保持到 Activity 被销毁，所以在这里除了要完成定义和初始化各个成员、为 UI 组件添加监听器等工作之外，还应该加载关键数据并恢复状态（比如任务数据和状态）。

`onPause()` 和 `onResume()` 两个节点之间是 Activity 真正在最顶层活动的时期，是「安全」的。一旦 `onPause()` 被调用，就意味着 Activity 不再在最顶层活动，也不再「安全」，应用可能不经 `onDestroy()` 就被结束，因此必须在此处保存关键数据（调用 `MissionManager.pause()`），并暂停任务和定位、计时等监听，不过可以转入服务的形式以继续运行。

`onResume()` 则是应用从后台回到顶层时，或者从 `onCreate()` 开始时**都会**经历的节点，在初期我天真地认为应该从这恢复任务（调用 `MissionManager.resume()`）和监听，但其实这样简单的处理存在很大问题：在 RoundO 中，从开始任务界面 `SetupActivity` 点击确认之后会返回 `MainAtivity` 并从 `onActivityResult()` 处开始任务：
```kotlin
when (requestCode) {

    AppRequest.ActivitySetup.code -> {
        if (resultCode == Activity.RESULT_OK) {
            // Something else here
            missionManager.start(locationKit.lastLocation)
        }
    }

}
```
结果导致它与之后才调用的 `onResume()` 的恢复任务相冲突，后者加载的数据覆盖了开始任务所生成的数据，结果任务无法开始。
后来找到了解决方法，就是视情况加载数据，当调用 `MissionManager.pause()` 时会将属性 `state` 修改为 `Pause`，然后在 `onResume()` 中，仅当任务处在这一状态时才恢复数据：
```kotlin
override fun onResume() {
    if (missionManager.state == MissionManager.MissionState.Paused) {
        missionManager.resume()
    }
    locationKit.startUpdate()
    super.onResume()
}
```
另外这样的话在 `onCreate()` 中恢复数据之后也不会在 `onResume()` 中再次恢复数据导致混乱。

## 「面向对象编程」和 MainActivity 的作用
面向对象编程在我看来就是将原本放在一起的变量和函数分门别类，用「类」封装起来，或者将原本通用型的系统接口再次封装等等。总的来说目的都是隐藏细节、令代码更精简、有条理，便于后续开发。比如 Patroute 和 RoundO 中，我将与任务有关的部分封装进 `MissionManager` 里，它的代码行数甚至比 `MainActivity` 还多；在 RoundO 中我还进一步将定位相关部分封装进 `LocationKit`，将地图控制相关部分封装进 `MapKit`，将显示对话框的功能封装进 `DialogKit`，大大简化了 `MainActivity` 的代码。

我认为 `MainAtivity` 是起到一个调度中枢的作用，接收回调并调用各个成员的方法，而具体的工作由各个成员完成。

在这一点上 Patroute 没做好，`MainAtivity` 里还留了很多本该被封装的代码，例如位置更新后检索检查点列表、显示普通警告对话框等等。RoundO 其实也有一些残留（主要是仪表盘对话框），希望后续能继续封装，保持 `MainAtivity` 「调度员」的身份。
~~啊没想到边写这篇文章边改就改完然后 Commit 上去了（~~

## 首选项检查和重置
RoundO 在生成任务前要让用户对参数进行设置，比如任务范围、签到点数量等，我很自然地想到利用首选项（`Preference`）来完成这个功能。不过其中有个要求是必须对用户输入的值进行合法性判断，最理想的方法自然是在用户输入时直接检查、提示和重置，但要实现这个效果似乎必须自己写新的 `Preference` 类。
我不太确定自己能否成功，于是选择在用户输入新值之后在回调中进行判断、提示以及重置
```kotlin
override fun onSharedPreferenceChanged(
    sharedPreferences: SharedPreferences?,
    key: String?
) {
    if (sharedPreferences == null || key == null) return
    when (key) {

        getString(R.string.setup_basic_radius_key) -> {
            if (sharedPreferences.getString(
                    key, getString(R.string.setup_basic_radius_default)
                ).toDouble() < 0.1
            ) {
                resetValue(key, getString(R.string.setup_basic_radius_default))
                warnIllegalValue(R.string.setup_basic_radius_warning)
            }
        }

        //...

    }
}

fun resetValue(key: String, default: String) {
    defaultSharedPreferences.edit().putString(key, default).apply()
}
```
但 `resetValue()` 里的代码似乎不会立即刷新首选项的显示，即 `SharedPreferences` 中值已经重置，但界面，包括 `EditTextPreference` 对话框里依然显示原来的值，必须退出这个 Activity 再进入才能看到新值。

对此，根据 [Stack Overflow 的答案](https://stackoverflow.com/a/31671831 "How do you refresh PreferenceActivity to show changes in the settings? | Stack Overflow")，需要重新载入首选项资源：
```kotlin
fun resetValue(key: String, default: String) {
    defaultSharedPreferences.edit().putString(key, default).apply()
    preferenceScreen.removeAll()
    addPreferencesFromResource(R.xml.preference_setup)
}
```
这样在重设之后界面会闪（刷新）一下，不算完美，但~~偷懒~~目的达成。

## 前台服务
在实际使用中，Android 应用处在前台的时间其实非常少，一个 Activity 通过 `onPaused()` 进入后台其实是非常频繁的，除了回到桌面、切到其它应用，甚至打开应用内的 Activity（比如首选项界面）、锁屏都会进入后台。为了让用户更加自由，就要在进入后台时启动服务，继续定位和进行任务。

服务有后台、前台和绑定三种服务，RoundO 使用的是前台服务，即通过 `startService()` 或 `startForegroundService()` 配合 `startForeground()` 启动的服务（[Android Developers 的中文页面](https://developer.android.com/guide/components/services?hl=zh-cn "服务 | Android Developers")还是按照旧分类描述的，没有更新，是个坑……），它可以尽量保证服务不被结束，不过需要提供一条持续通知来令服务对用户可见。

由于之前没经验，在前台服务上就栽了好几个跟头。

首先要确定的是，因为线程的关系，正常情况下当应用本身结束的时候服务也会随之结束，**而且此时服务的 `onDestroy()` 并不一定会被调用**，按我的是研究过来看是「几乎不会被调用」……所以如果要在服务中进行任务，就必须定时保存数据，虽然听上去很没效率，但对于不熟悉多线程的我来说这能解决不少问题：在 `onResume()` 中，我这样处理服务和任务：
```kotlin
override fun onResume() {
    if (missionManager.state == MissionManager.MissionState.Paused) {
        // Stop the background service
        val backgroundMissionService =
            Intent(this, BackgroundMissionService::class.java)
        stopService(backgroundMissionService)

        missionManager.resume()
    }
    locationKit.startUpdate()
    super.onResume()
}
```
这里 `stopService()` 是可以引发服务内 `onDestroy()` 回调的，但服务使用的似乎并不是主线程，如果不做多线程处理，`missionManager.resume()` 会先一步执行，因此在服务的 `onDestroy()` 中存储数据是行不通的。

其次，前台服务要求提供一条通知，而通知必须设置三个要素：标题（`setContentTitle()`）、内容（`setContentText()` 或 `setContent()`）和小图标（`setSmallIcon()`），[缺一不可](https://www.jianshu.com/p/5792cf3090bc "自定义 Notification 样式遇到的坑 | 简书")。

再次是对 Android O 的适配。我用来测试的两台手机分别是魅族 MX4 Pro（Flyme 6.3，Android 5.1.1）和华为 G7 Plus（EMUI 4，Android 6.0.1），模拟器最近大概是~~喝醉了~~哪里坏掉了，装不上应用。后来借到了同学的小米 6（MIUI，Android 8.0.0）测试了一小会，发现 Android O 宛如天坑。首先，启动前台服务必须在 `MainActivity` 里就换用 `startForegroundService()`，这个不是问题，问题是 Android O 要求先建立 `NotificationChannel` 创建一个通过 ID 识别的通知频道，再调用 `startForeground()` 才能正常显示通知……
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    val notificationChannel =
        NotificationChannel(
            CHANNEL_ID,
            getString(R.string.app_name),
            NotificationManager.IMPORTANCE_DEFAULT
        )
    notificationChannel.description =
        getString(R.string.service_notification_channel_description)
    notificationManager?.createNotificationChannel(notificationChannel)
}
```
官方指南里根本没写啊……还是我没看见……啊反正最后还是 [Stack Overflow](https://stackoverflow.com/a/44705829 "Android foreground service notification not showing | Stack Overflow") 把我从苦海中拯救出来的……

然后关于后台进程保活的问题，终于遇到了，~~传说中~~各种国产 ROM 的坑！
最开始意识到这个问题是在华为上测试的时候，每次锁屏系统都会把服务杀掉，根本不留情面，代码怎么写都没用，隔壁魅族就一直好好的，后来摸索了大半天发现 EMUI 的手机管家里有个「受保护应用」，介绍里赫然写着「锁屏后清理后台应用」……

{% blockquote %}  
哇
绝了
无话可说
<div style="font-size:32px;">RoundO</div>
<div style="font-size:64px;">保护</div>
行了
{% endblockquote %}

于是为了引导用户开启这个选项，我在首选项界面里加了一个「后台限制」的说明选项，为此特意去查了如何[检测 ROM 类型](https://blog.csdn.net/yu75567218/article/details/78109686 "Android获取操作系统名称 | CSDN")以及[打开后台设置](https://www.jianshu.com/p/797e23f10266 "Android 设置默认桌面,默认应用,辅助功能,电池优化,设备管理器,悬浮窗等 | 简书")，然后又发现 Android O 因为缩紧文件权限所以[又不一样](https://blog.csdn.net/zwww_/article/details/80340335 "Android O更改状态栏颜色出现 /system/build.prop (Permission denied) | CSDN")……

最后就这样：
```kotlin
fun getType(): RomType {
    if (Build.VERSION.SDK_INT < Build.VERSION_CODES.O) {
        val buildProp = Properties()
        buildProp.load(FileInputStream(File(Environment.getRootDirectory(), "build.prop")))
        return when {
            buildProp.containsKey(EMUI_KEY) -> RomType.EMUI
            buildProp.containsKey(MIUI_KEY) -> RomType.MIUI
            Build.DISPLAY.toUpperCase().contains(FLYME_KEYWORD) -> RomType.FLYME
            else -> RomType.OTHER
        }
    } else {
        try {
            @SuppressLint("PrivateApi")
            val clazz = Class.forName(SYSTEM_PROP_CLASS)
            val methodGet =
                clazz.getMethod("get", String::class.java, String::class.java)
            return when {
                (methodGet.invoke(clazz, EMUI_KEY, "") as String) != "" -> RomType.EMUI
                (methodGet.invoke(clazz, MIUI_KEY, "") as String) != "" -> RomType.MIUI
                Build.DISPLAY.toUpperCase().contains(FLYME_KEYWORD) -> RomType.FLYME
                else -> RomType.OTHER
            }
        } catch (error: Exception) {
            return RomType.OTHER
        }

    }

}
```
居然用了私有 API…感到良心不安（啥

## 后记
因为个人的陋习，我并没有系统性地学习过 Android 开发，整个开发过程都是「面向 Google 和 Stack Overflow 编程」，也就是「写什么查什么」，因此很多概念都是在这个过程中慢慢理解的，还存在很多不足，对原理的理解也可以说几乎等于零。在写这篇文章的过程中也体会到了：很多东西不知道如何转换成文字来表述，只能用看上去很业余又很莫名其妙的词汇去形容。

经过 Patroute 和 RoundO 的开发，也（再一次）认识到了自己的短板：算法一无所知，数据库一点不会，网络一窍不通，多线程是啥……这应该不是长久之计吧，而且真正的、能糊口等级的应用开发又是什么样子的呢，站在人生岔路口的自己竟然对此一无所知，慌啊。
