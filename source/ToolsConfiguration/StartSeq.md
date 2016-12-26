# 修改Ubuntu/Windows启动顺序


修改文件`/boot/grub/grub.cfg`

```bash
# If you change this file, run 'update-grub' afterwards to update
# /boot/grub/grub.cfg.
GRUB_DEFAULT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
```

GRUB_DEFAULT代表的就是启动项的顺序，从数字0开始，依次代表如下启动项（这是在我的电脑上，不同的ubuntu版本和windows系统可能会有一些不同）：
```
Ubuntu, with Linux 2.6.35-28-generic
Ubuntu, with Linux 2.6.35-28-generic (recovery mode)
Memory test (memtest86+)
Memory test (memtest86+, serial console 115200)
Windows 7 (loader) (on /dev/sda1)
```

修改完后
```
sudo update-grub
```
