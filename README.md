

## 配置user信息包括user.name和user.email

```
    git config --global user.name 'yourName'
    git config --global user.email 'yourEmail'
```

配置这两个信息，在多人协作中，Name是用于标注代码是谁提交的，Email在code_review时用于邮件通知。

## config的三个作用包括--local，--global，--system
- --local 当前身份只对某个仓库生效，对于公司的项目，公司给你身份时xxx，个人项目随心所欲的使用名称xsxs
- --global 当前身份对所有的仓库有效，不论个人还是公司项目使用统一的身份
- --system 对系统所有登录用户有效，对于服务器类别的电脑，电脑的使用者可能不止一个人，别人也能通过账号密码登录到这台电脑，此时他的项目如果提交之前，应该设置自己信息，如果其他使用这个电脑的人配置了这台git的信息，那么当你在提交代码的时候，显示的则是别人设置的信息

## 显示config的配置
```
    git config --list --local
    git config --list --global
    git config --list --system
```
为了避免发生一些提交信息不正确，使用--list查看config配置了哪些信息


## 建立Git仓库
你当前的项目可能是以下情景的一种或者三种
1. 新建一个git仓库，使用命令`git init projectName`，此时会创建projectName的文件夹，该文件夹中还会生成一个隐藏文件.git,用于存放git的版本信息
2. 本地已有项目，但未使用git管理，进入项目根目录，使用命令`git init`，和第一种情况一样，会生成.git的隐藏文件
3. 从git的远程托管平台git clone xxx,，进入项目，直接进行git操作即可

假设已有git项目分别为git-learn，使用命令配置一个`--global`的git身份
```
git config --global user.name lvanboy
git config --global user.email 1801996111@qq.com
```
进入项目git-learn，查看`git config --list --global`

![图1](./shot-screen/01.png)

再使用命令`git config --local user.name ajpjiang`配置当前项目的git信息，使用`git config --list --local`查看，验证是否会覆盖`--global`的git配置

![图2](./shot-screen/02.png)

显示，`--local`和`--global`他们都是独立的空间，并不会发生覆盖的现象

那么，最终提交的结果会使用哪种身份信息呢？

按照如下操作：

1. 使用git status 查看当前git项目是否有文件发生changes（修改、变更）

