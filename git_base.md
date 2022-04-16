## git基础

### 一、常用的dos命令

进入了命令提示符等于打开了我的电脑

- dir命令：查看当前目录

- cd:进入相应的目录
  
  cd / -->回到根目录
  
  cd Users -->进入Users目录
  
  cd ..  -->回到上一级目录
  
  cd  Users/Administrator  -->进入到Users中的Administrator目录

- 使用 x:进入某个盘，如:`d:`表示进入d盘

- mkdir  创建一个文件夹 
  
  - mkdir 01   -->创建01的文件夹

- echo a > a.txt  -->创建一个文件a.txt，在里面写入a这个内容，内容是重写

- echo aa >> a.txt  -->在a.txt中追加aa的内容

- type a.txt  -->看a.txt中的内容

- rmdir 01 -->删除01文件夹，注意01文件夹必须使空文件夹，非空文件夹不能删除

- del a.txt -->删除a.txt文件
  
  练习：在d盘创建test文件夹，进入test文件，创建01,02,03三个文件夹，在01中创建a.txt文件，在02中创建b.txt文件，在03中创建c.txt文件，之后查看文件内容，最后删除文件c.txt并且删除03文件夹。

### 二、git的配置

安装完成之后，需要对git进行最基本都配置，一般来说需要配置一个用户和邮箱。这个用户和邮箱将会是你将来github中的用户和邮箱

```shell
#设置用户名和用户邮箱
git config --global user.name "ynkonghao"
git config --global user.email "ynkonghao@gmail.com"
```

成功之后，在用户目录中会多一个`.gitconfig`的文件在文件夹中会显示刚才配置的user，此时需要添加color和alias，方便后续操作

```shell
[user]
    name = ynkonghao
    email = ynkonghao@gmail.com
[color]
    ui = true
[alias]
    lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    co = commit
```

在一个空文件夹中，通过命令`git init`完成工厂的初始化操作。初始化完成之后，可以看到一个隐藏的文件夹.git，使用命令`dir /a`可以进行查看，也可以在我电脑里面查询

```shell
D:\teach_source\20_wlw\git\01>git init
Initialized empty Git repository in D:/teach_source/20_wlw/git/01/.git/

D:\teach_source\20_wlw\git\01>dir /a
 驱动器 D 中的卷没有标签。
 卷的序列号是 7BAC-67DD

 D:\teach_source\20_wlw\git\01 的目录

2022/04/16  09:31    <DIR>          .
2022/04/16  09:31    <DIR>          ..
2022/04/16  09:31    <DIR>          .git
               0 个文件              0 字节
               3 个目录 334,652,563,456 可用字节
```

### 三、Git操作概览

首先创建一个新的文件，使用`git status`会显示该文件是一个新文件，这个文件没有被git所管理。

```shell
D:\teach_source\20_wlw\git\01>git status
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

使用git add命令可以把文件添加到git的stage中

```shell
git add a.txt #添加a.txt
git add . #添加文件夹中的所有文件

D:\teach_source\20_wlw\git\01>git add a.txt

D:\teach_source\20_wlw\git\01>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   a.txt
```

现在依然没有被git的仓库管理，需要使用commit命令将stage中内容提交到git的仓库中。

```shell
git co -m "add a.txt"
```

提交之后可以通过git log来查询提交的日志，由于在配置文件中对log进行了简化，可以直接使用git lg来查看日志

```shell
D:\teach_source\20_wlw\git\01>git lg
* 8323dbc - (HEAD -> master) add a.txt (22 seconds ago) <ynkonghao>

D:\teach_source\20_wlw\git\01>git log
commit 8323dbc18cc5871dc8cbf5a994c6eed923ed95ee (HEAD -> master)
Author: ynkonghao <ynkonghao@gmail.com>
Date:   Sat Apr 16 09:56:39 2022 +0800

    add a.txt
```

继续向文件夹中修改一个新的内容

```shell
D:\teach_source\20_wlw\git\01>echo world >> a.txt

D:\teach_source\20_wlw\git\01>type a.txt
hello
world

D:\teach_source\20_wlw\git\01>git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

此时新的内容并没有提交到stage中，需要提交之后才能进行commit

```shell
D:\teach_source\20_wlw\git\01>git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   a.txt
```

添加完成之后，应该使用git commit进行提交，提交之后就会产生一个新的版本

```shell
D:\teach_source\20_wlw\git\01>git co -m "update a.txt"
[master 8818ef3] update a.txt
 1 file changed, 1 insertion(+)

D:\teach_source\20_wlw\git\01>git status
On branch master
nothing to commit, working tree clean

D:\teach_source\20_wlw\git\01>git lg
* 8818ef3 - (HEAD -> master) update a.txt (10 seconds ago) <ynkonghao>
* 8323dbc - add a.txt (13 minutes ago) <ynkonghao>
```

以上流程中有两个版本，88xxx,8323xx这表示版本号，接着就显示提示信息，(HEAD->master)表示这个版本是当前版本。使用reset命令可以恢复到某个具体的版本

```shell
D:\teach_source\20_wlw\git\01>git reset --hard HEAD~ #恢复到上一个版本
# git reset --hard HEAD~~
HEAD is now at 8323dbc add a.txt
```

最常用的方式使用版本号来恢复具体的版本

```shell
D:\teach_source\20_wlw\git\01>git reset --hard  8818ef3
HEAD is now at 8818ef3 update a.txt
```

### 四、git的几种状态

git的几种状态如下图所示

![git的四种状态](https://ynkonghao.github.io/img/git/02/01.png)

- Untracked状态表示的就是新添加状态，当添加了一个新的文件就是Untracked，表示没有在stage或者仓库中的状态。

- 当执行add之后，进入Staged状态

- 当执行commit直接到Unmodified状态，表示内容已经提交

- 编辑了文件之后又会进入Modified状态，因为此时文件已经被管理了，但是内容和Staged不一样，此时通过add方法又会将其添加到Stage中

- 通过命令git restore --staged a.txt会将stage中的所有文件清空，此时回到unstaged状态

- 此时如果把文件删除，并且提交到工厂，之后添加新的同名文件，还是untracked状态。

如果有如下需求，添加一个新的文件，并且往新的文件中添加了具体的内容，之后添加到stage，继续进行修改，文件的状态会显示两个，一个是Stage中的，一个是修改状态。

```shell
D:\teach_source\20_wlw\git\01>echo ddddd >> d.txt

D:\teach_source\20_wlw\git\01>git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   d.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   d.txt
```

此时如果后面修改的内容有问题，需要还原，可以通过checkout命令，

```shell
D:\teach_source\20_wlw\git\01>git checkout d.txt
Updated 1 path from the index
```

checkout命令首先会找stage区的文件，如果stage区没有，就会去找工厂中还原。

### 五、远程工厂

在github上注册一个用户，之后创建一个public的项目，创建完成之后，如果是一个新项目，可以直接把自己项目中的内容提交上去。这种操作只要在自己的git的项目中使用git remote命令就可以完成

添加完成之后截图

![](https://tvax2.sinaimg.cn/large/0068ld55gy1h1be41uucjj318i0puk62.jpg)

```shell
D:\teach_source\20_wlw\git\01>git remote add origin https://github.com/ynkonghao/20wlw_test.git
```

添加完成之后，以后如果希望提交到远程工厂，不用再使用remote，直接使用push完成提交

```shell
git push ##相当于执行了 git push -u origin master
```

此时如果希望在一个新的文件夹中导入远程工厂的内容

```shell
git clone https://github.com/ynkonghao/20wlw_test.git
```

clone是所有用户都可以clone，但是只有授权用户才能push，clone完成之后就会有完整的远程工厂的内容，此时，可以对内容进行修改，修改完成之后又可以进行push（要有权限）

此时如果要在另外一个工厂中获取最新的更新

```shell
git pull
```
