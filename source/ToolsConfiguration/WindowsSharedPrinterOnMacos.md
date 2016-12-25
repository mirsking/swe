# Mac上安装windows共享打印机

高级中添加
`smb://IPAddress/Printer'sName`

比如地址为10.12.218.4的打印机，打印机名称为：`HP LaserJet P2050 Series PCL6 (副本 1)`，则实际转义后地址为：

```
smb://10.12.218.4/HP%20LaserJet%20P2050%20Series%20PCL6%20(%E5%89%AF%E6%9C%AC%201)
```
