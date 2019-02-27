# macOS Mojave(10.14) 配置python环境

## 配置头文件依赖
```
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

## 安装pyenv
```
brew install pyenv
```

## 安装python2.7.15
```
pyenv install 2.7.15
pyenv global 2.7.15
```
若遇到python.org下载Python安装包太慢，则先从镜像下载Python到缓存目录，再执行上述命令
```
mkdir -p ~/.pyenv/cache; wget http://npm.taobao.org/mirrors/python/2.7.15/Python-2.7.15.tgz -O ~/.pyenv/cache/Python-2.7.15.tar.xz
```

## 一些问题

### vim报系统python 3.7.0不存在而闪退？
使用brew安装一个供vim等软件使用的python
```
brew install python3
```

### 安装oss4blog
```
pip install oss4blog
```
启动`oss4blog`时会报错，`from oss4blog.oss4blog import main`，解决方案：
```
rm ~/.pyenv/versions/2.7.15/bin/oss4blog.py*

```

### 安装matplotlib
matplotlib依赖thinker，需要先安装系统tcl/tk库，然后再重新pyenv install
```
# mac
brew install tcl-tk
# ubuntu
sudo apt-get install tk-dev
```