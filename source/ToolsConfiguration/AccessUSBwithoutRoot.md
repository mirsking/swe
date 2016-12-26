# 免Root用户访问USB

创建ttyUSB权限规则文件

etc/udev/rules.d/70-ttyUSB.rules
 ```
KERNEL=="ttyUSB*", OWNER="root", GROUP="root", MODE="0666"
```
