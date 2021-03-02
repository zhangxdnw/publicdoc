# API接口实现需求

## API接口分析
1. 源码中实现名称为"smdt"的系统服务，实现以下AIDL接口

|AIDL接口|内容|备注|
|:---|:---|:---|
|shutdown|关机|调用系统关机接口|
|reboot|重启|调用系统重启接口|
|execSuCmd|管理员权限执行cmd|运行su shell|
|setPowerOffOnAlarm|定时关机||
|smdtSetPowerOffOnAlarm|待确定||
|getCurrentNetType|获取网络类型||
|getMCUVersion|获取MCU版本信息||
|getAndroidBoardType|获取主板类型||
|smdtGetSDcardPath|获取SD卡路径||
|smdtGetUSBPath|获取USB路径||
|smdtSilentInstall|静默安装||
|getUartPath|获取Uart路径||
|getSerialPorts|获取串口名称||
|openSerialPort|打开串口||
|getFormattedKernelVersion|获取内核版本信息(标准格式)||
|getRunningMemory|获取运行时内存||
|getInternalStorageMemory|获取外部存储||
|setVolumeStates|待确定||
|setRotation|设置屏幕方向||
|setUSBEnable|设置USB使能||

2. SMDT所有接口
* SmdtManager

|序号|接口|内容|实现方式|备注|
|:---|:---|:---|:---|:---|
|1|smdtGetAPIVersion|返回API版本|SDK已实现|格式如下 XXX-Vn-201YMMDD|
|2|getAndroidModel|获取目前设备的型号|SDK已实现|ro.product.model DS83X|
|3|getAndroidVersion|获取目前设备的 android 系统的版本|SDK已实现|ro.build.version.release 7.1.1|
|4|getRunningMemory|获取设备的硬件内存大小容量|AIDL实现|硬件内存大小，以 GB 为单位 2GB|
|5|getInternalStorageMemory|获取设备的硬件内部存储大小容量|AIDL实现|硬件内部存储大小，以 GB 为单位 8GB|
|6|getFirmwareVersion|获取设备的固件 SDK 版本|SDK已实现|ro.product.firmware  v1.2rc3|
|7|getFormattedKernelVersion|获取设备的固件内核版本|AIDL实现|3.4.39 wxl@server-109 #14|
|8|getAndroidDisplay|获取设备的固件系统版本和编译日期|SDK已实现|ro.build.display.id octopus_perf_eng 4.4.4 KTU84Q 20160318 test-keys|
|9|smdtSetTimingSwitchMachine|定时开关机(先关机后开机)|AIDL实现|"9:50", "20:10"， 1|
|10|shutDown|正常关闭系统|AIDL实现||
|11|smdtReboot|正常重启系统|AIDL实现||
|12|smdtSetPowerOnOff|设置开关机时间间隔|Native方法|需查找哪个so中实现|
|13|smdtWatchDogEnable|在应用层使能或者关闭开门狗功能|Native方法|需查找哪个so中实现|
|14|smdtWatchDogFeed|喂狗一次，对看门狗计数进行复位操作|Native方法|需查找哪个so中实现|
|15|smdtTakeScreenshot|截取当前全屏为 png 格式图片并重命名到相应位置|SDK已实现|screen -p|
|16|smdtScreenShot|截图增加一个方法直接返回 bitmap 的方法|SDK已实现|SurfaceControl.screenshot|
|17|setRotation|设置屏幕逆时针旋转 N 角度|AIDL实现||
|18|smdtGetScreenWidth|获取显示屏分辨率宽 X 像素|SDK已实现||
|19|smdtGetScreenHeight|获取显示屏分辨率高 Y 像素|SDK已实现||
|20|getExtendScreenWidth|获取副屏分辨率宽 X 像素|SDK已实现|sys/class/graphics/fb4/screen_info|
|21|getExtendScreenHeight|获副屏分辨率高 Y 像素|SDK已实现|sys/class/graphics/fb4/screen_info|
|22|smdtSetStatusBar|设置显示或者隐藏动态状态栏|系统服务实现|com.smdt.action.displaySystemUI/com.smdt.action.hideSystemUI|
|23|smdtGetStatusBar|获取当前动态状态栏显示或者隐藏状态|SDK已实现||
|24|smdtSetLcdBackLight|熄灭屏幕，只关背光，却不进入休眠，软件继续运行|Native方法|需查找哪个so中实现|
|25|smdtGetLcdLightStatus|获取屏幕状态|Native方法|需查找哪个so中实现|
|26|setBrightness|设置背光亮度|SDK已实现||
|27|smdtInstallPackage|将重启进行 update 差分包 file 升级||需要配合确认|
|28|smdtRebootRecovery|将重启进入 recovery 模式||需要配合确认|
|29|smdtSilentInstall|静默安装 APK 应用|AIDL实现||
|30|smdtGetEthMacAddress|获取设备以太网的 MAC 地址|SDK已实现|/sys/class/net/eth0/address|
|31|smdtGetEthIPAddress|获取设备以太网的 IP 地址|SDK已实现||
|32|smdtSetEthIPAddress|设置设备以太网的 IP 地址|SDK已实现|"192.168.1.100", "255.255.255.0", "192.9.50.1","202.96.134.133"|
|33|getCurrentNetType|获取当前网络连接的类型|AIDL实现|2G/3G/4G/WIFI/ETH/null|
|34|smdtGetEthernetState|获取以太网状态 3288|SDK已实现||
|35|smdtGetSDcardPath|获取外部存储 SD 卡路径|AIDL实现||
|36|smdtGetUSBPath|获取外部存储 U 盘路径|AIDL实现||
|37|unmountVolume|卸载外部存储|系统服务实现|com.smdt.unload.storage|
|38|smdtReadExtROM|读取外部 EEPROM 存储|Native实现|需查找哪个so中实现|
|39|smdtWriteExtROM|写入外部 EEPROM 存储|Native实现|需查找哪个so中实现|
|40|getUartPath|获取串口绝对路径|AIDL实现||
|41|smdtSetUsbPower|设置 USB 口电源|Native实现|需查找哪个so中实现|
|42|smdtReadExtrnalGpioValue|获取 IO 口输入状态|Native实现|需查找哪个so中实现|
|43|smdtSetExtrnalGpioValue|设置 IO 口输出状态|Native实现|需查找哪个so中实现|
|44|smdtSetGpioDirection|设置 IO 口方向|Native实现|需查找哪个so中实现|
|45|smdtGetGpioDirection|获取 IO 口方向|Native实现|需查找哪个so中实现|
|46|smdtSetXrm117xGpioValue|设置外部扩展 IO 值|未找到||
|47|smdtGetXrm117xGpioValue|获取外部扩展 IO 值|未找到||
|48|smdtSetXrm117xGpioDirection|设置外部扩展 IO 口方向|未找到||
|49|smdtGetXrm117xGpioDirection|获取外部扩展 IO 口方向|未找到||
|50|smdtSetVolume|设置当前声音大小|SDK已实现||
|51|smdtGetVolume|获取当前通道声音|SDK已实现||
|52|setHeadsetMicOnOff|开关耳机麦克风|系统服务实现|/sys/devices/ff660000.i2c/i2c-2/2-001c/mic_status|
|53|setTime|设置并保存系统时间|系统服务实现|alarm|
|54|execSuCmd|将以 ROOT 权限运行 shell 命令|AIDL实现||
|55|smdtGetSystemLogcat|抓取 Android 层的 LOG 并保存相应目录|SDK已实现||
|56|smdtSetControl|其它设备相关设置|Native实现|需查找哪个so中实现|
|57|getScreenNumber|获取双屏异显接口的值|系统服务实现|/sys/class/param/smdt_param/screen_number|
|58|getHdmiinStatus|获取 Hdmi in 状态值|系统服务实现|/sys/class/hdmiin_reg/hdmiin_status|
|59|setHdmiInAudioEnable|开关 Hdmi in 声音|系统服务实现|audio|

SDK已实现：接口实现是在SDK中，可暂时认为无开发工作，但有一些接口可能需要系统支持，例如一些属性查询
AIDL实现： 需在源码中自定义一个smdt的服务，实现ISmdtManager接口
系统服务实现： 需检查系统中是否有匹配的调用逻辑
Native实现： 原生实现，SDK中未包含so文件，所以认为so会放在系统某个地方
