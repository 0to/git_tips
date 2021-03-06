# git_tips
记录日常遇到的Git问题和对应Git命令。

git version 2.26.0


### 查看特定commit在哪些分支上存在
场景:
1. 某个commit有致命错误，需要在涉及分支上Revert；
2. 某个commit修复了致命缺陷，需要cherry-pick到其它主要分支。
```
git branch -a --contains CommitID
    # -a, --all             列出远程跟踪及本地分支
    # --contains <提交>     只打印包含该提交的分支
    # --no-contains <提交>  只打印不包含该提交的分支

git branch -a --contains 9f4a67782
```

### 查看指定分支上Patch对应了多少Tag
场景:
1. 需要同步指定分支和相关Tags到另外一个代码库；
```
git tag --merged Branch_name
    # --merged <提交>       只打印已经合并的标签
    # --no-merged <提交>    只打印尚未合并的标签

git tag --merged develop
```

### 修改当前版本提交信息(人员/日期)
场景：
1. 机器时间不对，导致git显示版本时很奇怪；
2. Git config 的 username/email有问题，需要更新。

PS：
- 如果当前commit只是在本地仓库，还没有push到远端，那么请放心执行amend操作；
- 如果commit已经push到远端仓库，那么需要很慎重执行amend操作。后续需要force push 和 通知团队成员更新本地git仓库
```
git commit --amend
    # --amend               修改先前的提交
    # --author <作者>       提交时覆盖作者
    # --date <日期>         提交时覆盖日期

git commit --amend --date="2020-03-26T19:04:57+0800"
git commit --amend --author='weilong.wang <326688025@qq.com>'
```

### 配置http/https协议账户密码缓存
场景：
1. 使用http/https协议代码库，有时会出现不能缓存认证信息。每次执行fetch/pull/push命令，都需要输入帐户密码情况。

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

### 查询 merge commit conflict info
场景：
1. 发现一个线上问题，怀疑是某次merge冲突解决引入的
- 查找哪些commit是merge产生的
- 显示merge commit是否有冲突文件处理
```
git log --merges
    # --merges  Print only merge commits. This is exactly the same as --min-parents=2.

git log -1 CommitID --cc
    OR
git show CommitID
    # -c or --cc option to produce a combined diff when showing a merge.
    # This is the default format when showing merges with git-show
```

### bad signature, index file corrupt 问题
场景：
```
$ git status

error: bad signature
fatal: index file corrupt
```

原因分析： .git/index 保存了暂存区的信息（索引信息），index file corrupt说明此文件损坏了。

解决方法：
```
# 删除损坏的索引文件
rm .git/index
# 重建索引文件
git read-tree   # Reads tree information into the index
    OR
git reset      # Reset current HEAD to the specified state
```

### 编写多行commit message方法
日常使用中，我们经常会编写多行commit message来提供更多信息。

有下面几种方法来实现：
1. 如果是个人编写多行commit message：使用editor工具
- core.editor=vim 当前git config编辑工具为vim，各位有可能配置了其他工具；
- 执行git commit 会自动弹出交互式编辑工具窗口，编写完成后保持退出即可。
2. 如果是命令行/脚本自动生成多行commit message：使用多个-m参数来实现
```
-m <msg>
    --message=<msg>
    Use the given <msg> as the commit message. If multiple -m options are given, their values are concatenated as separate paragraphs.

执行命令：
git commit -m "commit title" -m "commit test case" -m "commit by wwl"
结果如下：
Author: weilong.wang@memblaze.com <weilong.wang@memblze.com>
Date:   Thu Jul 16 09:12:17 2020 +0800

    commit title

    commit test case

    commit by wwl
```
