# git版本管理

## 1.基本操作
```
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
git push

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