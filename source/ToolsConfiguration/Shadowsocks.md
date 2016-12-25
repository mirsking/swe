# Shadowoscks with Arukas.io

## 方法一
### arukas.io
1. docker镜像
tutum/centos:centos7

2. 端口
* 22
* shadowsocks端口


### shadowsocks
#### Install
```
yum install python-pip
pip install shadowsocks
```

#### Run as damond
```
ssserver -d start
```

## 方法二
### arukas.io
```
lowid/ss-with-net-speeder:latest
ssserver -p 8888-k 666666 -m aes-256-cfb
```
![](http://7xnluw.com1.z0.glb.clouddn.com/shadowsocks_arukas.png)


## My servers

### cloudhero
```
swarm1880.cloudout.co
8888
```
