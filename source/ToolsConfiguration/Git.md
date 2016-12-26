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
