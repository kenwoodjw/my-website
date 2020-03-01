---
title: "Arch_linux deepin 桌面蓝牙连接故障解决"
date: 2019-05-04T22:47:12+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["linux"]
---

### arch linux deepin桌面蓝牙无法连接

重装桌面deepin，发现没有蓝牙功能，按照wiki<https://wiki.archlinux.org/index.php/Bluetooth_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>

安装bluetooth

```shell
yaourt -S pulseaudio-alsa pulseaudio-bluetooth bluez bluez-libs bluez-utils

```

启动bluetooth

```
sudo systemctl start bluetooth
sudo systemctl enable bluetooth
```

移除设备

```
bluetoothctl
➜  my-website git:(master) ✗ bluetoothctl 
Agent registered
[F9-5.0]# devices 
Device 40:EF:4C:61:xx:xx EDIFIER R1700BT
Device 34:36:3B:D1:xx:xx 34-36-3B-D1-xx-xx
Device 76:84:EA:CB:xx:xx 76-84-EA-CB-xx-xx
Device CB:4F:51:12:xx:xx Mi Band 3


remove xx:xx...
```



### 设备连接不了，日志里有错误

如果你在连接设备时看见 `journalctl` 的输出有类似的消息：

或者 sudo systemctl status bluetooth

```
a2dp-source profile connect failed for 9C:64:40:22:E1:3F: Protocol not available
```



解决方法

安装 [pulseaudio-bluetooth](https://www.archlinux.org/packages/?name=pulseaudio-bluetooth) 再重启 [PulseAudio](https://wiki.archlinux.org/index.php/PulseAudio)。这个错误在文件传输是也会有。

```
killall pulseaudio
pulseaudio --start --log-target=syslog
```

