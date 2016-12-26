# Git的一些操作

## 修改上一次提交信息

```
git rebase -i <hash-of-commit-preceding-the-incorrect-one>(这里可以用要改的那次的hash，如果是前一两次也可以用HEAD^或者HEAD^^)
```

* In the editor that opens, change pick to reword on the line for the incorrect commit.
* Save the file and close the editor.
* The editor will open again with the incorrect commit message. Fix it.
* Save the file and close the editor.
```
git push --force
```

## Git tag
### 加tag
#### 本地加tag
```
git tag -a v1.0 -m "version 1.0"
```

#### push tag到remote
```
git push --tag
```

### 删除tag
#### 本地删除tag
```
git tag -d v1.0
```

#### remote删除tag
```
git push origin :refs/tags/v1.0
```
