## 电池
### 1.性能与省电
- 高性能模式
- 平衡模式
cpu频率变高,电压变大,相应的耗电量也会上升
```
1.高性能模式：
　　cpu频率在(1664000hz..)
2.平衡模式：
　　cpu频率在(559000hz..)
```

### 2.待机智能省电
- 开关
```
屏幕亮度并没改变,待机时,cpu休眠时,耗电量本来就最小,无实质性优化
```

### 3.省电模式
- 开关
- 自动开启
  - 一律不
  - 电量剩余５%时
  - 电量剩余15%时

概述：
```
省电模式会降低设备的性能，并限制振动　位置信息服务和大部分后台流量。
对于电子邮件　聊天工具等依赖于同步功能的应用，可能要打开这类应用时
才能收到新信息
```

简要分析：
```
开关　或者　自动开启
最终实现功能的方法都是：
updateLowPowerModeLocked()
发送处理广播ACTION_POWER_SAVE_MODE_CHANGING
发送ACTION_POWER_SAVE_MODE_CHANGED更新省电模式状态


"ACTION_POWER_SAVE_MODE_CHANGING" 广播的接收类:
/frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/BatteryControllerImpl.java
/frameworks/base/packages/SystemUI/src/com/android/systemui/power/PowerUI.java

"ACTION_POWER_SAVE_MODE_CHANGED" 广播的接收类:
/frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/BatteryControllerImpl.java
/frameworks/base/packages/SystemUI/src/com/android/systemui/power/PowerUI.java
/frameworks/base/services/voiceinteraction/java/com/android/server/soundtrigger/SoundTriggerHelper.java
/frameworks/base/services/core/java/com/android/server/location/GnssLocationProvider.java
在对应的BroadcastReceiver中处理相关逻辑,比如: 降低屏幕亮度 关闭GPS功能 关闭语音识别 等.
```

### 4.电池优化
- 未优化
- 所有应用
```
1.优化
　建议选择此项以延长电池续航时间

2.不优化
　电池电量的消耗可能会更快
```

对比

 斐讯Email来新邮件不同模式下

 | 模式 | 电池优化 | 电池未优化  |　usb是否连接　|
 |--- | --- | ---| --- |
 |  省电模式 |无铃声，无通知，无振动 | 有铃声，有通知，无振动 |否|
 |  正常模式 |有铃声，有通知，有振动 | 有铃声，有通知，有振动 |否|

  华为　－　斐讯

 | 手机品牌 |　模式　| 无操作灭屏时间 |有无连wifi|　有无提示音　| 有无振动　 |　电量消耗排行前２ | 灭屏cpu是否进入休眠 |　电量值　|
 |--- | ---|---|---|---|---|---|---| ---|
 |华为 |正常模式| 自设 | 有 | 有 |  有　|1.SCREEN 2.ANDROID_SYSTEM   | 是　| 2418 - 2457(mAh) |
 |华为 |省电模式| 限制30s | 有 | 无 |  无　|1.SCREEN 2.IDLE  | 是　| 2379 - 2418(mAh) |
 |华为 |超级省电模式| 限制15s | 有|无 |无| 1.ANDROID_SYSTEM 2.SCREEN  | 是 | 2223 - 2262(mAh)  |
 | 斐讯 |正常模式　| 自设　| 有 | 有|有| 1.SCREEN 2.WIFI |是 | 820 - 820(mAh) |
 | 斐讯 |省电模式　| 自设　| 有 | 有|无| 1.SCREEN 2.WIFI |是 |810 - 810(mAh) |



### 参考资料

谷歌battery功耗分析工具
https://bathist.ef.lc/

官方电池管理
https://source.android.com/devices/tech/power/mgmt

官方优化电池寿命
https://developer.android.com/training/monitoring-device-state/index.html
