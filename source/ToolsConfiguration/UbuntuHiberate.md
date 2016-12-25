# Ubuntu开启休眠模式

[官方文档](http://askubuntu.com/questions/94754/how-to-enable-hibernation)

修改如下配置文件
```
/etc/polkit-1/localauthority/50-local.d/com.ubuntu.enable-hibernate.pkla
```


Ubuntu 14.04 and later:
```
[Re-enable hibernate by default for login1]
  Identity=unix-user:*
  Action=org.freedesktop.login1.hibernate
  ResultActive=yes

[Re-enable hibernate for multiple users by default in logind]
  Identity=unix-user:*
  Action=org.freedesktop.login1.hibernate-multiple-sessions
  ResultActive=yes
```
