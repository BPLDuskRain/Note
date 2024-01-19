# GIT
## 使用方式
### 命令行(git bash)
#### 配置git config
>git config user.name "<名字>"

>git config user.email <邮箱> 

--local本地配置（一个仓库）

--global全局配置（自己的所有仓库）（常用）

--system系统配置（很多人的很多仓库）

--list查看配置
#### 新建仓库
>git init

需要在某目录下使用，将本目录变成git仓库（产生.git隐藏文件夹存放git数据）
>git clone <地址>

从远程服务器克隆仓库
#### 工作方式（多级提交）
数据管理
1. 工作区working directory
2. 暂存区index
3. 本地仓库local repository

在工作区修改后通过git add添加到暂存区，再通过git commit提交到仓库

文件状态
1. 未跟踪：新创建的
2. 未修改：保存的内容
3. 已修改：修改了，未添加
4. 已暂存：添加了，未提交

> git status

查看仓库状态

-s以简略模式查看
>git add <文件名>

将文件加入暂存区

通配符：

    可以使用*.txt的格式提交所有同一格式的文件

文件夹：

    可以使用.直接提交当前文件夹
>git commit

将暂存区文件提交到本地仓库

-m "我是信息" 提交提交信息 ；不使用此项会进入vim界面编辑提交信息

>git log

查看提交记录

--oneline以简洁方式查看提交信息
#### 回退版本
>git reset <版本号>

--soft保留工作区和暂存区（未提交）

--hard丢弃工作区和暂存区（未修改）

--mixed保存工作区,丢弃暂存区（未添加）

>git reflog
查看操作记录,可以找到新版本号以回溯到新版本作为错误使用了--hard参数的补救
#### 查看差异
>git diff
 
默认比较工作区和暂存区差异（包括文件差异和文件内容差异）

HEAD 比较工作区和当前版本仓库差异(HEAD表示最新提交节点)

--cashed 比较暂存区和当前版本仓库仓库差异

git diff <版本号> <版本号> 比较两个版本差异

git diff HEAD~ 比较当前版本和上一个版本

git diff HEAD~2 比较当前版本和上上个版本

git diff HEAD~2 file.txt 比较两个版本之间某个文件的差异 

git diff <分支名> <分支名> 比较两个分支差异

差异是以前一个版本为基准的
#### 删除文件
对于在工作区已经被删除的文件，如果还在暂存区，可以通过git add <该文件>更新暂存区内容

>git rm <文件名>

在工作区、暂存区中都删除该文件

--cashed只删除暂存区中的该文件（即踢出暂存区）

最后git commit就可以从版本库中更新掉删除文件
#### 忽略文件
一般来说，可忽略文件包括：

    系统自动生成
    编译中间文件
    日志、缓存、临时
    敏感配置文件

创建一个.gitignore文件，将需要忽略的文件名（可以使用通配符）写入该文件

其中采用简化正则表达Blob形式匹配

但该方法对已经纳入git检测的文件无效
### GUI(git GUI)
### IDE
# GitHub
## 创建远程仓库
### SSH方式克隆仓库
git clone <ssh地址>

直接使用会报错，因为需要配置SSH密钥
#### 配置密钥
在根目录下的.ssh目录中

ssh-keygen -t rsa -b 4096

生成大小为4096的rsa协议的ssh密钥

输入文件名作为密钥文件名

可以选择输入SSH密码

请注意，不带后缀的是私钥，而有后缀的是公钥

另外还需要一个名为config的文件使访问github时指定使用某个密钥

config文件内容形如

    # github
    Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/NOOB

#### 配置到GitHub
在setting中打开SSH and GPG keys
## 远程仓库同步
git push

本地仓库提交

git pull

远程仓库拉取
## 远程仓库关联
git remote add <别名> <地址>

在本地仓库目录使用，关联一个远程仓库并可以起别名

git remote -v

查看别名

git push -u <别名> <分支1>:<分支2>

将本地仓库的分支1推送给别名的远程仓库的分支2

git pull <别名> <分支1>:<分支2>

将别名的远程仓库的分支1拉取到本地仓库的分支2