# Mac上用Brew管理python

## Python
```
brew install python
```

## pip
```
easy_install pip
```

### pip error
```
DistutilsOptionError: must supply either home or prefix/exec-prefix -- not both
```

添加`~/.pydistutils.cfg`文件，内容是
```
[install]
prefix=
```

参考[stackoverflow](http://stackoverflow.com/questions/24257803/distutilsoptionerror-must-supply-either-home-or-prefix-exec-prefix-not-both)
