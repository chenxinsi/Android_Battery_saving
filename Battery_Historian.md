# Battery Historian

### 数据准备
battery-historian工具需要使用bugreport中的Battery History
数据，我们在开始的时候需要通过以下命令来打开电池数据的获取以及重置：

```
adb shell dumpsys batterystats --enable full-wake-history
adb shell dumpsys batterystats --reset
```

### 数据获取
获取bugreport信息，生成zip包
```
adb bugreport
```

### 数据分析

上传生成的bugreport信息压缩包至分析地址
https://bathist.ef.lc/
