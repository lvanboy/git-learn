

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

按照如下操作（如果忽略其中的步骤，操作会失败，并给出相应的操作提示）：

1. 使用git status 查看当前git项目是否有文件发生changes（修改、变更）,如果发生修改则显示

![图3](./shot-screen/03.png)
一些红色的文件标识了出来，很显眼。

2. 使用`git add [文件名|.]`，多数情况使用**.**通配符，添加所有变更文件到stage（暂存区|缓存），你可以不停的变更文件，然后add操作，都会不再git上留有记录，也就是说，你无法找回前n次修改的内容。再次`git status`查看项目文件状态
![图4](./shot-screen/04.png)
一些绿色的文件表示了出来，也很显眼。

3. 使用`git commit -m'提交'`,将文件提交，一旦提交，档案成立，你可以commit多次，这些记录一直都在，`-m`描述当前提交是做了什么事情，变更了功能，修复什么bug、添加哪些文档等等，everything is ok.这些记录一直都你的电脑硬盘上
![图5](./shot-screen/05.png)

4. 使用命令`git log`,查看
![图6](./shot-screen/06.png)
这里有两次commit记录，有提交的时间，配置的git信息，提交时的描述，这里Author，最新的一次记录是ajpjiang，也就是--local生效了，而--global没有生效。

**由此得出结论在提交记录的时候，--local配置的信息大于--global。**



## git add 暂存区的意义
假设有这样一个场景，实现xxx需求，`git status`,`git add .`后，发现这个写法不够好，代码质量太低，想把所有add的内容全部撤销，回到初始状态，放弃之前编辑的代码，也就是当前commit时的代码状态，使用命令`git --rest --hard`。慎重使用，别把自己一天的成果reset了。
 

 ## git 文件/文件夹 重命名
1. 在本地修改文件或文件夹后，执行git status后，如图：
![图7](./shot-screen/07.png)
2. 如果你没有按照提示操作，直接`git add .`,`git commit -m'xxxx'`,你的记录将会出现添加文件，和删除文件的记录，而实际上你只是进行重命名的操作。正确的做法应该按照提示操作
```
git add 修改后的文件名
git rm 修改前的文件名
```
![图8](./shot-screen/08.png)

3. 上面操作过于麻烦，最快的做法是git mv 当前文件名  期望修改的文件名(如果你只是因语义问题，期望改变文件名称时，你应该这样做)。
![图9](./shot-screen/09.png)



## git log 查询版本演变历史
1. 直接执行`git log`，显示git提交的详细信息
![图10](./shot-screen/10.png)

2. 为了更清晰的显示主要提交做了什么，省略其他额外的不必要的信息，命令`git log --oneline`
![图11](./shot-screen/11.png)

3. 如果这个项目提交了好几百次，你只期望查看最近的3（或者任意次）次提交记录，命令`git log -n5 --oneline`
![图12](./shot-screen/12.png)

4. 假设不止一个分支，为了满足这个条件，使用命令git checkout -b [BranchName] [CommitID],创建一个分支
![图13](./shot-screen/13.png)
使用命令`git log --all --oneline`,查看所有分支的简要提交信息
![图14](./shot-screen/14.png)
使用命令`git log --all --oneline --graph`,以图形分叉的方式展示出分支克隆的关系,因为当前只是创建了一个分支，并没有在当前分支上进行修改和提交，最左侧展示了一条直线
![图15](./shot-screen/15.png)
修改feature-dev分支，并提交一次，在使用命令`git log --all --oneline --graph`查看。
![图16](./shot-screen/16.png)
使用命令`git log --oneline feature-dev`，只查看feature-dev分支的简要提交记录。
![图17](./shot-screen/17.png)

