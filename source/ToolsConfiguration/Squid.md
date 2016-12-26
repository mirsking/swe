# Ubuntu下设置squid做代理上网

-------
##Install squid
        sudo apt-get install squid
##Config squid

- 找到`visible_hostname`，在下面的#none后面加上一行：
`visible_hostname proxy`

- 找到`http_access allow localhost`，在它前面加上两行：
```
acl csc104_networks src 10.12.218.0/24
http_access allow csc104_networks
```
> note:10.12.218.0/24表示网段是10.12.218.0，子网掩码是24位，子网掩码为：255.255.255.0，用二进制表示为：11111111 11111111 11111111 00000000 (连续的“1”的个数为24个)

- 找到maxconn，添加：
        acl allowmax maxconn 30

- 找到http_port，修改为：
        http_port 808

## run squid

```
sudo squid -z    #显示“2008/03/02 18:21:33| Creating Swap Directories”创建交换目录
sudo squid -k parse    #分析一下配置文件
sudo squid    #启动squid代理

sudo squid -k shutdown    #关闭squid代理
```



## Client config

### windows

Internet 选项，局域网设置，设置代理地址和端口为：
10.12.218.16:808

### Linux
- chrome 利用AutoProxy

- apt配置：
/etc/apt/apt.conf
```
Acquire
{
  http::Proxy "http://172.16.***.***:808/";
};
```
