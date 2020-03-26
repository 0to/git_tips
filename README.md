# git_tips
整理日常使用git命令，持续更新中。

git version 2.26.0


### 查看特定commit在哪些分支上存在
```
git branch -a --contains CommitID
    # -a, --all             列出远程跟踪及本地分支
    # --contains <提交>     只打印包含该提交的分支
    # --no-contains <提交>  只打印不包含该提交的分支

git branch -a --contains 9f4a67782
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

### 配置http/https协议账户密码缓存
使用http/https协议代码库，有时会出现不能缓存认证信息。每次执行fetch/pull/push命令，都需要输入帐户密码情况。

参考文档： [gitcredentials](https://git-scm.com/docs/gitcredentials)
```
git config credential.helper cache  
    # Helper to temporarily store passwords in memory.
    # Number of seconds to cache credentials (default: 900)

git config credential.helper 'cache --timeout=3600'
    # 自定义缓存时间 3600秒，在此期间不需要再次输入账户密码

git config credential.helper store
    # Helper to store credentials on disk.
    # 永久存储账户密码信息在当前机器，适合在信赖机器上面进行设置
```
