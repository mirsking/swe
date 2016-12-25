## VNC server
### 安装realvnc并破解

[百度网盘](https://pan.baidu.com/s/1cCFnHW) 密码: 3khy

* 5.05秘钥
```
H322B-2B225-XR3S9-BGVXZ-9674A
```

* 5.1秘钥
```
BQ24G-PDXE4-KKKRS-WBHZE-F5RCA
BQ24G-PDXE4-KKKRS-WBHZE-F5RCA
8ZEZH-QPANM-NX3A5-8C4TS-8B97A
7AB4X-3YNXF-C5MRR-59DJG-7HGNA
UPL8P-CN2MT-85ERA-N3E3B-GERDA
```

### vncserver 开机启动
```
sudo update-rc.d vncserver-x11-serviced defaults
```
## 选择桌面
### LXDE
```
sudo apt-get install lxde
```

添加文件 /etc/vnc/xstartup.custom
``` bash
#!/bin/sh
DESKTOP_SESSION=LXDE
export DESKTOP_SESSION
startlxde
vncserver-virtual -kill $DISPLAY
```
### [KDE plasma-desktop](http://bbs.archbang.org/viewtopic.php?id=5088)
添加文件 /etc/vnc/xstartup.custom
``` bash
#!/bin/sh
DESKTOP_SESSION= plasma
export DESKTOP_SESSION
startkde
vncserver-virtual -kill $DISPLAY
```

**Notice:** xstartup.custom要拥有执行权限

```
sudo chmod +x xstartup.custom
```

## 添加桌面
在/etc/rc.local中添加
```
su mirsking -c "vncserver-virtual -geometry 1600x900 :24"
```
