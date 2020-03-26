# git_tips
整理日常使用git命令，持续更新中；

## 查看特定commit在哪些分支上存在
```
git branch -a --contains CommitID
    # -a, --all             列出远程跟踪及本地分支
    # --contains <提交>     只打印包含该提交的分支
    # --no-contains <提交>  只打印不包含该提交的分支
```
