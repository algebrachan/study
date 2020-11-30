# git版本管理

## 1.基本操作
```shell
# 设置用户名和邮箱
git config (--global) user.name xxxx
git config (--global) user.email xxx@xxx
# 配置ssh免密登陆

# 初始化本地仓库
git init 
# 克隆远程仓库
git clone XXX@xxx.git  #推荐使用ssh地址
# 开发过程操作
git add . 
git add 具体文件
git commit -m 'mod 说明'
git status
git push -u origin master

# 同步本地仓库到远程
git remote add origin git@xxxx.com:xx/xx.git
# 删除远程分支
git push origin --delete 分支名称
```

### 1.1 配置ssh多个git免密登录

```shell
ssh keygen -t rsa -C "xxx@xxx.com" -f ~/.ssh/id_rsa
# 将 id_rsa.pub 添加到remote的仓库中
# 在./ssh目录下 新建config文件配置如下：
Host github.com                                       HostName github.com                                 PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
  User algebrachan
```

### 1.2 git其他操作
```shell
 # git更新版本
 git update-git-for-windows # 速度很慢 不推荐
 # 版本回退 
 git log --pretty=oneline
 git reset --hard HEAD^ # ^代表上一个版本 ~100：上100个版本
 git reset --hard a4e37 # commit id
 git commit # [master 318ee13] test 2 [分支 commit id] commit说明
 
 # 撤销修改 回到最近一次add 或者commit 没有--就变成切换分支了
 git checkout -- 1.txt 
 # 版本库删除文件(不推荐)
 git rm 1.txt
 
```

- [git淘宝镜像](https://npm.taobao.org/mirrors/git-for-windows/)

### 1.3 分支管理

- 分支可以理解为平行时空
- master为主线，其他为分线，需要增加、合并、删除
- 团队合作 master 分支主要是稳定的发布版本，dev 分支用来开发

```shell
# 创建dev分支 然后切换到dev -b是创建 checkout是切换 建议使用switch
 git checkout -b dev # 不推荐
 git checkout master # 不推荐
 
 git switch -c dev
 git switch master
 
 git branch
# 合并分支 merge用于合并指定分支到当前分支 使用--no-ff 删除分支之后不会丢掉分支信息
 git merge --no-ff -m "merge 信息" dev
# 删除分支
 git branch -d dev
# 合并存在冲突时，要解决冲突之后再提交
 
# BUG分支

# 存储工作现场 git status就是干净的
 git stash
 git stash list
# 恢复工作现场 并删除stash
 git stash pop
 
# Feature分支 强行删除未开发完的分支
 git branch -D feature-vulcan
 
# 多人合作 分支提交报错
 git branch --set-upstream-to <branch-name> origin/<branch-name># 关联分支

# 标签
 git tag v1.0 # 用于新建一个标签，默认为HEAD 也可指定一个commit id
 git tag -a <tagname> -m "..."
 git show <tagname>
# 操作标签
 git push origin <tagname>
 git push origin --tags #推送全部未推送的本地标签
 git tag -d <tagname>
 git push origin :refs/tags/<tagname>
```
### 1.4 参考资料
- [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

## 2 问题记录及操作方案