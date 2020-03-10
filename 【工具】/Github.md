# Github

## Config

可查询 ~/.gitconfig

### 设置姓名和邮箱地址

```shell
Git config –global user.name “”
Git config –global user.email “”
Git config –global color.ur true,显示颜色
```

### 设置私密文件

​	保存数据库的配置文件，每次提交会显示untracked files。可在git工作区创建一个特殊的.gitignore文件，把要忽略的文件名填入。若想强制添加，可用-f。Git add –f name。使用git check-ignore来检查.gitignore文件

### 简写

- st:status
- co:checkout
- ci:commit
- br:branch

可以添加别名，git config --global alias.unstage 'reset HEAD'

- git config --global alias.last 'log -1'
- git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

### 提高命令输出的可读性

Git config –global color.ui auto

### 设置SSH Key

ssh-keygen –t rsa –C “mail”

- -t指定密钥类型，默认是rsa，可以省略
- -C设置注释文字，比如邮箱
- -f指定密钥文件存储文件名
- id_rsa文件是私有密钥
- id_rsa.pub是公开密钥

### 修改本地主目录路径

- 修改起始位置
- 删除目标后的cd-to-home，可实现bash启动默认指向该文件夹
- 新建环境变量HOME
- 将原目录下的.ssh\.gitconfig\.bash_history等文件拷贝到新目录下即可

## Clone

Git clone [git@github.com:jiudc/Hello-World.git](mailto:git@github.com:jiudc/Hello-World.git)

## Git基础操作

### Git init—初始化仓库

生成.git目录，附属于该仓库的工作树

### Git status—查看仓库的状态

### Git add—向暂存区中添加文件

### Git commit—保存仓库的历史记录

- 记述一行提交信息：使用-m参数
- 记述详细提交信息：不适用-m参数，需要编辑
	- 第一行：用一行文字简述
	- 第二行：空行
	- 第三行：记述更改的详细信息

### Git log—查看提交日志

- 只显示提交信息的第一行 –pretty=short
- 显示指定的目录、文件日志，git log filename/dirname
- 显示文件的改动,-p

### Git diff—查看更改前后的差别

- Git diff,查看工作树和暂存区的差别
- Git diff Head,查看工作树和最新提交的差别

### Git branch—显示分支一览表

- Git branch –a, -a参数可以通水显示本地仓库和远程仓库的分支信息
- Git branch
- Git branch name
- Git branch –d name（删除分支）
- Git branch –D name（强制删除分支）

### Git checkout –b创建、切换分支

- Git branch feature-A
- Git checkout feature-A

### Git checkout –切回上一个分支

### Git merge—合并分支

Git merge –no-ff feature-A，no-ff表示禁用Fast forword（该模式下，删除分支，会丢掉分支信息）

### Git log—graph—以图表形式查看分支

### Git reset—回溯历史版本

Git reset –hard 哈希值

### Git reflog—查看当前仓库的操作历史

Git log只能查看以当前状态为终点的历史日志

### Git commit –amend—修改提交信息

### Git rebase –i—压缩历史

### Git commit –am “”

使用一步骤代替两部add和commit

### Git remote add—添加远程仓库

Git remote add orign [git@github.com:jiudc/git-tutorial.git](mailto:git@github.com:jiudc/git-tutorial.git)

Git会设置别名orign

### Git push—推送至远程仓库

Git push –u orign master,-u参数可以将orign仓库的master分支设置为本地仓库当前分支的upstream，添加了这个参数，将来运行git pull从远程仓库获取内容时，该分支可直接从orign的master分支获取内容。后面可通过git push orign master

### Git clone—获取远程仓库

- Git clone git@...
- Git checkout –b feature-D orign/feature-D,获取远程仓库的分支feature-D

### Git pull—获取最新的远程仓库的分支

Git pull origin feature-D

### Git remote –v—查看远端地址

### Git config –list—查看配置

- Git add . //暂存所有的更改
- Git checkout . //丢弃所有的更改
- Git status //查看文件状态
- Git commit – m “本次要提交的概要信息
- Ssh [git@github.com](mailto:git@github.com)测试连接
- Ssh –T [git@github.com](mailto:git@github.com)测试连接
- Ssh –t –p 22 [git@github.com](mailto:git@github.com)更换连接端口

### Git stash

将工作现场保存起来。可用git stash list查看刚才的工作现场。可使用两种方式恢复现场：

- git stash apply，恢复后，stash内容并不删除，需要使用git stash drop删除
- git stash pop，恢复删除一步

### Git fetch--all

更新git remote中所有远程包含的最新的分支

## 多人协作

- Git push orign dev,推送dev分支到远程，/origin/dev
- git checkout –b dev orign/dev，建立远程origin的dev与本地的联系
- git pull，若没有建立连接，则报错
- git branch –set-upstream dev origin/dev

## 标签管理

1. 切换到需要打标签的分支
2.  Git tag v1.0
3. 可以对commit id打上log，如git tag v0.9 6224937
4. 可用git tag命令查看
5. Git tag –m说明文字 –a标签名，可用-s用私钥签名
6. Git show <tagname>查看信息
7. Delete tag tagname
8. Git push origin tagname推送
9. Git push origin –tags，推送所有标签
10. 若需要删除已推送的标签，1）先删除本地标签git tag –d tagname        2）git push origin :refs/tags/tagnam

## 建立git服务器

1. 安装git：sudo apt-get install git
2. 创建git用户：sudo adduser git
3. 创建证书登录：收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
4. 初始化Git仓库：先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：sudo git init --bare sample.git
	 Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：sudo chown -R git:git sample.git
5. 禁用ssh登录：通过编辑/etc/passwd
	 git:x:1001:1001:,,,:/home/git:/bin/bash 改为
	 git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
6. 克隆远程库：git clone
7. 管理公钥：Gitosis
8. 管理权限：Gitolite

## 快捷键

Shift+/显示快捷键

## Tips

### LF will be replaced by CRLF in hello_world.php.

Windows中的换行符为CRLF，而linux下的换行符为LF，因而执行add会有该提示。解决方法是禁用自动转换：
Git config –global core.autocrlf false