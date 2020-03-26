# git_tips
整理日常使用git命令，持续更新中。
git version 2.26.0

### 查看特定commit在哪些分支上存在
```
git branch -a --contains CommitID
    # -a, --all             列出远程跟踪及本地分支
    # --contains <提交>     只打印包含该提交的分支
    # --no-contains <提交>  只打印不包含该提交的分支
```

### 修改当前版本提交信息(人员/日期)
```
git commit --amend
    # --amend               修改先前的提交
    # --author <作者>       提交时覆盖作者
    # --date <日期>         提交时覆盖日期

git commit --amend --date="2020-03-26T19:04:57+0800"
git commit --amend --author='weilong.wang <326688025@qq.com>'
```
