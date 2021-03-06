Android下的配置管理之道之git的使用

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


    【马哥私房菜】亲情推出《linux shell脚本攻略》视频教程

    【马哥私房菜】亲情推出 git 视频教程



# git help
git help 命令用来显示任何命令的 Git 自带文档。     
对于每一个命令的完整的可选项及标志列表，你可以随时运行 git help <command> 命令来了解。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git config
/etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。
``` 
git config --system
``` 
~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。
``` 
git config --global 
``` 
当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。
```bash
git config --local

```
每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖~/.gitconfig, /etc/gitconfig 中的同名变量。 

```bash
$ git config --global user.name "马哥私房菜"
$ git config --global user.email bright.ma@mage.com
$ git config --global core.editor vim
$ git config --global color.ui true

```
最后在~/.gitconfgi 文件下内容如下了
```
[color]
    ui = true
[user]
    email = bright.ma@mage.com
    name = 马哥私房菜
[core]
    editor = vim

    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这种git的配置文件我们也可以直接使用编辑器取编辑修改。  
配置文件中的空行被忽略。井号，分号开头的行是注释行，被忽略的。

配置文件中的数据类型
```
Type
    --bool                value is "true" or "false"
    --int                 value is decimal number
    --bool-or-int         value is --bool or --int
    --path                value is a path (file or directory name)

```

列出所有配置信息 可以使用 git config --list 命令
```bash
$ git config --list --local
user.email=<https://shop592330910.taobao.com/>
user.name=马哥私房菜
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/mageSFC/myblog.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master


```

指定使用某个配置文件，可以使用-f，--file选项,该选项和--system,--local,--global不能同时使用的
```bash
git config -f some_to_path/config --list
```

下面再举个例子
```text
#Given a .git/config like this:

#
# This is the config file, and
# a '#' or ';' character indicates
# a comment
#

; core variables
[core]
    ; Don't trust file modes
    filemode = false

; Our diff algorithm
[diff]
    external = /usr/local/bin/diff-wrapper
    renames = true

; Proxy settings
[core]
    gitproxy=proxy-command for kernel.org
    gitproxy=default-proxy ; for all the rest

; HTTP
[http]
    sslVerify
[http "https://weak.example.com"]
    sslVerify = false
    cookieFile = /tmp/cookie.txt
```

设置filemode为true：
```
git config core.filemode true
```
删除diff下面的renames
```
git config --unset diff.renames
```
查询某个给定的key的value
```
git config --get core.filemode

git config       core.filemode
```



# git init

```
git init        # 在一个空目录下面初始化一个空的仓库。
                # 在存在的仓库下会reinit一个仓库的。作用不大。
```
```
git init --bare # 建一个裸仓库
git init --bare my.git
一般的裸仓库名称我们习惯在目录名上带上.git结尾。
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
```
git init --shared[=(false|true|umask|group|all|world|everybody|0xxx)]

git init --shared=everybody

git init --shared=777 # 这样个出来的.git目录权限就会是777.
```

```
git init --template=<template_directory> #这个是指定一个模板路径。默认的路径是/usr/share/git-core/templates.

```

```bash
git init --separate-git-dir=<git dir> #这个选项用来分开存放.git目录的。

git init my  --separate-git-dir my.git # 这样初始化一个空的仓库，名字是my，my这个目录也就是我们的工作区间。
但是真正的仓库是在my.git这个目录下面的。 同时在my工作区间目录下面不会再有.git目录了，代替的一个.git文件。

$ git init my --separate-git-dir=my.git
Initialized empty Git repository in /home/mamh/tmp/my.git/

$ ls 
 my/  my.git/  test/

$ ll my                                                                                                     
total 0
$ ls -lha my
total 12K
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 ./
drwxrwxr-x 7 mamh mamh 4.0K 6月   1 08:42 ../
-rw-rw-r-- 1 mamh mamh   30 6月   1 08:42 .git
$ cat my/.git
gitdir: /home/mamh/tmp/my.git

$ ll my.git                                                                                                 
total 32K
-rw-rw-r-- 1 mamh mamh   23 6月   1 08:42 HEAD
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 branches/
-rw-rw-r-- 1 mamh mamh   92 6月   1 08:42 config
-rw-rw-r-- 1 mamh mamh   73 6月   1 08:42 description
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 hooks/
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 info/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:42 objects/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:42 refs/
$                                                 
```
```bash
git init --separate-git-dir=../my.git  在一个已经存在的git仓库下面执行这样的命令，可以把当前仓库的.git目录移到外面某个目录下面。

如果是对一个裸仓库执行这样的命令，他也会把仓库移到指定的目录下面，但是原来的仓库好像没什么变化的。
```

环境变量 GIT_DIR   和   GIT_WORK_TREE
```bash
#  如果设置了这个$GIT_DIR环境变量，默认初始化时候是带当前目录下面创建的.git目录，有了这个环境变量， .git目录下面的内容就会放到这个环境变量路径下面了。

export GIT_DIR=/home/mamh/tmp/aaa.git
$ git init bbb 
Initialized empty Git repository in /home/mamh/tmp/aaa.git/ 
$ ls -lh 
total 575M
drwxrwxr-x  7 mamh mamh 4.0K 6月   1 08:54 aaa.git/
drwxrwxr-x  2 mamh mamh 4.0K 6月   1 08:54 bbb/

$ ls -lha aaa.git bbb
aaa.git:
total 40K
drwxrwxr-x 7 mamh mamh 4.0K 6月   1 08:54 ./
drwxrwxr-x 6 mamh mamh 4.0K 6月   1 08:54 ../
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 branches/
-rw-rw-r-- 1 mamh mamh   66 6月   1 08:54 config
-rw-rw-r-- 1 mamh mamh   73 6月   1 08:54 description
-rw-rw-r-- 1 mamh mamh   23 6月   1 08:54 HEAD
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 hooks/
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 info/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:54 objects/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:54 refs/

bbb:
total 8.0K
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 ./
drwxrwxr-x 6 mamh mamh 4.0K 6月   1 08:54 ../
通过上面的初始化方法，我初始化了一个bbb的仓库，通过环境变量GIT_DIR指定仓库的路径，这里是aaa.git，然后我们发现bbb目录是个空目录。下面什么文件都没的。这里我们的bbb目录应该是一个工作区间目录的。
$ git status 
fatal: This operation must be run in a work tree
$ git diff 
fatal: This operation must be run in a work tree
这个时候你执行一些命令会报不是个工作目录的。这个时候我们可以使用另外一个环境变量来解决GIT_WORK_TREE

$ export GIT_WORK_TREE=/home/mamh/tmp/bbb 
$ git add .                   
$ git st                       
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   t.txt
	
这个时候git就能正常的识别到哪个是仓库目录，哪个是工作目录了。

一般情况很少这样使用的。


```
选项 --work-tree 和   --git-dir 和上面的环境变量起到了同样的作用。
```
git --work-tree=<some path> --git-dir=<some path> 

git --work-tree=/home/mamh/tmp/bbb --git-dir=/home/mamh/tmp/aaa.git   status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   t.txt
```



# git clone

```
git clone user@git.example.com:/opt/git/my_project.git

git clone --bare https://github.com/mageSFC/my_project my_project.git

git clone https://github.com/mageSFC/myblog.git

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

--local选项，--no-hardlinks选项
```bash
git clone --local /home/mirror/repo.git r1
--local，-l 选项，这个会在克隆一个本地路径的仓库时候 它会把  .git/objects/ 下面的文件做成硬链接的形式。（尽可能的，如果不能做成硬链接，他就会是复制的形式了）

这个只有在路径是个本地路径的情况下面才有用的。如果是ssh://, http://这样的远端仓库路径就不会使用硬链接了。

--no-local 和上面的意思相反，他不会使用硬链接的。

$ git clone --local /home/mirror/repo.git 
Cloning into 'repo'...
done.

$ ll repo/.git/objects/pack 
total 924K
-r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
-r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

$ ll repo/.git/objects/pack -i    
total 924K
57278473 -r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
57278469 -r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

$ ll /home/mirror/repo.git/objects/pack -i  
total 924K
57278473 -r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
57278469 -r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

通过上面的--local我们看到文件的i节点编号是一样的，说明我们克隆的时候采用了硬链接的方式。

默认不加上--local，git自行判断，也会优化使用硬链接的方式的。

git clone --no-hardlinks      -- copy files instead of hardlinking when doing a local clone 也是不采用硬链接的方式。 

git clone --no-hardlinks   /home/mirror/repo.git repo_bak #一般的这个使用情况是，你想备份一个仓库，就可以使用这种不带硬链接的克隆了。

```

--bare和--mirror的区别
```bash
git clone --mirror https://github.com/mageSFC/myblog.git  myblog.git   #服务器上面会这么来克隆一个裸仓库，服务器上引用第三方仓库。

git clone --bare https://github.com/mageSFC/my_project my_project.git   # 这个选项仅仅是复制克隆一个裸仓库。

--bare和--mirror的区别是

$ git clone --mirror /home/mirror/repo.git r1
$ git clone --bare /home/mirror/repo.git r2

$ ls r1 r2                                                                                                                                     
r1:
branches/  config  description  HEAD  hooks/  info/  objects/  packed-refs  refs/

r2:
branches/  config  description  HEAD  hooks/  info/  objects/  packed-refs  refs/
从表面上看，这两种方式克隆下面的都是裸仓库，没上面区别。但是我们看看他们的config文件就知道了
$ cat r1/config    
[core]
	repositoryformatversion = 0
	filemode = true
	bare = true
[remote "origin"]
	url = /home/mirror/repo.git
	fetch = +refs/*:refs/*
	mirror = true
 
$ cat r2/config   
[core]
	repositoryformatversion = 0
	filemode = true
	bare = true
[remote "origin"]
	url = /home/mirror/repo.git

我们可以看到--mirror的配置文件上是多了这个“fetch = +refs/*:refs/*”的。
这个说明我们可以执行git fetch来继续获取上游仓库的更新。没有这个是不能继续更新的。所以--mirror更常用一些。



```

--reference 使用参考仓库来克隆仓库
```bash
git clone https://github.com/mageSFC/myblog.git --reference <本地仓库镜像库路径>

git clone https://github.com/mageSFC/myblog.git --reference         /home/mirror/myblog.git
git clone https://github.com/mageSFC/myblog.git --reference-if-able /home/mirror/myblog.git

如果参考的本地镜像仓库不存在--reference  选项会报错的。    --reference-if-able不会报错，会继续克隆的。

这个参考的仓库要是一个本地的裸仓库的。加上这个选项会节省克隆时间，节省磁盘空间的。


$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/kernel/msm-4.9.git                             
Cloning into 'msm-4.9'...
remote: Counting objects: 3064590, done
remote: Finding sources: 100% (3012366/3012366)
remote: Total 3012366 (delta 2393732), reused 2939537 (delta 2393732)
Receiving objects: 100% (3012366/3012366), 1.36 GiB | 11.24 MiB/s, done.
Resolving deltas: 100% (2393732/2393732), completed with 9850 local objects.
Checking connectivity: 8230985, done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.

$ du -sh msm-4.9/.git  
1.5G	msm-4.9/.git
$ cat msm-4.9/.git/objects/info/alternates  
/home/mirror/kernel/msm-4.9.git/objects
使用了 --reference 选项克隆后.git目录只有1.5G大小。
使用了 --reference 选项的克隆，.git/objects/info/alternates文件里面会指向镜像仓库的路径。


$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 
Cloning into 'msm-4.9'...
remote: Counting objects: 8230985, done
remote: Finding sources: 100% (8230985/8230985)
remote: Total 8230985 (delta 6720180), reused 8140778 (delta 6720180)
Receiving objects: 100% (8230985/8230985), 2.70 GiB | 18.83 MiB/s, done.
Resolving deltas: 100% (6720180/6720180), done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.
$ du -sh msm-4.9/.git  
3.0G	msm-4.9/.git
$ cat msm-4.9/.git/objects/info/alternates 
cat: msm-4.9/.git/objects/info/alternates: No such file or directory

通过比较加不加 --reference /home/mirror/kernel/msm-4.9.git 我们看到.git目录大小不一样，加上要小很多的。
然后就是加上会有msm-4.9/.git/objects/info/alternates这个文件的。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/msm-4.9.git                                 
Cloning into 'msm-4.9'...
fatal: reference repository '/home/mirror/msm-4.9.git' is not a local repository.
如果这个本地镜像仓库路径不存在，会报错的。

如果使用了--reference-if-able不会报错的。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/kernel/msm-4.9.git --dissociate  
Cloning into 'msm-4.9'...
remote: Counting objects: 3064590, done
remote: Finding sources: 100% (3012366/3012366)
remote: Total 3012366 (delta 2393732), reused 2939537 (delta 2393732)
Receiving objects: 100% (3012366/3012366), 1.36 GiB | 16.33 MiB/s, done.
Resolving deltas: 100% (2393732/2393732), completed with 9850 local objects.
Checking connectivity: 8230985, done.
Counting objects: 8230985, done.
Compressing objects: 100% (2540121/2540121), done.
Writing objects: 100% (8230985/8230985), done.
Total 8230985 (delta 5970312), reused 6375489 (delta 4914488)
warning: remote HEAD refers to nonexistent ref, unable to checkout.

$ du -sh msm-4.9/.git  
3.7G	msm-4.9/.git

$ cat msm-4.9/.git/objects/info/alternates
cat: msm-4.9/.git/objects/info/alternates: No such file or directory

--dissociate选项：
如果同时使用这个选项和--reference 选项，克隆的时候也会参考镜像仓库的，也是会提高克隆速度的。
但是不会创建 .git/objects/info/alternates 文件的。 “.git”目录的大小会变大的。

这个--reference选项对于特别大的仓库很有用。对于多人在一台服务器上克隆然后开发的情况也是很有用的。
这个镜像仓库一般可以使用git clone --mirror来创建。

```


--branch &lt;name&gt;, -b &lt;name&gt;选项  和 --single-branch选项
```bash
在不使用--branch选项的时候git clone的时候检出的分支是HEAD指向的那个分支。
使用这个选项我们就可以在克隆时候检出某个指定的分支了。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo 
Cloning into 'repo'...
remote: Counting objects: 3990, done
remote: Finding sources: 100% (3990/3990)
remote: Total 3990 (delta 2671), reused 3990 (delta 2671)
Receiving objects: 100% (3990/3990), 1.23 MiB | 18.55 MiB/s, done.
Resolving deltas: 100% (2671/2671), done.
$ cd repo                                
$ git branch -a                                                                                      
* stable
  remotes/origin/HEAD -> origin/stable
  remotes/origin/maint
  remotes/origin/master
  remotes/origin/stable

下面我们使用了-b选项，指定了检出maint分支。
$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo -b maint 
Cloning into 'repo'...
remote: Counting objects: 3990, done
remote: Finding sources: 100% (3990/3990)
remote: Total 3990 (delta 2671), reused 3990 (delta 2671)
Receiving objects: 100% (3990/3990), 1.23 MiB | 26.84 MiB/s, done.
Resolving deltas: 100% (2671/2671), done.
$ cd repo                                                                                                                                
$ git branch -a 
* maint          这里说明了检出的分支是maint分支。
  remotes/origin/HEAD -> origin/stable
  remotes/origin/maint
  remotes/origin/master
  remotes/origin/stable

--single-branch选项什么意思呢？ 它和-b选项配合使用，让克隆的时候只下载-b指定的这一个分支。
在上面的例子我们看到了克隆的时候git会把所有分支都下载到本地的，例如有maint，master，stable，这3个分支都下载了。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo -b maint --single-branch 
Cloning into 'repo'...
remote: Counting objects: 1668, done
remote: Finding sources: 100% (1764/1764)
remote: Total 1764 (delta 1117), reused 1700 (delta 1117)
Receiving objects: 100% (1764/1764), 716.75 KiB | 7.47 MiB/s, done.
Resolving deltas: 100% (1117/1117), done.
$ cd repo                    
$ git branch -a
* maint
  remotes/origin/maint
$  
这个时候我们看到只有maint分支了。其他分支都没有下载。
$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo
	fetch = +refs/heads/maint:refs/remotes/origin/maint
[branch "maint"]
	remote = origin
	merge = refs/heads/maint
通过.git/config 配置文件我们可以看到 fetch = +refs/heads/maint:refs/remotes/origin/maint 这么一行，它就是指定了只下载服务器上的maint分支到本地。
之前的这一行样子是这个的： fetch = +refs/heads/*:refs/remotes/origin/*，星号代表所有，refs/heads/*也是所有分支的意思。
你要是想再下载其他分支，可以在那个fetch行下面加上一行  fetch = +refs/heads/stable:refs/remotes/origin/stable 。


这个-b和--single-branch的一个使用场景是：
在一个仓库下面，我们有个分支源码分支bsui，
另一个是二进制分支bsui_apk.二进制分支都是放的源码分支编译生成的apk文件的，这个分支很大，很占磁盘空间的。
对于研发人员来说，他只会关系bsui源码分支，下载的时候也是只想下载这个bsui分支。另外一个分支是没什么用的。
这个时候就可以使用 --branch bsui --single-branch 这2个选项了。


$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService                                      
Cloning into 'I19tService'...
remote: Counting objects: 6884, done
remote: Finding sources: 100% (6884/6884)
remote: Total 6884 (delta 3550), reused 6698 (delta 3550)
Receiving objects: 100% (6884/6884), 6.68 GiB | 47.00 MiB/s, done.
Resolving deltas: 100% (3550/3550), done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.
$ du -sh I19tService/.git
6.7G	I19tService/.git

下面是使用了--branch bsui --single-branch选项的效果，磁盘空间只有600MB。之前是6GB。这就是差异。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch        
Cloning into 'I19tService'...
remote: Counting objects: 5995, done
remote: Finding sources: 100% (5995/5995)
remote: Total 5995 (delta 2897), reused 5604 (delta 2897)
Receiving objects: 100% (5995/5995), 663.97 MiB | 50.77 MiB/s, done.
Resolving deltas: 100% (2897/2897), done.
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:29  /home/mamh/tmp
665M	I19tService/.git

如果使用了--reference .git目录大小会更小。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git             
Cloning into 'I19tService'...
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:33  /home/mamh/tmp
164K	I19tService/.git

如果使用了--dissociate和 --reference 选项，目录大小恢复为600MB了。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git  --dissociate
Cloning into 'I19tService'...
Counting objects: 5995, done.
Compressing objects: 100% (3211/3211), done.
Writing objects: 100% (5995/5995), done.
Total 5995 (delta 2519), reused 4618 (delta 1722)
$ du -sh I19tService/.git     
646M	I19tService/.git



```

--depth <depth>浅克隆
```bash
git clone --depth 3  /home/mirror/repo.git
使用这个选项是，浅克隆的意思，后面指定一个数字，下载的时候只下载指定的提交历史。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo --depth 3 
Cloning into 'repo'...
remote: Counting objects: 87, done
remote: Finding sources: 100% (90/90)
remote: Total 90 (delta 7), reused 45 (delta 7)
Receiving objects: 100% (90/90), 164.53 KiB | 4.22 MiB/s, done.
Resolving deltas: 100% (7/7), done.
$ cd repo                                                                                                                                    
$ git ll                                                                                                                                
* eceeb1b - Support broken symlinks when cleaning obsolete paths                      — Dan Willemsen (HEAD -> stable, tag: v1.12.37, origin/stable, origin/HEAD) - (1 year, 8 months ago)
* 16889ba - Revert "Repo: fall back to http, if ssh connection fails for http repos"  — Dan Willemsen (tag: v1.12.36) - (1 year, 8 months ago)
*   40d3952 - Merge "On project cleanup, don't remove nested projects"                — David Pursehouse (tag: v1.12.35) - (1 year, 8 months ago)
|\  
| * 4350791 - On project cleanup, don't remove nested projects                       — Dan Willemsen (grafted) - (1 year, 9 months ago)
* d648045 - implement optional 'pushurl' in the manifest file                        — Steve Rae (grafted) - (1 year, 10 months ago)
$
加上了--depth选项，这个git log历史只有这么4个,或者说是5个了。

$ cat .git/config   
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo
	fetch = +refs/heads/stable:refs/remotes/origin/stable
[branch "stable"]
	remote = origin
	merge = refs/heads/stable
加上这个--depth，克隆的时候只会下载到一个分支，这里是stable，应为服务器上配置的HEAD执向的是stable分支。

$ ls .git 
branches/  config  description  HEAD  hooks/  index  info/  logs/  objects/  packed-refs  refs/  shallow
在浅克隆的时候.git目录下面会有个shallow文件



```


--origin &lt;name&gt;, -o &lt;name&gt;选项
```bash
默认克隆的时候，使用的名称是origin，我们可以通过.git/config 看到
$ cat I19tService/.git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService
	fetch = +refs/heads/bsui:refs/remotes/origin/bsui
[branch "bsui"]
	remote = origin
	merge = refs/heads/bsui
	
这里[remote "origin"]标记的就是远端仓库名称是origin。

使用--origin选项在克隆的时候可以更改这个名称，一般不会取改它的。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git   --origin o
fatal: destination path 'I19tService' already exists and is not an empty directory.
$ rm -rf I19tService 

$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git   --origin o
Cloning into 'I19tService'...
$ cat I19tService/.git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "o"]
	url = ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService
	fetch = +refs/heads/bsui:refs/remotes/o/bsui
[branch "bsui"]
	remote = o
	merge = refs/heads/bsui
这里就使用了  --origin o选项，把这个名称改为了“o”
```


--no-checkout选项
```bash
加上这个选项 clone的时候不会把源码检出到工作目录。默认是会检出的。有时候会提示个warning，就是检出源码失败。
 warning: remote HEAD refers to nonexistent ref, unable to checkout.
一般这种情况是服务器端HEAD引用的那个分支是个不存在的分支，下载到本地检出代码的时候就报错了。

$ git clone /home/mirror/kernel/msm-4.9.git --no-checkout                                                                                    
Cloning into 'msm-4.9'...
done.
$ ls msm-4.9                                                                                                                                 

$ rm -rf msm-4.9                                                                                                                             
$ git clone /home/mirror/kernel/msm-4.9.git                                                                                                  
Cloning into 'msm-4.9'...
done.
Checking out files: 100% (58866/58866), done.
$ ls msm-4.9  
AndroidKernel.mk  build.config.goldfish.arm    build.config.goldfish.mips64  certs/   crypto/         firmware/  init/   Kconfig  MAINTAINERS  net/            samples/   sound/     usr/
arch/             build.config.goldfish.arm64  build.config.goldfish.x86     COPYING  Documentation/  fs/        ipc/    kernel/  Makefile     README          scripts/   techpack/  virt/
block/            build.config.goldfish.mips   build.config.goldfish.x86_64  CREDITS  drivers/        include/   Kbuild  lib/     mm/          REPORTING-BUGS  security/  tools/
$  
```

--template=<template_directory>选项
```bash
git clone --template=<template_directory>
--template=<template_directory>选项指定一个模板路径，克隆的时候使用。我们在git init时候讲过这个选项。

$ git clone --template=/usr/share/git-core/templates /home/mirror/kernel/msm-4.9.git

```

--no-tags选项
```bash
git clone  --no-tags /home/mirror/repo.git
克隆的时候不下载tags标签，使用这个选项会在.git/config 配置文件中有个这个配置：remote.<remote>.tagOpt=--no-tags 。
不使用这个选项是会下载标签的。      

```

-v,-q,--progress选项
```bash
git clone -v, --verbose 克隆的时候显示更多的详细信息。
git clone -q, --quiet 克隆时候不显示任何提示信息。
git clone --progress 强制显示进度报告。

```

# git add
git add 命令将内容从工作目录添加到暂存区（或称为索引（index）区），以备下次提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```bash
git add --dry-run,-n  <pathspec>   # 不去真正执行add命令，只是打印出来将会被git添加进版本的文件。

git add --force,-f  <pathspec>     # 这个会把.ignore文件中被忽略的文件也添加到版本库中。

git add --update，-u  <pathspec>   # 这个选项不会把没有被git追踪的文件添加进来，也就是新建的文件不会添加进来

git add  -A,--all, --no-ignore-removal # 这个选项会把所有的文件都添加进来，包括删除的，新增的，修改的。
                                       # 如果没有给出具体的<pathspec> 这个选项会把当前版本库的工作空间中的所有文件都添加进来的。
                                       
git add --no-all, --ignore-removal     # 这个不会把删除的文件添加进来。

git add   [option]  -- <pathspec>  # 这里 -- 的作用就是分开选项和文件，让git知道哪些是选项，哪些是文件名。如果文件名和选项重名了这个就很有用了。

git add Documentation/\*.txt  #这个会把 Documentation 和 子目录下面的所有.txt 文件添加进来的。这里加了反斜线，这个星号就交给git命令来扩展了。

git add git-*.sh
                                       

```

# git rm
git rm 是 Git 用来从工作区，或者暂存区移除文件的命令。 在为下一次提交暂存一个移除操作上，它与 git add 有一点类似  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git mv
git mv 命令是一个便利命令，用于移到一个文件并且在新文件上执行git add命令及在老文件上执行git rm命令。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git status

git status 命令将为你展示工作区及暂存区域中不同状态的文件。 这其中包含了已修改但未暂存，或已经暂存但没有提交的文件。  
一般在它显示形式中，会给你展示一些关于如何在这些暂存区域之间移动文件的提示。  

红色表示修改了。  

绿色的文件表示执行过git add之后的状态  

 -s, --short 选项，显示简短格式的状态  
 --long 选项，默认选项，就是长格式的状态。  

# git diff

此命令可以查看你工作环境与你的暂存区的差异（git diff 默认的做法）  

你暂存区域与你最后提交之间的差异（git diff --staged ）  
或者比较两个提交记录的差异（git diff master branchB）  

--staged 和 --cached 是同义词  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git difftool

当你不想使用内置的 git diff 命令时。git difftool 可以用来简单地启动一个外部工具来为你展示两棵树之间的差异。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git commit

git commit 命令将所有通过 git add 暂存的文件内容在数据库中创建一个持久的快照，然后将当前分支上的分支指针移到其之上。  

git commit --amend  追加提交到最后一个快照上。  
-m 选项，将提交信息与命令放在同一行  
-a 选项, 跳过使用git add命令，直接把修改过后的文件直接作为一个提交  
-s, --signoff 添加signoff行  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  


# git clean
git clean 是一个用来从工作区中移除不想要的文件的命令。 可以是编译的临时文件或者合并冲突的文件。  
git clean 命令只会移除没有忽略的未跟踪文件,-x 选项就会移除.gitignore里面忽略的文件。  
-d 移除未追踪的目录  
-n 选项来运行命令，这意味着 “做一次演习然后告诉你 将要 移除什么”。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
git clean -fdx  

  

# git branch
git branch testing 这会在当前所在的提交对象上创建一个指针。  


git branch -d testing 删除分支，-D 如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 -D 选项强制删除它。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
git branch -u origin/serverfix

git branch 如果不加任何参数运行它，会得到当前所有分支的一个列表  
```bash
$ git branch 
* (HEAD detached at cf7c083)
  master
  new
  test

```
git branch --list <pattern> 列出所有匹配模式的分支
```bash
$ git branch --list
* (HEAD detached at cf7c083)
  master
  new
  test

特别注意，如果后面跟上了pattern，这个--list选项必须加上的，如果不加上会变成创建分支的功能。
$ git branch --list 'maint*' 
  maint-dev
  maint-sh
  maint-test
  maint1.0.1
  
$ git branch --list 'maint*'  'dev*'   # 这个pattern可以有多个的。
  dev
  dev-sh
  maint-dev
  maint-sh
  maint-test
  maint1.0.1
  
特别注意 -l 选项可不是--list的缩写选项。-l是--create-reflog的意思    
```

git branch --color，--no-color这2个选项来控制是否输出显示为彩色

git branch --edit-description &lt;branchname&gt; 编辑分支描述信息，会把信息写到.git/config 文件中

git branch -r, --remotes 显示远端分支列表
```bash
$ git branch -r  
  origin/HEAD -> origin/master
  origin/maint
  origin/master
  origin/stable

配合-v，--verbose选项
$ git branch -rv  
  origin/HEAD   -> origin/master
  origin/maint  34acdd2 Fix ManifestParseError when first child node is comment
  origin/master da40341 manifest: Support a default upstream value
  origin/stable eceeb1b Support broken symlinks when cleaning obsolete paths
```
git branch -a, --all 显示远端本地分支列表
```bash
当前分支会标记个星号
$ git br -a  
* (HEAD detached at cf7c083)  # 这个表明当前是一个匿名分支。
  master
  new
  remotes/origin/HEAD -> origin/master
  remotes/origin/maint
  remotes/origin/master
  remotes/origin/stable

同样的配合-v选项一起的效果
$ git br -av 
* (HEAD detached at cf7c083) cf7c083 Download latest patch when no patch is specified
  master                     da40341 manifest: Support a default upstream value
  new                        da40341 manifest: Support a default upstream value
  remotes/origin/HEAD        -> origin/master
  remotes/origin/maint       34acdd2 Fix ManifestParseError when first child node is comment
  remotes/origin/master      da40341 manifest: Support a default upstream value
  remotes/origin/stable      eceeb1b Support broken symlinks when cleaning obsolete paths
```

git branch -v, -vv, --verbose 显示的信息更加详细
```bash
$ git br -v  
* (HEAD detached at cf7c083) cf7c083 Download latest patch when no patch is specified
  master                     da40341 manifest: Support a default upstream value
  new                        da40341 manifest: Support a default upstream value

使用-vv的效果
$ git br -vv  #
* (HEAD detached at cf7c083) cf7c083 Download latest patch when no patch is specified
  master                     da40341 [origin/master] manifest: Support a default upstream value
  new                        da40341 [origin/master] manifest: Support a default upstream value

使用--abbrev=15选项来指定显示的commit id的位数，默认是显示7位。 （abbrev是缩写的意思）
$ git br -v --abbrev=15  
* (HEAD detached at cf7c083) cf7c0834cfc24c5 Download latest patch when no patch is specified
  master                     da40341a3e6e2e4 manifest: Support a default upstream value
  new                        da40341a3e6e2e4 manifest: Support a default upstream value

使用--no-abbrev 显示完整的40位的commit id  
$  git br -v --no-abbrev 
* (HEAD detached at cf7c083) cf7c0834cfc24c5c9584695c657c6baf97d0fbb3 Download latest patch when no patch is specified
  master                     da40341a3e6e2e45877426aaefb97b3f0735a776 manifest: Support a default upstream value
  new                        da40341a3e6e2e45877426aaefb97b3f0735a776 manifest: Support a default upstream value
  
``` 

git branch --contains &lt;commit&gt;， --no-contains &lt;commit&gt;列出包含/不包含某个提交的分支列表
```bash

$ git branch --no-contains=7d52585  #<commit>参数没有默认是HEAD
  test
 
$ git branch --contains=7d52585     #<commit>参数没有默认是HEAD
* (HEAD detached at cf7c083)
  master
  new

$ git branch --no-contains  #<commit>参数没有默认是HEAD
  test
  
$ git branch --contains     #<commit>参数没有默认是HEAD
* (HEAD detached at cf7c083)
  master
  new
  

```

git branch --merged &lt;commit&gt;，--no-merged &lt;commit&gt;  
--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。  

```bash

$ git br --merged  #列出已经合并到当前分支的其他分支列表 <commit>参数没有默认是HEAD
* (HEAD detached at cf7c083)
  test

$ git br --no-merged 
  master
  new

The options --contains, --no-contains, --merged and --no-merged serve four related but different purposes:
·   --contains <commit> is used to find all branches which will need special attention if <commit> were to be rebased or amended, since those branches contain the specified <commit>.
·   --no-contains <commit> is the inverse of that, i.e. branches that don’t contain the specified <commit>.
·   --merged is used to find all branches which can be safely deleted, since those branches are fully contained by HEAD.
·   --no-merged is used to find branches which are candidates for merging into HEAD, since those branches are not fully contained by HEAD.
```



git branch --track | --no-track &lt;branchname&gt; &lt;start-point&gt;
```bash
专家一个本地分支，并且追踪远端master分支。
$ git br --track track-master origin/master
Branch track-master set up to track remote branch master from origin.
$ cat .git/config      
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://aosp.tuna.tsinghua.edu.cn/tools/repo
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "track-master"]
	remote = origin
	merge = refs/heads/master
	
如果配置了“git config branch.autoSetupMerge true”每次创建新分支的时候都会设置这个追踪分支的。（只有后面的参数<start-point>是一个远端分支的情况才会的）
$ git br mm origin/master
Branch mm set up to track remote branch master from origin.
$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://aosp.tuna.tsinghua.edu.cn/tools/repo
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "track-master"]
	remote = origin
	merge = refs/heads/master
[branch]
	autoSetupMerge = true
[branch "mm"]
	remote = origin
	merge = refs/heads/master

这种情况下可以使用--no-track选项改变这种情况
$ git br --no-track notrack-master origin/master
这样就不会设置”branch.<name>.remote“ 和 ”branch.<name>.merge“

也可以设置个追踪本地分支的一个分支，这个需要加上--track才行。
$ git br --track local-track-master track-master 
Branch local-track-master set up to track local branch track-master.
$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://aosp.tuna.tsinghua.edu.cn/tools/repo
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "track-master"]
	remote = origin
	merge = refs/heads/master
[branch]
	autoSetupMerge = true
[branch "mm"]
	remote = origin
	merge = refs/heads/master
[branch "local-track-master"]
	remote = .
	merge = refs/heads/track-master
```

git branch -l，--create-reflog    &lt;branchname&gt; &lt;start-point&gt;
```bash
$ git br -l test origin/maint
fatal: A branch named 'test' already exists.

$ git br -l test origin/maint -f 
Branch test set up to track remote branch maint from origin.

$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://aosp.tuna.tsinghua.edu.cn/tools/repo
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "test"]
	remote = origin
	merge = refs/heads/maint

```
git branch --set-upstream-to=&lt;upstream&gt;  &lt;branchname&gt;
```bash
设置某个本地分支<branchname>追踪某个<upstream>远端分支
省略参数  <branchname> 就会设置当前分支的

$ git br --set-upstream-to=origin/master
Branch test1 set up to track remote branch master from origin.
$ git brvv
  dev   34acdd2 Fix ManifestParseError when first child node is comment
  test  34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1 eceeb1b [origin/master: behind 87] Support broken symlinks when cleaning obsolete paths
  test2 f46902a forall: Clarify expansion of REPO_ environment values with -c
  test3 f46902a forall: Clarify expansion of REPO_ environment values with -c

$ git br --set-upstream-to=origin/master test3 
Branch test3 set up to track remote branch master from origin.
$ git brvv
  dev   34acdd2 Fix ManifestParseError when first child node is comment
  test  34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1 eceeb1b [origin/master: behind 87] Support broken symlinks when cleaning obsolete paths
  test2 f46902a forall: Clarify expansion of REPO_ environment values with -c
  test3 f46902a [origin/master: behind 18] forall: Clarify expansion of REPO_ environment values with -c
$

```
git branch --unset-upstream &lt;branchname&gt;   删除追踪关系，&lt;branchname&gt; 参数没有给出默认是当前分支名称。
```bash
$ git brvv   
  dev   34acdd2 Fix ManifestParseError when first child node is comment
  test  34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1 eceeb1b [origin/master: behind 87] Support broken symlinks when cleaning obsolete paths
  test2 f46902a forall: Clarify expansion of REPO_ environment values with -c
  test3 f46902a [origin/master: behind 18] forall: Clarify expansion of REPO_ environment values with -c
               
$ git br --unset-upstream dev
fatal: Branch 'dev' has no upstream information

$ git br --unset-upstream  test1

$ git br --unset-upstream  test2 
fatal: Branch 'test2' has no upstream information

$ git br --unset-upstream  test3

$ git brvv 
  dev   34acdd2 Fix ManifestParseError when first child node is comment
  test  34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1 eceeb1b Support broken symlinks when cleaning obsolete paths
  test2 f46902a forall: Clarify expansion of REPO_ environment values with -c
  test3 f46902a forall: Clarify expansion of REPO_ environment values with -c

```

git branch (-m | -M) &lt;oldbranch&gt; &lt;newbranch&gt; 重命名分支名称  
git branch -m，--move  
git branch --move --force  
```bash
$ git brvv
  dev   34acdd2 Fix ManifestParseError when first child node is comment
  test  34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1 eceeb1b Support broken symlinks when cleaning obsolete paths
  test2 f46902a forall: Clarify expansion of REPO_ environment values with -c
  test3 f46902a forall: Clarify expansion of REPO_ environment values with -c

$ git br -m test3 test33

$ git brvv
  dev    34acdd2 Fix ManifestParseError when first child node is comment
  test   34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
* test1  eceeb1b Support broken symlinks when cleaning obsolete paths
  test2  f46902a forall: Clarify expansion of REPO_ environment values with -c
  test33 f46902a forall: Clarify expansion of REPO_ environment values with -c

```
git branch (-c | -C) &lt;oldbranch&gt; &lt;newbranch&gt;  复制分支  
git branch -c，--copy  
git branch --copy --force  
```bash
这个好像在2.14版本中没有了

```


git branch (-d | -D) &lt;branchname&gt;…​ 删除分支  
git branch -d，--delete  
git branch --delete --force  
```bash

git branch -d test test1

git branch -D test33

如果带上-r选项 可以删除refs/remotes/下的分支，并不是远端服务器是的分支啊！！！
 
$ git br -r   # 列出目前所有的远端分支
  origin/HEAD -> origin/master
  origin/maint
  origin/master
  origin/stable

$ git br -d -r origin/HEAD      # 删除
Deleted remote-tracking branch origin/HEAD (was refs/remotes/origin/master).

$ git br -r   # 列出目前所有的远端分支
  origin/maint
  origin/master
  origin/stable

$ git br -d -r origin/maint
Deleted remote-tracking branch origin/maint (was 34acdd2).

$ git br -r    # 列出目前所有的远端分支
  origin/master
  origin/stable
```

批量删除某个目录下面的所有git仓库指定的匹配的某些分支
```bash

deleteBranches() {
    git -C "$1" symbolic-ref HEAD refs/heads/master
    a=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/msm8909\*)
      b=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/pollux\*)
      c=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/zs_msm8909\*)
    git -C "$1"  branch -D $a $b $c
}

export -f deleteBranches
find . -name "*.git" -type d -exec bash -c 'src="{}"; echo =$src=; deleteBranches $src' \; 

git branch -D $(git for-each-ref --format="%(refname:short)" refs/heads/\*_master)


cd  /home/gerrit2/review_site/git/git/android
#find . -name "*.git" -type d -exec bash -c 'src="{}"; echo =$src=; git -C $src tag|xargs git -C $src tag -d' \;
deleteBranches() {
    #git -C "$1" symbolic-ref HEAD refs/heads/master
    a=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/L\*)
    b=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/caf/**)
    c=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/aosp/**)
    d=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/aosp**)
    e=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/github**)
    f=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/kk\*)
    g=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/ics\*)
    h=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/jb\*)
    i=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/aosp-new/**)
    j=$(git -C "$1" for-each-ref --format="%(refname:short)" refs/heads/2017-spf-1-0_master_rebase_r00400.1\*)
    git -C "$1"  branch -D $a $b $c $d $e $f $g $h $i $j
}

export -f deleteBranches
find . -name "*.git" -type d -exec bash -c 'src="{}"; echo =$src=; deleteBranches $src' \; 

```

# git checkout
git checkout 命令用来切换分支，或者检出内容到工作目录
 
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

git checkout的通用使用格式 
```bash
git checkout [-q] [-f] [-m] [<branch>]
git checkout [-q] [-f] [-m] --detach [<branch>]
git checkout [-q] [-f] [-m] [--detach] <commit>
git checkout [-q] [-f] [-m] [[-b|-B|--orphan] <new_branch>] [<start_point>]
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <paths>…​
git checkout [<tree-ish>] [--] <pathspec>…​
git checkout (-p|--patch) [<tree-ish>] [--] [<paths>…​]

```

git checkout &lt;branch&gt; 切换分支   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
准备工作空间为给定的分支 &lt;branch&gt; 。同时更新index暂存区和working tree工作空间。然后把HEAD指向这个分支。  
本地被改动的文件会被保留。
```bash
当前我们在test1分支上，我们修改了color.py文件
$ git st
On branch test1
Changes not staged for commit:  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   color.py

no changes added to commit (use "git add" and/or "git commit -a")

然后我们试着切换分支到dev分支上，但是发现报错了，因为color.py 文件的改动和 分支 dev上的有冲突了。
$ git checkout dev 
error: Your local changes to the following files would be overwritten by checkout:
	color.py
Please commit your changes or stash them before you switch branches.
Aborting   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

这种情况下我们可以使用 -f选项，强制切换到dev分支，不过我们的改动会丢失。



我们在切换到另外一个分支test2，这个分支可以顺利切换
$ git checkout test2
M	color.py
Switched to branch 'test2'  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

$ git st 
On branch test2
Changes not staged for commit:  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   color.py

no changes added to commit (use "git add" and/or "git commit -a")
$                                               

```

git checkout -b &lt;branch&gt; --track &lt;remote&gt;/&lt;branch&gt; 创建一个新分支，并设置一个远端追踪关系。
```bash
我创建了新的分支，同时设置追踪一个远端分支origin/maint
$ git checkout -b track-maint --track origin/maint
Branch track-maint set up to track remote branch maint from origin.
Switched to a new branch 'track-maint'  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

$ git brvv 
  dev         34acdd2 Fix ManifestParseError when first child node is comment
* track-maint 34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment

$ cat .git/config  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

[core]
	repositoryformatversion = 0 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

	url = https://aosp.tuna.tsinghua.edu.cn/tools/repo
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "track-maint"]
	remote = origin
	merge = refs/heads/maint  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

git checkout -b|-B &lt;new_branch&gt; &lt;start point&gt;新建分支，并且检出到这个新分支上
```bash
以v1.0这个tag点创建一个新分支，请检出到这个分支上
$ git checkout -b  new-v1.0 v1.0
Switched to a new branch 'new-v1.0' 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
$ git brvv
  dev         34acdd2 Fix ManifestParseError when first child node is comment
* new-v1.0    cf31fe9 Initial Contribution
  track-maint 34acdd2 [origin/maint] Fix ManifestParseError when first child node is comment
  
使用-b选项时候，如果分支名称已经存在会报错的，这个时候可以使用-B选项强制创建  
$ git checkout -b  new-v1.0 v1.1
fatal: A branch named 'new-v1.0' already exists.
$ git checkout -B  new-v1.0 v1.1   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
Reset branch 'new-v1.0'

当前在一个匿名分支上，然后start point我使用origin/master试试
$ git brvv 
* (HEAD detached at v1.0) cf31fe9 Initial Contribution

$ git co -b track-master origin/master
Previous HEAD position was cf31fe9... Initial Contribution
Branch track-master set up to track remote branch master from origin.
Switched to a new branch 'track-master' 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
$ git brvv
* track-master da40341 [origin/master] manifest: Support a default upstream value
通过这个方式我们创建了一个追踪分支

默认是创建追踪分支的，如果不想可以使用--no-track选项。

 
```
git checkout --detach \[&lt;branch&gt;\]    切换/创建匿名分支，基于一个分支名  
git checkout \[--detach\] &lt;commit&gt;    切换/创建匿名分支，基于一个commit提交id。 
```bash
当前我们在new分支上
$ git brvv 
* new          02dbb6d Fix StopIteration exception during repo {sync,status}
  track-master da40341 [origin/master] manifest: Support a default upstream value

通过使用git checkout我们切换到 track-master 分支上。这个是不带--detach选项的行为。
$ git co track-master 
Switched to branch 'track-master'
Your branch is up-to-date with 'origin/master'.
 
$ git brvv 
  new          02dbb6d Fix StopIteration exception during repo {sync,status}
* track-master da40341 [origin/master] manifest: Support a default upstream value

下面我们带上--detach选项来切换到new分支上试试
$ git co --detach new
HEAD is now at 02dbb6d... Fix StopIteration exception during repo {sync,status}

$ git brvv                欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
* (HEAD detached at refs/heads/new) 02dbb6d Fix StopIteration exception during repo {sync,status}
  new                               02dbb6d Fix StopIteration exception during repo {sync,status}
  track-master                      da40341 [origin/master] manifest: Support a default upstream value
我们可以看到并没有切换到new分支上,而是以这个new分支为基准切换到了一个匿名分支上.

如果后面的参数是commitID,tag这个时候切换的是匿名分支,这个时候--detach选项可以省略.
$ git checkout da40341 
Previous HEAD position was 02dbb6d... Fix StopIteration exception during repo {sync,status}
HEAD is now at da40341... manifest: Support a default upstream value
 
$ git brvv               欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
* (HEAD detached at da40341) da40341 manifest: Support a default upstream value
  new                        02dbb6d Fix StopIteration exception during repo {sync,status}
  track-master               da40341 [origin/master] manifest: Support a default upstream value
$欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 
 
 

``` 

git checkout \[&lt;tree-ish&gt;\] \[--\] &lt;pathspec&gt;

git checkout \(-p|--patch\) \[&lt;tree-ish&gt;\] \[--\] \[&lt;pathspec&gt;\]
         
git checkout \[-f|--ours|--theirs|-m|--conflict=&lt;style&gt;\] \[&lt;tree-ish&gt;\] \[--\] &lt;paths&gt;
这几个命令是用 “index暂存区内容” 或者 “指定tree-ish” 来覆盖paths指定的文件，  
```bash
我们这个时候修改了color.py文件，但是我们想撤销修改，就可以使用git checkout了
HEAD detached at da40341
Changes not staged for commit:   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   color.py
	modified:   command.py

no changes added to commit (use "git add" and/or "git commit -a")
$ git co HEAD  -- color.py
$ git st  
HEAD detached at da40341
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   command.py    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

no changes added to commit (use "git add" and/or "git commit -a")

我们也可以指定个commit来做作为参数   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
$ git co a32c92c  -- command.py
$ git st

HEAD detached at da40341
nothing to commit, working tree clean
 
```
git checkout --orphan <new_branch> <start point>  创建一个孤儿分支
这个会基于指定的 <start point> 的代码，然后需要重新git commit，提交后会新建个分支<new_branch>，
这个分支和其他的分支没有任何关系联系。  
```bash
当前分支状态
$ git brvv                    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
* new          02dbb6d Fix StopIteration exception during repo {sync,status}
  track-master da40341 [origin/master] manifest: Support a default upstream value

我们基于v1.0这个点创建一个孤立分支orphan-branch 
$ git co --orphan orphan-branch v1.0 
Switched to a new branch 'orphan-branch'
这个时候我们看到所有的代码都是绿色的，表明都是在暂存区的。
$ git st  
On branch orphan-branch        欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
	new file:   COPYING
	new file:   Makefile
	new file:   codereview/__init__.py
	new file:   codereview/need_retry_pb2.py
	new file:   codereview/proto_client.py
	new file:   codereview/review_pb2.py
	new file:   codereview/upload_bundle_pb2.py
	new file:   color.py
	new file:   command.py
	new file:   editor.py
	new file:   error.py
	new file:   froofle/__init__.py
	new file:   froofle/protobuf/__init__.py
	new file:   froofle/protobuf/descriptor.py
	new file:   froofle/protobuf/descriptor_pb2.py
	new file:   froofle/protobuf/internal/__init__.py
	new file:   froofle/protobuf/internal/decoder.py
	new file:   froofle/protobuf/internal/encoder.py
	new file:   froofle/protobuf/internal/input_stream.py
	new file:   froofle/protobuf/internal/message_listener.py
	new file:   froofle/protobuf/internal/output_stream.py
	new file:   froofle/protobuf/internal/type_checkers.py
	new file:   froofle/protobuf/internal/wire_format.py
	new file:   froofle/protobuf/message.py
	new file:   froofle/protobuf/reflection.py
	new file:   froofle/protobuf/service.py
	new file:   froofle/protobuf/service_reflection.py
	new file:   froofle/protobuf/text_format.py
	new file:   gerrit_upload.py
	new file:   git_command.py
	new file:   git_config.py
	new file:   import_ext.py
	new file:   import_tar.py
	new file:   import_zip.py
	new file:   main.py
	new file:   manifest.py
	new file:   pager.py
	new file:   project.py
	new file:   remote.py
	new file:   repo
	new file:   subcmds/__init__.py
	new file:   subcmds/compute_snapshot_check.py
	new file:   subcmds/diff.py
	new file:   subcmds/forall.py
	new file:   subcmds/help.py
	new file:   subcmds/init.py
	new file:   subcmds/prune.py
	new file:   subcmds/stage.py
	new file:   subcmds/start.py
	new file:   subcmds/status.py
	new file:   subcmds/sync.py
	new file:   subcmds/upload.py 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
这个之后我们需要git commit 一下。
$ git ci -s -m 'init my orphan branch base on tag v1.0' 
[orphan-branch (root-commit) 139e9c1] init my orphan branch base on tag v1.0
 53 files changed, 11781 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 COPYING
 create mode 100644 Makefile
 create mode 100644 codereview/__init__.py
 create mode 100644 codereview/need_retry_pb2.py
 create mode 100755 codereview/proto_client.py
 create mode 100644 codereview/review_pb2.py
 create mode 100644 codereview/upload_bundle_pb2.py
 create mode 100644 color.py
 create mode 100644 command.py
 create mode 100644 editor.py
 create mode 100644 error.py
 create mode 100644 froofle/__init__.py
 create mode 100644 froofle/protobuf/__init__.py
 create mode 100644 froofle/protobuf/descriptor.py
 create mode 100644 froofle/protobuf/descriptor_pb2.py
 create mode 100644 froofle/protobuf/internal/__init__.py
 create mode 100644 froofle/protobuf/internal/decoder.py
 create mode 100644 froofle/protobuf/internal/encoder.py
 create mode 100644 froofle/protobuf/internal/input_stream.py
 create mode 100644 froofle/protobuf/internal/message_listener.py
 create mode 100644 froofle/protobuf/internal/output_stream.py
 create mode 100644 froofle/protobuf/internal/type_checkers.py
 create mode 100644 froofle/protobuf/internal/wire_format.py
 create mode 100644 froofle/protobuf/message.py
 create mode 100644 froofle/protobuf/reflection.py
 create mode 100644 froofle/protobuf/service.py
 create mode 100644 froofle/protobuf/service_reflection.py
 create mode 100644 froofle/protobuf/text_format.py
 create mode 100755 gerrit_upload.py
 create mode 100644 git_command.py
 create mode 100644 git_config.py
 create mode 100644 import_ext.py
 create mode 100644 import_tar.py
 create mode 100644 import_zip.py
 create mode 100755 main.py
 create mode 100644 manifest.py
 create mode 100755 pager.py
 create mode 100644 project.py
 create mode 100644 remote.py
 create mode 100755 repo
 create mode 100644 subcmds/__init__.py
 create mode 100644 subcmds/compute_snapshot_check.py
 create mode 100644 subcmds/diff.py
 create mode 100644 subcmds/forall.py
 create mode 100644 subcmds/help.py
 create mode 100644 subcmds/init.py
 create mode 100644 subcmds/prune.py
 create mode 100644 subcmds/stage.py
 create mode 100644 subcmds/start.py
 create mode 100644 subcmds/status.py
 create mode 100644 subcmds/sync.py
 create mode 100644 subcmds/upload.py  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

$ git st 
On branch orphan-branch
nothing to commit, working tree clean

git commit之后我们的孤立分支orphan-branch就新建好了
$ git brvv  
  new           02dbb6d Fix StopIteration exception during repo {sync,status}
* orphan-branch 139e9c1 init my orphan branch base on tag v1.0
  track-master  da40341 [origin/master] manifest: Support a default upstream value

$ git log   
commit 139e9c128630d6c7d8a01e2a35cde0f44190988d (HEAD -> orphan-branch)
Author: mamh <bright.ma@blackshark.com>
Date:   Thu Jun 14 12:58:04 2018 +0800

    init my orphan branch base on tag v1.0 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
    
    Signed-off-by: mamh <bright.ma@blackshark.com>
通过git log我们只看到一个提交                                            
```

git checkout --merge，-m 切换分支，采用合并，就是但你本地文件有修改情况下你要切换分支，有时候是不能切换的文件有冲突，  
这个时候可以使用--merge选项来合并当前分支，你的工作空间，你要切换的分支这3者。也就是这3者做一个三方合并的操作。  

```bash
$ git co track-master     欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
error: Your local changes to the following files would be overwritten by checkout:
	color.py
Please commit your changes or stash them before you switch branches.
Aborting
这个时候git checkout命令是拒绝你的切分支请求的

$ git co track-master --merge 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
M	color.py
Switched to branch 'track-master'
Your branch is up-to-date with 'origin/master'.

$ git st
On branch track-master
Your branch is up-to-date with 'origin/master'.

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	both modified:   color.py  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

no changes added to commit (use "git add" and/or "git commit -a")
```

```
$ git diff 
diff --cc color.py
index 0218aab,fd26ee0..0000000
--- a/color.py
+++ b/color.py
@@@ -17,92 -17,70 +17,112 @@@ import o
  import sys
  
  import pager
++<<<<<<< track-master       欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
 +
 +COLORS = {None: -1,
 +          'normal': -1,
 +          'black': 0,
 +          'red': 1,
 +          'green': 2,
 +          'yellow': 3,
 +          'blue': 4,
++=======欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
+ from git_config import GitConfig
+ 
+ COLORS = {None     :-1,
+           'normal' :-1,
+           'black'  : 0,
+           'red'    : 1,
+           'green'  : 2,
+           'yellow' : 3,
+           'blue'   : 4,
+           'yellow' : 3,
++>>>>>>> local    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
            'magenta': 5,
 -          'cyan'   : 6,
 -          'white'  : 7}
 -
 -ATTRS = {None     :-1,
 -         'bold'   : 1,
 -         'dim'    : 2,
 -         'ul'     : 4,
 -         'blink'  : 5,
 +          'cyan': 6,
 +          'white': 7}
 +
 +ATTRS = {None: -1,
 +         'bold': 1,
 +         'dim': 2,
 +         'ul': 4,
 +         'blink': 5,
           'reverse': 7}
  
  RESET = "\033[m"
+           'yellow' : 3,
  
++<<<<<<< track-master 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
...skipping...
 +      need_sep = True
 +
 +    if fg >= 0:
 +      if need_sep:
 +        code += ';'
 +      need_sep = True
 +
 +      if fg < 8:
 +        code += '3%c' % (ord('0') + fg)
 +      else:
 +        code += '38;5;%d' % fg
 +
 +    if bg >= 0:
 +      if need_sep:
 +        code += ';'
 +
 +      if bg < 8:
 +        code += '4%c' % (ord('0') + bg)
 +      else:
 +        code += '48;5;%d' % bg
 +    code += 'm'
 +  else:
 +    code = ''
 +  return code
 +
 +DEFAULT = None
 +
 +
 +def SetDefaultColoring(state):
 +  """Set coloring behavior to |state|.
 +
 +  This is useful for overriding config options via the command line.
 +  """
 +  if state is None:
 +    # Leave it alone -- return quick!
 +    return
 +
 +  global DEFAULT
 +  state = state.lower()
 +  if state in ('auto',):
 +    DEFAULT = state
 +  elif state in ('always', 'yes', 'true', True):
 +    DEFAULT = 'always'
 +  elif state in ('never', 'no', 'false', False):
 +    DEFAULT = 'never'
  
  
  class Coloring(object):

```

DETACHED HEAD 剥离的头指针，也就是匿名分支
```bash
HEAD一般的都是指向一个有名字的分支的（例如master），同时每个分支指向一个特定的commit。
       HEAD (refers to branch 'master')
        |
        v
a---b---c  branch 'master' (refers to commit 'c')
    ^
    |
  tag 'v2.0' (refers to commit 'b')欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

当有一个新的提交被创建了，分支也会更新指向新的commit上。同样的HEAD也还是指向master分支，master此时已经指向到新的提交上了。
$ edit; git add; git commit

           HEAD (refers to branch 'master')
            |
            v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

但是呢有时候检出一个特定的commit，不是一个命名分支也是很有用的。
这个时候特别注意HEAD指向了b这个提交上，这个时候就是一个“detached HEAD”的状态。
$ git checkout v2.0  # or  检出到b这个提交上
$ git checkout master^^

   HEAD (refers to commit 'b')
    |
    v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

这种情况下我们再新建个提交看看
$ edit; git add; git commit

     HEAD (refers to commit 'e')
      |
      v
      e
     /
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b')欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

这个使用有个新的提交e了，这个e只被HEAD引用，如果HEAD引用到其他地方，这个e就有可能丢失了。
下面我们在e提交的基础上再次提交个f提交
$ edit; git add; git commit

         HEAD (refers to commit 'f')
          |
          v
      e---f
     /
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b') 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 
这个时候如果我们切换到master分支上会怎么样？？？
$ git checkout master

               HEAD (refers to branch 'master')
      e---f     |
     /          v
a---b---c---d  branch 'master' (refers to commit 'd')
    ^
    |
  tag 'v2.0' (refers to commit 'b') 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
这个时候我们要特别注意，提交f没有被任何分支引用了。最终这个提交f会被git 垃圾回收机制给清理掉，到那之后就再也找不回f提交了。
也就是执行了git gc就会删除f提交，e提交等。除非我们在次之前建个引用指向这个提交f。
$ git checkout -b foo   (1)
$ git branch foo        (2)
$ git tag foo           (3)
(1)creates a new branch foo, which refers to commit f, and then updates HEAD to refer to branch foo. In other words, we’ll no longer be in detached HEAD state after this command.

(2)similarly creates a new branch foo, which refers to commit f, but leaves HEAD detached.

(3)creates a new tag foo, which refers to commit f, leaving HEAD detached.

匿名分支就讲解到这里  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

git checkout branchname  
如果branchname和某个文件名重复了，git是首先作为分支名称的，如果你确实要指定的文件名称可以使用'--'。

git checkout的一些例子
```bash
$ git checkout master             (1)    switch branch
$ git checkout master~2 Makefile  (2)     take a file out of another commit
$ rm -f hello.c
$ git checkout hello.c            (3)   restore hello.c from the index

$ git checkout -- '*.c'       check out all C source files out of the index
这个将会包所有的.c文件都检出了，文件hello.c 也会，这个不是看working tree下面是否有个这个hello.c
文件，而是看暂存区是否有个hello.c文件。

不幸的是如果你有个分支名称也是叫hello.c这个时候有可以产生疑惑了？到底是分支呢？还是文件呢？
我可以这样使用‘--’来明确说明是文件。
$ git checkout -- hello.c  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

# git reset
git reset 命令主要用来执行撤销操作  
--soft 会将HEAD引用指向给定提交。索引和工作目录的内容保持不变。撤销上次git commit操作的。变成绿色状态。  
--mixed 会将HEAD引用指向给定提交。索引内容也跟着改变以符合给定提交的树结构。但是工作目录的内容不变。默认是这个。变成红色状态。  
--hard 将HEAD引用指向给定提交，索引内容发生改变，工作目录内容也发生改变。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
取消暂存的文件,就是取消你git add 后的那个文件，就是把绿色的变成红色的

# git merge
git merge 工具用来合并一个或者多个分支到你已经检出的分支中。 然后它将当前分支指针移动到合并结果上。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
--ff  快进式的合并（fast-forward），  这时候只会更新分支的指向，不会创建一个"merge xxxx"这样的一个提交的。这个是默认行为。    
--no-ff 这个选项是 非快快进的，它总是会创建一个”merge xxx“这样的一个提交的。不管你是快进式，还是非快进式的。  
--ff-only  这个选项是只采用快进式的，如果不是快进式就会报错退出合并操作的。  
--squash, --no-squash              
git merge --abort #终止合并操作，恢复合并前的状态。  
git merge --continue # 继续合并操作  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git mergetool
当你在 Git 的合并中遇到问题时，可以使用 git mergetool 来启动一个外部的合并帮助工具。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git log
git log 命令用来展示一个项目的可达历史记录，从最近的提交快照起。     
默认情况下，它只显示你当前所在分支的历史记录，但是可以显示不同的甚至多个头记录或分支以供遍历。     
此命令通常也用来在提交记录级别显示两个或多个分支之间的差异。   
```bash
git log -p  #选项展开显示每次提交的内容差异，按补丁格式显示每个更新之间的差异。
git log -2  # 用 -2 则仅显示最近的两次更新
git log -p  --word-diff #--word-diff获取单词层面上的对比.
# 在程序代码中进行单词层面的对比常常是没什么用的。不过当你需要在书籍、论文这种很大的文本文件上进行对比的时候，这个功能就显出用武之地了。

git log --stat # --stat，仅显示简要的增改行数统计
git log --shortstat	 # 只显示 --stat 中最后的行数修改添加移除统计。

git log --name-only  #仅在提交信息后显示已修改的文件清单
git log --name-status#显示新增、修改、删除的文件清单

git log --abbrev-commit # 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。

git  log --oneline  # --pretty=oneline --abbrev-commit 的简化用法。


```

我们还可以对历史进行格式化输出，利用--pretty 选项。
```bash
git log --pretty[=<format>] # 其中<format>可以是oneline, short, medium, full, fuller, email, raw, format:<string>, tformat:<string>
# 默认是medium样式的输出。可以配置到gitconfig文件改变这个默认样式。
```

oneline的样式是
``` 
    <sha1> <title line>
```
short的样式是：
```
    commit <sha1>
    Author: <author>
    
    <title line>
```    
medium的样式是
```
    commit <sha1>
    Author: <author>
    Date:   <author date>
    
    <title line>
    
    <full commit message>    
```
full的样式是
```
    commit <sha1>
    Author: <author>
    Commit: <committer>
    
    <title line>
    
    <full commit message>
```
fuller的样式是
```
    commit <sha1>
    Author:     <author>
    AuthorDate: <author date>
    Commit:     <committer>
    CommitDate: <committer date>
    
    <title line>
    
    <full commit message>
```
email的样式是
```
    From <sha1> <date>
    From: <author>
    Date: <author date>
    Subject: [PATCH] <title line>
    
    <full commit message>

```
raw的样式
```text
    commit <sha1>
    tree <sha1>
    parent <sha1>
    author <author> 1528082498 +0800
    committer <date> 1528082498 +0800
    
        <full commit message>

```
format:<string>的，这种自定义样式更强大，功能更丰富。类似printf的格式化输出。
```text
# 例如： format:"The author of %h was %an, %ar%nThe title was >>%s<<%n"，其中使用%n来换行。

可以用的占位符有：
    ·   %H: commit hash  提交对象（commit）的完整哈希字串
    ·   %h: abbreviated commit hash 提交对象的简短哈希字串
    ·   %T: tree hash  树对象（tree）的完整哈希字串
    ·   %t: abbreviated tree hash 树对象的简短哈希字串
    ·   %P: parent hashes  父对象（parent）的完整哈希字串
    ·   %p: abbreviated parent hashes 父对象的简短哈希字串
    ·   %an: author name 作者（author）的名字
    ·   %aN: author name (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %ae: author email 作者的电子邮件地址
    ·   %aE: author email (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %ad: author date (format respects --date= option) 作者修订日期（可以用 -date= 选项定制格式）
    ·   %aD: author date, RFC2822 style
    ·   %ar: author date, relative  作者修订日期，按多久以前的方式显示
    ·   %at: author date, UNIX timestamp
    ·   %ai: author date, ISO 8601-like format
    ·   %aI: author date, strict ISO 8601 format
    ·   %cn: committer name  提交者(committer)的名字
    ·   %cN: committer name (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %ce: committer email 提交者的电子邮件地址
    ·   %cE: committer email (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %cd: committer date (format respects --date= option) 提交日期（可以用 -date= 选项定制格式）
    ·   %cD: committer date, RFC2822 style
    ·   %cr: committer date, relative  提交日期，按多久以前的方式显示
    ·   %ct: committer date, UNIX timestamp
    ·   %ci: committer date, ISO 8601-like format
    ·   %cI: committer date, strict ISO 8601 format
    ·   %d: ref names, like the --decorate option of git-log(1)
    ·   %D: ref names without the " (", ")" wrapping.
    ·   %e: encoding
    ·   %s: subject  提交说明
    ·   %f: sanitized subject line, suitable for a filename
    ·   %b: body
    ·   %B: raw body (unwrapped subject and body)
    ·   %N: commit notes
    ·   %GG: raw verification message from GPG for a signed commit
    ·   %G?: show "G" for a good (valid) signature, "B" for a bad signature, "U" for a good
             signature with unknown validity, "X" for a good signature that has expired, "Y" for a
             good signature made by an expired key, "R" for a good signature made by a revoked key, 
             "E" if the signature cannot be checked (e.g. missing key) and "N" for no signature
    ·   %GS: show the name of the signer for a signed commit
    ·   %GK: show the key used to sign a signed commit
    ·   %gD: reflog selector, e.g., refs/stash@{1} or refs/stash@{2 minutes ago}; the format follows 
             the rules described for the -g option. The portion before the @ is the refname
             as given on the command line (so git log -g refs/heads/master would yield refs/heads/master@{0}).
    ·   %gd: shortened reflog selector; same as %gD, but the refname portion is shortened for human readability
             (so refs/heads/master becomes just master).
    ·   %gn: reflog identity name
    ·   %gN: reflog identity name (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %ge: reflog identity email
    ·   %gE: reflog identity email (respecting .mailmap, see git-shortlog(1) or git-blame(1))
    ·   %gs: reflog subject
    ·   %Cred: switch color to red
    ·   %Cgreen: switch color to green
    ·   %Cblue: switch color to blue
    ·   %Creset: reset color
    ·   %C(...): color specification, as described under Values in the "CONFIGURATION FILE" section of git-config(1). 
                 By default, colors are shown only when enabled for log output
                 (by color.diff, color.ui, or --color, and respecting the auto settings of the former if we are going to a terminal).
                 %C(auto,...)  is accepted as a historical synonym for
                 the default (e.g., %C(auto,red)). Specifying %C(always,...) will show the colors even 
                 when color is not otherwise enabled (though consider just using `--color=always to
                 enable color for the whole output, including this format and anything else git might color).
                 auto alone (i.e.  %C(auto)) will turn on auto coloring on the next
                 placeholders until the color is switched again.
    ·   %m: left (<), right (>) or boundary (-) mark
    ·   %n: newline
    ·   %%: a raw %
    ·   %x00: print a byte from a hex code
    ·   %w([<w>[,<i1>[,<i2>]]]): switch line wrapping, like the -w option of git-shortlog(1).
    ·   %<(<N>[,trunc|ltrunc|mtrunc]): make the next placeholder take at least N columns, padding spaces on the 
                                        right if necessary. Optionally truncate at the beginning (ltrunc),
                                        the middle (mtrunc) or the end (trunc) if the output is longer than N columns.
                                        Note that truncating only works correctly with N >= 2.
    ·   %<|(<N>): make the next placeholder take at least until Nth columns, padding spaces on the right if necessary
    ·   %>(<N>), %>|(<N>): similar to %<(<N>), %<|(<N>) respectively, but padding spaces on the left
    ·   %>>(<N>), %>>|(<N>): similar to %>(<N>), %>|(<N>) respectively, except that if the next placeholder takes 
                             more spaces than given and there are spaces on its left, use those spaces
    ·   %><(<N>), %><|(<N>): similar to % <(<N>), %<|(<N>) respectively, but padding both sides (i.e. the text is centered)
    ·   %(trailers): display the trailers of the body as interpreted by git-interpret-trailers(1)           
需要注意的是有些占位符需要和某些选择配合使用的。              

```

git log  --graph，  用 oneline 或 format 时结合 --graph 选项，可以看到开头多出一些 ASCII 字符串表示的简单图形，形象地展示了每个提交所在的分支及其分化衍合情况
```text
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local


```

限制输出长度 --since
```text
-<n> 选项,后面跟上数字，显示最新的几个提交。
--since, --after  仅显示指定时间之后的提交。 
--until, --before仅显示指定时间之前的提交
--author 仅显示指定作者相关的提交。
--committer 仅显示指定提交者相关的提交。

$ git log --since=2.weeks     #

$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges -- t/

```


下面是定义的几个常用的git log的别名。
```text
    last = log -1 HEAD  
    lo   = log --oneline  
    ll   = log --graph --format=format:'%C(bold blue)%h%C(reset) %C(red)%<(100,trunc)%s%C(reset) %C(bold black)— %an%C(reset)%C(bold green)%d%C(reset) - %C(bold green)(%ar)%C(reset)' --abbrev-commit --date=relative --show-signature   

```

比较2个分支的差异的

```  
git lc   = log --left-right --cherry-pick --date=short --pretty='%m || %h ||  %<(120,trunc)%s (%<(10,trunc)%an) (%cd)'    
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

# git stash
```
git stash 命令用来临时地保存一些还没有提交的工作，以便在分支上不需要提交未完成工作就可以清理工作目录。  
git stash用于想要保存当前的修改,但是想回到之前最后一次提交的干净的工作仓库时进行的操作.git stash将本地的修改保存起来,并且将当前代码切换到HEAD提交上.
通过git stash存储的修改列表,可以通过git stash list查看.git stash show用于校验,git stash apply用于重新存储.直接执行git stash等同于git stash save.

最新的存储保存在refs/stash中.老的存储可以通过相关的参数获得,例如stash@{0}获取最新的存储,stash@{1}获取次新.stash@{2.hour.ago}获取两小时之前的.存储可以直接通过索引的位置来获得stash@{n}.


git stash   save [-p|--patch] [-k|--[no-]keep-index] [-u|--include-untracked] [-a|--all] [-q|--quiet] [<message>]

git stash   push [-p|--patch] [-k|--[no-]keep-index] [-u|--include-untracked] [-a|--all] [-q|--quiet] [-m|--message <message>] [--] [<pathspec>…​]

save和push命令都可以用于存储修改.并且将git的工作状态切回到HEAD也就是上一次合法提交上.后面的<message>是选填项.

save 子命令最后可以跟上一个描述信息。用push的话需要加上-m选项的。
push子命令最后可以跟上多个patspec。

--keep-index(简写为-k)只会存储为加入git管理的文件，未追踪的文件不会加入。
--include-untracked未追踪的文件也会被缓存,当前的工作空间会被恢复为完全清空的状态.
--all,那么除了未加入管理的文件,被git忽略(ignore)的文件也会被缓存.
 
 
git stash list 查看现有的储藏 
  
git stash apply 重新应用你刚刚实施的储藏 
 
git stash apply stash@{2}。指定应用第二个。如果你不指明，Git 默认使用最近的储藏并尝试应用它.  

git stash drop，加上你希望移除的储藏的名字

git stash show 展示存储单元和最新提交的diff结果.如果没有给定<stash>参数时,会对比最新的存储单元.

git stash pop 移除单个存储单元.和git stash save的作用相反  
```
开发到一半,同步远端代码
```

当你的开发进行到一半,但是代码还不想进行提交 ,然后需要同步去关联远端代码时.如果你本地的代码和远端代码没有冲突时,
可以直接通过git pull解决.但是如果可能发生冲突怎么办.直接git pull会拒绝覆盖当前的修改.

遇到这种情况,需要先保存本地的代码,进行git pull,然后再pop出本地代码:
```


工作流被打断,需要先做别的需求
```

当开发进行到一半,老板过来跟你说"线上有个bug,你现在给我改好,不然扣你鸡腿".当然,你可以开一个新的分支,
把当前代码提交过去,回头再merge,具体代码如下 
繁琐的工作流示例
# ... hack hack hack ...
 git checkout -b my_wip
 git commit -a -m "WIP"
 git checkout master
 edit emergency fix
 git commit -a -m "Fix in a hurry"
 git checkout my_wip
 git reset --soft HEAD^
# ... continue hacking ...

我们可以通过git stash来简化这个流程
正确姿势
# ... hack hack hack ...
 git stash        //保存开发到一半的代码
 edit emergency fix
 git commit -a -m "Fix in a hurry"
 git stash pop   //将代码追加到最新的提交之后
# ... continue hacking ...

 


 
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```


#git worktree
```bash
管理多个工作目录的一个命令

git worktree add [-f] [--detach] [--checkout] [--lock] [-b <new-branch>] <path> [<commit-ish>]
git worktree list [--porcelain]
git worktree lock [--reason <string>] <worktree>
git worktree move <worktree> <new-path>
git worktree prune [-n] [-v] [--expire <expire>]
git worktree remove [-f] <worktree>
git worktree unlock <worktree> 



这个命令和git stash有相似的使用场景。当工作被打断了，有不想取储存，就可以使用这个新建个干净的工作目录。
You are in the middle of a refactoring session and your boss comes in and demands that you fix something immediately. You might typically use git-stash(1) to store your changes away temporarily, however, your working
tree is in such a state of disarray (with new, moved, and removed files, and other bits and pieces strewn around) that you don’t want to risk disturbing any of it. Instead, you create a temporary linked working tree to
make the emergency fix, remove it when done, and then resume your earlier refactoring session.

$ git worktree add -b emergency-fix ../temp master
$ pushd ../temp
# ... hack hack hack ...
$ git commit -a -m 'emergency fix for boss'
$ popd
$ rm -rf ../temp
$ git worktree prune


```


# git tag
git tag 命令用来为代码历史记录中的某一个点指定一个永久的书签。  
git tag -l  
git tag -d  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fetch
git fetch 命令与一个远程的仓库交互，并且将远程仓库中有但是在当前仓库的没有的所有信息拉取下来然后存储在你本地数据库中。
git fetch origin   

--all 下载所有的远程仓库  
-p, --prune 删除本地所有追踪分支，对应的服务器上的分支不存在的追踪分支。  
-n, --no-tags 不下载标签  
-t, --tags 下载所有标签  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


引用规格的格式由一个可选的 + 号和紧随其后的 <src>:<dst> 组成，  
其中 <src> 是一个模式（pattern），代表远程版本库中的引用；  
<dst> 是那些远程引用在本地所对应的位置。  
+ 号告诉 Git 即使在不能快进的情况下也要（强制）更新引用。  

默认情况下，引用规格由 git remote add 命令自动生成， Git 获取服务器中 refs/heads/ 下面的所有引用，  
并将它写入到本地的 refs/remotes/origin/ 中。 所以，如果服务器上有一个 master 分支，我们可以在本地通过下面这种方式来访问该分支上的提交记录：  
```
$ git log origin/master
$ git log remotes/origin/master
$ git log refs/remotes/origin/master

# 上面的三个命令作用相同，因为 Git 会把它们都扩展成 refs/remotes/origin/master。

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

如果想让 Git 每次只拉取远程的 master 分支，而不是所有分支，可以把（引用规格的）获取那一行修改为：
```
fetch = +refs/heads/master:refs/remotes/origin/master


欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
当然你也可以直接在git fetch 后面跟上,你也可以指定多个引用规格。 在命令行中，你可以按照如下的方式拉取多个分支：

```
$ git fetch origin master:refs/remotes/origin/mymaster

$ git fetch origin master:refs/remotes/origin/mymaster topic:refs/remotes/origin/topic
From git@github.com:schacon/simplegit
! [rejected]        master     -> origin/mymaster  (non fast forward)
* [new branch]      topic      -> origin/topic
#在这个例子中，对 master 分支的拉取操作被拒绝，因为它不是一个可以快进的引用。 我们可以通过在引用规格之前指定 + 号来覆盖该规则。


欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

你也可以在配置文件中指定多个用于获取操作的引用规格。 如果想在每次获取时都包括 master 和 experiment 分支，添加如下两行：
```
[remote "origin"]
    url = https://github.com/schacon/simplegit-progit
    fetch = +refs/heads/master:refs/remotes/origin/master
    fetch = +refs/heads/experiment:refs/remotes/origin/experiment

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

我们不能在模式中使用部分通配符，所以像下面这样的引用规格是不合法的：
```
fetch = +refs/heads/qa*:refs/remotes/origin/qa*  # 这个不对的，不能两边都是星号的.这个在高版本git中可以用
fetch = +refs/heads/*:refs/remotes/origin/qa*   # 不过可以这样，这样把服务器的分支都下载本地，本地都以qa开头。

fetch = +refs/heads/qa/*:refs/remotes/origin/qa/* # 这样的也是可以的,heads后面可以跟任意多层的，就像路径那样的层次结构一样

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

# git pull
git pull 命令基本上就是 git fetch 和 git merge 命令的组合体，Git 从你指定的远程仓库中抓取内容，然后马上尝试将其合并进你所在的分支中。  
git pull 等价于 git fetch  然后 git merge FETCH_HEAD  
git pull --rebase  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/



# git push
git push origin HEAD:master  
git push origin HEAD:refs/heads/master  
git push git@192.168.1.106:mage/my-git-project.git  HEAD:refs/heads/dev  
git push origin HEAD:refs/for/master  
git push origin HEAD:refs/tags/v1.0  
git push origin master:refs/heads/master  
git push origin refs/heads/master:refs/heads/master  
git push origin master:refs/heads/qa/master  
git push origin :topic 删除  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git remote
git remote 命令是一个是你远程仓库记录的管理工具。   
它允许你将一个长的 URL 保存成一个简写的句柄，例如 origin ，这样你就可以不用每次都输入他们了。   
你可以有多个这样的句柄，git remote 可以用来添加，修改，及删除它们。  
git remote add <name> <url>   
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git archive
git archive 命令用来创建项目一个指定快照的归档文件。  
我们在 准备一次发布 一节中，使用 git archive 命令来创建一个项目的归档文件用于分享。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git submodule
git submodule 命令用来管理一个仓库的其他外部仓库。 它可以被用在库或者其他类型的共享资源上。  
submodule 命令有几个子命令, 如（add、update、sync 等等）用来管理这些资源。  
git submodule update --remote 来从上游拉取新工作  
git submodule update --remote --rebase  
参考：https://git-scm.com/book/zh/v2/Git-工具-子模块   
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git show
git show 命令可以以一种简单的人类可读的方式来显示一个 Git 对象。 你一般使用此命令来显示一个标签或一个提交的信息。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```bash
git show [<options>] <object>...

#这里的<object>可以是tree,blob,commit,tag

git cat-file -p HEAD^{tree} # 得到当前HEAD对应的tree-blob结构
git show blobID #显示文件内容
git show -s --pretty=raw  commitID # 显示commit的具体信息
git cat-file tag <TAG> 显示tag的具体信息
```

这个命令和git log命令有很多重复的地方，重复的选项等。


git show --notes,--no-notes
```bash

$ git config --add remote.origin.fetch +refs/notes/*:refs/notes/*
$ git config notes.displayRef refs/notes/*
配置了上面的选项会下载到 notes的信息，一般是review的相关信息

$ git show
commit 9a460a3af12a150192c84f568c28e78bc256cabc (HEAD, changes/66/83266/1)
Author: 马哥私房菜 <bright.ma@马哥私房菜.com>
Date:   Tue Jun 12 19:33:15 2018 +0800

    blackshark: add platform/vendor/zui/app/BSAMAgent
    
    Change-Id: I6d632133aaf2ae5cdc99df56f2e499443caf24d2
    Signed-off-by: 马哥私房菜 <bright.ma@马哥私房菜.com>

Notes (review):
    Code-Review+1: Jira <jira@马哥私房菜.com>
    Code-Review+2: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Verified+1: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Submitted-by: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Submitted-at: Tue, 12 Jun 2018 19:34:07 +0800
    Reviewed-on: http://gerrit.马哥私房菜.com:8080/83266
    Project: git/android/platform/manifest
    Branch: refs/heads/bs_master

如果没有配置“$ git config notes.displayRef refs/notes/*”这个，
默认git show不会显示notes信息的，可以带上--notes选项来使用。
$ git show --notes=refs/notes/review  
commit 9a460a3af12a150192c84f568c28e78bc256cabc (HEAD, changes/66/83266/1)
Author: 马哥私房菜 <bright.ma@马哥私房菜.com>
Date:   Tue Jun 12 19:33:15 2018 +0800

    blackshark: add platform/vendor/马哥私房菜/zui/app/BSAMAgent
    
    Change-Id: I6d632133aaf2ae5cdc99df56f2e499443caf24d2
    Signed-off-by: 马哥私房菜 <bright.ma@马哥私房菜.com>

Notes (review):
    Code-Review+1: Jira <jira@马哥私房菜.com>
    Code-Review+2: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Verified+1: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Submitted-by: 马哥私房菜 <bright.ma@马哥私房菜.com>
    Submitted-at: Tue, 12 Jun 2018 19:34:07 +0800
    Reviewed-on: http://gerrit.马哥私房菜.com:8080/83266
    Project: git/android/platform/manifest
    Branch: refs/heads/bs_master

使用--no-notes选项可以不显示notes信息。
$ git show --no-notes 
```
git show
```bash
#什么参数都加 是显示HEAD指向的那个提交信息的
$ git --no-pager show   
commit da40341a3e6e2e45877426aaefb97b3f0735a776 (HEAD -> master, origin/master, origin/HEAD, new)
Author: Nasser Grainawi <nasser@codeaurora.org>
Date:   Fri May 4 12:53:29 2018 -0600

    manifest: Support a default upstream value
    
    It's convenient to set upstream for all projects in a manifest instead of
    repeating the same value for each project.
    
    Change-Id: I946b1de4efb01b351c332dfad108fa7d4f443cba

diff --git a/docs/manifest-format.txt b/docs/manifest-format.txt
index 56bdf14..0c957dd 100644
--- a/docs/manifest-format.txt
+++ b/docs/manifest-format.txt
@@ -44,6 +44,7 @@ following DTD:
     <!ATTLIST default remote      IDREF #IMPLIED>
     <!ATTLIST default revision    CDATA #IMPLIED>
     <!ATTLIST default dest-branch CDATA #IMPLIED>
+    <!ATTLIST default upstream    CDATA #IMPLIED>
     <!ATTLIST default sync-j      CDATA #IMPLIED>
     <!ATTLIST default sync-c      CDATA #IMPLIED>
     <!ATTLIST default sync-s      CDATA #IMPLIED>
@@ -164,6 +165,11 @@ Project elements not setting their own `dest-branch` will inherit
 this value. If this value is not set, projects will use `revision`
 by default instead.
 
+Attribute `upstream`: Name of the Git ref in which a sha1
+can be found.  Used when syncing a revision locked manifest in
+-c mode to avoid having to sync the entire ref space. Project elements
+not setting their own `upstream` will inherit this value.
+
 Attribute `sync-j`: Number of parallel jobs to use when synching.
 
 Attribute `sync-c`: Set to true to only sync the given Git
diff --git a/manifest_xml.py b/manifest_xml.py
index 60d6116..d0211ea 100644
--- a/manifest_xml.py
+++ b/manifest_xml.py
@@ -59,6 +59,7 @@ class _Default(object):
 
   revisionExpr = None
   destBranchExpr = None
+  upstreamExpr = None
   remote = None
   sync_j = 1
   sync_c = False
@@ -230,6 +231,9 @@ class XmlManifest(object):
     if d.destBranchExpr:
       have_default = True
       e.setAttribute('dest-branch', d.destBranchExpr)
+    if d.upstreamExpr:
+      have_default = True
+      e.setAttribute('upstream', d.upstreamExpr)
     if d.sync_j > 1:
       have_default = True
       e.setAttribute('sync-j', '%d' % d.sync_j)
@@ -295,7 +299,8 @@ class XmlManifest(object):
         revision = self.remotes[p.remote.orig_name].revision or d.revisionExpr
         if not revision or revision != p.revisionExpr:
           e.setAttribute('revision', p.revisionExpr)
-        if p.upstream and p.upstream != p.revisionExpr:
+        if (p.upstream and (p.upstream != p.revisionExpr or
+                            p.upstream != d.upstreamExpr)):
           e.setAttribute('upstream', p.upstream)
 
       if p.dest_branch and p.dest_branch != d.destBranchExpr:
@@ -694,6 +699,7 @@ class XmlManifest(object):
       d.revisionExpr = None
 
     d.destBranchExpr = node.getAttribute('dest-branch') or None
+    d.upstreamExpr = node.getAttribute('upstream') or None
 
     sync_j = node.getAttribute('sync-j')
     if sync_j == '' or sync_j is None:
@@ -830,7 +836,7 @@ class XmlManifest(object):
 
     dest_branch = node.getAttribute('dest-branch') or self._default.destBranchExpr
 
-    upstream = node.getAttribute('upstream')
+    upstream = node.getAttribute('upstream') or self._default.upstreamExpr
 
     groups = ''
     if node.hasAttribute('groups'):
                     

```

git show -s, --no-patch 不显示提交更改的内容  
默认是显示更改内容的，可以通过-p, -u, --patch选项来改变这个选项的行为。


```bash
$ git --no-pager show  --pretty=raw -s  
commit cf7c0834cfc24c5c9584695c657c6baf97d0fbb3
tree 49be7db9b2f7b0e0cf9026a84b81716ddfc66034
parent 4ea1f0cabdda28fdee837ee2f99e14375028b5f4
author Akshay Verma <akshayverma948@gmail.com> 1521131190 +0530
committer Akshay Verma <akshayverma948@gmail.com> 1521284363 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58
$      

```
git show --name-only，--name-status这个和git log 中的一样
```bash
$ git show --name-only 
commit cf7c0834cfc24c5c9584695c657c6baf97d0fbb3 (HEAD)
Author: Akshay Verma <akshayverma948@gmail.com>
Date:   Thu Mar 15 21:56:30 2018 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58

project.py
subcmds/download.py

$ git show --name-status
commit cf7c0834cfc24c5c9584695c657c6baf97d0fbb3 (HEAD)
Author: Akshay Verma <akshayverma948@gmail.com>
Date:   Thu Mar 15 21:56:30 2018 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58

M       project.py
M       subcmds/download.py

```
git show --abbrev-commit，--no-abbrev-commit 是显示40位commitid还是显示简短的commitid。
```bash
使用 --abbrev-commit 选项显示简短的
$ git show --abbrev-commit -s  
commit cf7c083 (HEAD)
Author: Akshay Verma <akshayverma948@gmail.com>
Date:   Thu Mar 15 21:56:30 2018 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58

同时可以使用--abbrev=3来决定显示几位commit id。
$ git show --abbrev-commit -s --abbrev=3  
commit cf7c (HEAD)
Author: Akshay Verma <akshayverma948@gmail.com>
Date:   Thu Mar 15 21:56:30 2018 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58
 
$ git show --abbrev-commit -s --abbrev=15   
commit cf7c0834cfc24c5 (HEAD)
Author: Akshay Verma <akshayverma948@gmail.com>
Date:   Thu Mar 15 21:56:30 2018 +0530

    Download latest patch when no patch is specified
    
    When someone does "repo download -c <project> <change>"
    without specifying a patch number, by default patch 1 is
    downloaded. An alternative is to look for the latest patch
    and download the same when no explicit patch is given.
    This commit does the same by identifying the latest patch
    using "git ls-remote".
    
    Change-Id: Ia5fa7364415f53a3d9436df4643e38f3c90ded58

使用了--no-abbrev-commit选项就会显示完整的40位的commit id了。
 
```

git show --oneline   显示为一行，和git log的一样效果。如果同时使用了-s那就是和git log --oneline -1 等价效果了。
```bash
$ git show --oneline 
cf7c083 (HEAD) Download latest patch when no patch is specified
diff --git a/project.py b/project.py
old mode 100644
new mode 100755
index e4682e6..5297a5c
--- a/project.py
+++ b/project.py
@@ -2270,6 +2270,16 @@ class Project(object):
       if self._allrefs:
         raise GitError('%s cherry-pick %s ' % (self.name, rev))
 
+  def _LsRemote(self):
+    cmd = ['ls-remote']
+    p = GitCommand(self, cmd, capture_stdout=True)
+    if p.Wait() == 0:
+      if hasattr(p.stdout, 'decode'):
+        return p.stdout.decode('utf-8')
+      else:
+        return p.stdout
+    return None
+
   def _Revert(self, rev):
     cmd = ['revert']
     cmd.append('--no-edit')
diff --git a/subcmds/download.py b/subcmds/download.py
old mode 100644
new mode 100755
index e1010aa..384af78
--- a/subcmds/download.py
+++ b/subcmds/download.py
@@ -62,6 +62,14 @@ If no project is specified try to use current directory as a project.
           ps_id = int(m.group(2))
         else:
           ps_id = 1
+          regex = r'refs/changes/%2.2d/%d/(\d+)' % (chg_id % 100, chg_id)
+          output = project._LsRemote()
+          if output:
+            rcomp = re.compile(regex, re.I)
+            for line in output.splitlines():
+              match = rcomp.search(line)
+              if match:
+                ps_id = max(int(match.group(1)), ps_id)
         to_get.append((project, chg_id, ps_id))
       else:
         project = self.GetProjects([a])[0]

如果同时使用了 -s选项
$ git show -s --oneline  
cf7c083 (HEAD) Download latest patch when no patch is specified
这个时候和git log 的效果一样了
$ git log --oneline -1 
cf7c083 (HEAD) Download latest patch when no patch is specified
```

git show --pretty 和 git log的一样。



# git shortlog
git shortlog 是一个用来归纳 git log 的输出的命令。 它可以接受很多与 git log 相同的选项，
但是此命令并不会列出所有的提交，而是展示一个根据作者分组的提交记录的概括性信息。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git describe
git describe 命令用来接受任何可以解析成一个提交的东西，然后生成一个人类可读的字符串且不可变。 
这是一种获得一个提交的描述的方式，它跟一个提交的 SHA-1 值一样是无歧义，但是更具可读性。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git bisect
git bisect 工具是一个非常有用的调试工具，它通过自动进行一个二分查找来找到哪一个特定的提交是导致 bug 或者问题的第一个提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


# git blame
git blame 命令标注任何文件的行，指出文件的每一行的最后的变更的提交及谁是那一个提交的作者。 当你要找那个人去询问关于这块特殊代码的信息时这会很有用。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git grep
git grep 命令可以帮助在源代码中，甚至是你项目的老版本中的任意文件中查找任何字符串或者正则表达式。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git cherry-pick
git cherry-pick 命令用来获得在单个提交中引入的变更，然后尝试将作为一个新的提交引入到你当前分支上。  
从一个分支单独一个或者两个提交而不是合并整个分支的所有变更是非常有用的。  
git cherry-pick --ff  
git cherry-pick --continue  
git cherry-pick --abort  
git cherry-pick --quit  
git cherry-pick c6ade72c576^..b299ad990db 一个连续的提交记录,基线升级使用。https://blog.csdn.net/mmh19891113/article/details/79258738  
git cherry-pick start^..end  表示包括 start，结束到end。都是一个闭区间。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git rebase
git rebase 命令基本是是一个自动化的 cherry-pick 命令。 它计算出一系列的提交，然后再以它们在其他地方以同样的顺序一个一个的 cherry-picks 出它们。  
git rebase -i 交互式的，可以合并多个提交为一个，可以调整提交的前后顺序等  
git rebase --onto  基线升级使用  
git rebase  --onto  new   start_point   end_point   
git rebase --continue | --skip | --abort | --quit  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git revert
git revert 命令本质上就是一个逆向的 git cherry-pick 操作。  
它将你提交中的变更的以完全相反的方式的应用到一个新创建的提交中，本质上就是撤销或者倒转。  
git revert --continue  
git revert --quit  
git revert --abort                                       
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```bash
git revert [--[no-]edit] [-n] [-m parent-number] [-s] [-S[<keyid>]] <commit>...
git revert --continue
git revert --quit
git revert --abort

```
-e, --edit进入到编辑界面，可以再次编辑提交信息。--no-edit
```bash
git revert --edit HEAD~3

$ git revert --no-edit 96ed6df cdd80fa ef9faab                                                                                      
[test d9bb334] Revert "touch c"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 c
[test ba0ff99] Revert "touch e"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 e
[test 0c19967] Revert "update b file"
 1 file changed, 1 deletion(-)
$ git st                                                                                                                            
On branch test
nothing to commit, working tree clean
$ git ll                                                                                                                           
* 0c19967 - Revert "update b file"                                                                                                   — SCM life (HEAD -> test) - (3 seconds ago)
* ba0ff99 - Revert "touch e"                                                                                                         — SCM life - (3 seconds ago)
* d9bb334 - Revert "touch c"                                                                                                         — SCM life - (3 seconds ago)
* ef9faab - update b file                                                                                                            — SCM life - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)
$                     


在revert多个提交的时候如果没有联系，这个revert的顺序可以变的。如果代码是有联系的revert顺序变了会有冲突了。
$ git revert --no-edit ef9faab  96ed6df cdd80fa                                                                                     
[test 952062a] Revert "update b file"
 1 file changed, 1 deletion(-)
[test 20aa374] Revert "touch c"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 c
[test dc48716] Revert "touch e"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 e
$ git ll                                                                                                                            
* dc48716 - Revert "touch e"                                                                                                         — SCM life (HEAD -> test) - (3 seconds ago)
* 20aa374 - Revert "touch c"                                                                                                         — SCM life - (3 seconds ago)
* 952062a - Revert "update b file"                                                                                                   — SCM life - (3 seconds ago)
* ef9faab - update b file                                                                                                            — SCM life - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git
```

```bash
$ git ll                                                                                                                            [mamh@10.0.63.43 ] 18-06-19 13:57  /home/mamh/tmp/myconfig
* ef9faab - update b file                                                                                                            — SCM life (HEAD -> test) - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)

这个范围写反是不对的 
$ git revert --no-edit 5211c8d..96ed6df  
error: empty commit set passed
fatal: revert failed

一次revert多个提交，可以是一个连续的提交区间，这里加上--no-edit选项就不用每次都进入到vim的编辑提交信息的界面了。 
$ git revert --no-edit 96ed6df..5211c8d                                                                                             
[test a8b374e] Revert "update a file"
 1 file changed, 1 deletion(-)
[test d1cf5e7] Revert "touch e"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 e
[test 48b7718] Revert "touch d"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 d
$ git ll                                                                                                                            
* 48b7718 - Revert "touch d"                                                                                                         — SCM life (HEAD -> test) - (3 seconds ago)
* d1cf5e7 - Revert "touch e"                                                                                                         — SCM life - (3 seconds ago)
* a8b374e - Revert "update a file"                                                                                                   — SCM life - (3 seconds ago)
* ef9faab - update b file                                                                                                            — SCM life - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)
```
                                                                                                                                   
-n, --no-commit，只是revert一个提交而不进行commit，这是文件状态是绿色的。
```
下面我们试试这个-n选项的效果
$ git ll                                                                                                                            
* ef9faab - update b file                                                                                                            — SCM life (HEAD -> test) - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)
$                                                                                                                                   
$  git revert -n HEAD~5..HEAD~2                                                                                                     
到这里git revert执行成功了 

通过git log看到并没有产生revert的那个提交
$ git ll                                                                                                                            
* ef9faab - update b file                                                                                                            — SCM life (HEAD -> test) - (2 days ago)
* 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
* cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
* 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
* 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
* f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
* a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)

通过git status我们查看工作区和暂存区的状态，发现3个删除状态的文件，这个3个文件就是我们命令参数中的3个提交加上的文件，这里是revert操作那就是要删除这3个文件了
$ git st                                                                                                                            
On branch test
You are currently reverting commit 96ed6df.
  (all conflicts fixed: run "git revert --continue")
  (use "git revert --abort" to cancel the revert operation)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    c
	deleted:    d
	deleted:    e

                                                                                                                                   
```

-m parent-number, --mainline parent-number当revert一个merge的提交需要加上
```bash
git revert -m 2 xxx

$ cd tmp/myconfig                                                                                                                                
$ git ll                                                                                                                            
*   d322685 - Merge branch 'test' into empty                                                                                           — SCM life (HEAD -> empty) - (2 days ago)
|\  
| * ef9faab - update b file                                                                                                            — SCM life (test) - (2 days ago)
| * 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
| * cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
| * 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
| * 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
| * f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
| * a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* | d826f8a - update file a                                                                                                            — SCM life - (2 days ago)
* | 01b8625 - touch a                                                                                                                  — SCM life - (2 days ago)
|/  
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)
$                                                                                                                                   
$                                                                                                                                   
$ git revert d322685                                                                                                                
error: commit d3226856737d20b4945ef6ff3454f093fe851052 is a merge but no -m option was given.
fatal: revert failed
这个时候不使用-m选项会报错的，这个提交有2个父提交，我们要通过-m来指定哪个父提交。

$ git revert -m 3 d322685                                                                                                           
error: commit d3226856737d20b4945ef6ff3454f093fe851052 does not have parent 3
fatal: revert failed

$ git revert -m 2 d322685                                                                                                           
[empty 56049dc] Revert "Merge branch 'test' into empty"
 1 file changed, 1 insertion(+), 8 deletions(-)
$ git ll                                                                                                                            
* 56049dc - Revert "Merge branch 'test' into empty"                                                                                  — SCM life (HEAD -> empty) - (5 seconds ago)
*   d322685 - Merge branch 'test' into empty                                                                                           — SCM life - (2 days ago)
|\  
| * ef9faab - update b file                                                                                                            — SCM life (test) - (2 days ago)
| * 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
| * cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
| * 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
| * 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
| * f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
| * a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* | d826f8a - update file a                                                                                                            — SCM life - (2 days ago)
* | 01b8625 - touch a                                                                                                                  — SCM life - (2 days ago)
|/  
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)
$                                                                                                                                   
 
 
如果是-m 1这个时候revert的 “a2b661e - touch a”  到 “ef9faab - update b file” 这几个提交的改动
$ git revert -m 1 d322685                                                                                                           
[empty 03da93b] Revert "Merge branch 'test' into empty"
 4 files changed, 1 deletion(-)
 delete mode 100644 b
 delete mode 100644 c
 delete mode 100644 d
 delete mode 100644 e
$ git ll                                                                                                                            
* 03da93b - Revert "Merge branch 'test' into empty"                                                                                  — SCM life (HEAD -> empty) - (4 seconds ago)
*   d322685 - Merge branch 'test' into empty                                                                                           — SCM life - (2 days ago)
|\  
| * ef9faab - update b file                                                                                                            — SCM life (test) - (2 days ago)
| * 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
| * cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
| * 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
| * 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
| * f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
| * a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* | d826f8a - update file a                                                                                                            — SCM life - (2 days ago)
* | 01b8625 - touch a                                                                                                                  — SCM life - (2 days ago)
|/  
* f7277c4 - init empty git                                                                                                                                    
```

```bash
我们重置回原来
$ git reset --hard d322685                                                                                                          
HEAD is now at d322685 Merge branch 'test' into empty
$ git ll                                                                                                                            
*   d322685 - Merge branch 'test' into empty                                                                                           — SCM life (HEAD -> empty) - (2 days ago)
|\  
| * ef9faab - update b file                                                                                                            — SCM life (test) - (2 days ago)
| * 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
| * cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
| * 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
| * 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
| * f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
| * a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* | d826f8a - update file a                                                                                                            — SCM life - (2 days ago)
* | 01b8625 - touch a                                                                                                                  — SCM life - (2 days ago)
|/  
* f7277c4 - init empty git 

此时看看b文件的内容
$ cat b                                                                                                                             
bbb

我们可以看到这一行是在 ef9faabedab94293708588d93e4b6e8a6a697a3a 这个提交上加上的
$ git show ef9faab                                                                                                                  
commit ef9faabedab94293708588d93e4b6e8a6a697a3a (test)
Author: SCM life <scm.life@scmlife.com>
Date:   Sun Jun 17 12:35:10 2018 +0800

    update b file
    
    Change-Id: Ic28a9e9434fe4cbc1d95ec4436289d9008689c31
    Signed-off-by: SCM life <scm.life@scmlife.com>

diff --git a/b b/b
index e69de29..f761ec1 100644
--- a/b
+++ b/b
@@ -0,0 +1 @@
+bbb

这个时候我们revert这个提交 
$ git revert ef9faab                                                                                                                
[empty a1cfaec] Revert "update b file"
 1 file changed, 1 deletion(-)

$ git st                                                                                                                            
On branch empty
nothing to commit, working tree clean

$ git ll                                                                                                                            
* a1cfaec - Revert "update b file"                                                                                                   — SCM life (HEAD -> empty) - (5 seconds ago)
*   d322685 - Merge branch 'test' into empty                                                                                           — SCM life - (2 days ago)
|\  
| * ef9faab - update b file                                                                                                            — SCM life (test) - (2 days ago)
| * 5211c8d - update a file                                                                                                            — SCM life - (2 days ago)
| * cdd80fa - touch e                                                                                                                  — SCM life - (2 days ago)
| * 34260fc - touch d                                                                                                                  — SCM life - (2 days ago)
| * 96ed6df - touch c                                                                                                                  — SCM life - (2 days ago)
| * f3480c7 - touch b                                                                                                                  — SCM life - (2 days ago)
| * a2b661e - touch a                                                                                                                  — SCM life - (2 days ago)
* | d826f8a - update file a                                                                                                            — SCM life - (2 days ago)
* | 01b8625 - touch a                                                                                                                  — SCM life - (2 days ago)
|/  
* f7277c4 - init empty git                                                                                                           — Minghui Ma - (12 days ago)

这个时候revert之后我们再看这个b文件，这个文件内容已经没有了。
$ cat b                                                                                                                             

```

# git apply
git apply 命令应用一个通过 git diff 或者甚至使用 GNU diff 命令创建的补丁。 它跟补丁命令做了差不多的工作，但还是有一些小小的差别。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git am
git am 命令用来应用来自邮箱的补丁。特别是那些被 mbox 格式化过的。 这对于通过邮件接受补丁并将他们轻松地应用到你的项目中很有用。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git format-patch
git format-patch 命令用来以 mbox 的格式来生成一系列的补丁以便你可以发送到一个邮件列表中。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git imap-send
git imap-send 将一个由 git format-patch 生成的邮箱上传至 IMAP 草稿文件夹。 
我们在 通过邮件的公开项目 一节中见过一个通过使用 git imap-send 工具向一个项目发送补丁进行贡献的例子。

# git send-email
git send-mail 命令用来通过邮件发送那些使用 git format-patch 生成的补丁。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git request-pull
git request-pull 命令只是简单的用来生成一个可通过邮件发送给某个人的示例信息体。 如果你在公共服务器上有一个分支，
并且想让别人知道如何集成这些变更，而不用通过邮件发送补丁，你就可以执行此命令的输出发送给这个你想拉取变更的人。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git svn
git svn 可以使 Git 作为一个客户端来与 Subversion 版本控制系统通信。
这意味着你可以使用 Git 来检出内容，或者提交到 Subversion 服务器。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fast-import
对于其他版本控制系统或者从其他任何的格式导入，你可以使用 git fast-import 快速地将其他格式映射到 Git 可以轻松记录的格式。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git gc
git gc 命令在你的仓库中执行 “garbage collection” ，删除数据库中不需要的文件和将其他文件打包成一种更有效的格式。
此命令一般在背后为你工作，虽然你可以手动执行它-如果你想的话。    
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fsck
git fsck 命令用来检查内部数据库的问题或者不一致性。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git reflog
git reflog 命令分析你所有分支的头指针的日志来查找出你在重写历史上可能丢失的提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git filter-branch
git filter-branch 命令用来根据某些规则来重写大量的提交记录，
例如从任何地方删除文件，或者通过过滤一个仓库中的一个单独的子目录以提取出一个项目。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git中2个点和3个点区别
What are the differences between double-dot “..” and triple-dot “…” in Git commit ranges?

https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#Commit-Ranges

对于在git log中使用double dot和triple dot的区别：  

对于2个分支A和B来说：  
```bash
git log A..B    # 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这个会显示B分支有的，而A分支没有的那些提交，下面这个图片可以清楚的显示出来

![Android下的配置管理之道之git的使用_1.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_1.png)




对于2个分支A和B来说： 
```bash
git log A...B   # 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这个将会显示A分支有的B分支没有的， B分支有的A分支没有的。用下面的图清楚的表示  

![Android下的配置管理之道之git的使用_2.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_2.png)


这个3个点的非常有用，经常会配合--lfet-right来一起用
```bash

$ git log --oneline --decorate --left-right --graph master...origin/master
< 1794bee (HEAD, master) Derp some more
> 6e6ce69 (origin/master, origin/HEAD) Add hello.txt

```
上面就是比较2个分支的差异，小于号开头的行是表示master分支有的提交，大于号表示origin/master分支的提交



```bash

$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right --boundary                  
< 036a649 (shgit/dev_20180319) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
< 547f251 [SHAR-995][BSP] Disable ext4 barrier
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
| > 89f5290 (HEAD, dev_20171101) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
| > 29b877a [SHAR-995][BSP] Disable ext4 barrier
|/  
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
通过这个命令我们可以看到左右2个分支之间的差异，使用--left-right选项，小于号开始的表示是shgit/dev_20180319有的提交，  
大于号开始的表示shgit/dev_20171101分支的提交。最后一行表示他们的共同祖先提交。这个是加了--boundary的效果。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  


```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right --boundary   --cherry-pick 
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
同样的这个是加了  --cherry-pick选项，我们发现少了几行，这个选项会认为通过cherr-pick的提交是一样的提交，会过滤掉这些提交的。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 



```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right   --cherry-pick 
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
```
这个是去掉 --boundary  选项，就是不显示他们的起始点那个提交。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
 

```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-only --boundary 
* 036a649 (shgit/dev_20180319) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
* 547f251 [SHAR-995][BSP] Disable ext4 barrier
* 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
--left-only 选项只会显示左边那个分支的提交就是只显示shgit/dev_20180319分支的提交。  
同样的他们的共同起始提交是最后一行那个。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 



```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101  --right-only --boundary   
* 89f5290 (HEAD, shgit/dev_20171101) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
* 29b877a [SHAR-995][BSP] Disable ext4 barrier
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
--right-only 选项只会显示右边那个分支的提交，就是只显示shgit/dev_20171101分支的提交。  
同样的他们的共同起始提交是最后一行那个。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 




![Android下的配置管理之道之git的使用_5.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_5.png)






对于在git diff中使用double dot和triple dot的区别：  

```
$ git diff topic master    (1)      #欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
$ git diff topic..master   (2)  
$ git diff topic...master  (3)  
```
1.Changes between the tips of the topic and the master branches.  
2.Same as above.  
3.Changes that occurred on the master branch since when the topic branch was started off it. 
第一个和第二个一样，都是比较topic 分支到 master分支在那个最后一个提交处的代码的差异。  
第三个就是比较两个分支共同起始点那个提交开始，到master结束的那个提交 的差异。  
我们给个图片就清楚可以看到了： 
![Android下的配置管理之道之git的使用_3.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_3.png)

git diff topic..master  等价 git diff topic master      #欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
git diff topic...master 等价 git diff $(git merge-base topic master) master     
![Android下的配置管理之道之git的使用_4.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_4.png)


# git中HEAD^ 和HEAD~ 区别

What's the difference between HEAD^ and HEAD~ in git

```bash
       <rev>^, e.g. HEAD^, v1.5.1^0   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           A suffix ^ to a revision parameter means the first parent of that commit object.  ^<n> means the <n>th parent (i.e.  <rev>^ is equivalent to <rev>^1). As a special rule,
           <rev>^0 means the commit itself and is used when <rev> is the object name of a tag object that refers to a commit object.

```
```bash

       <rev>~<n>, e.g. master~3   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           A suffix ~<n> to a revision parameter means the commit object that is the <n>th generation ancestor of the named commit object, following only the first parents. I.e.  <rev>~3
           is equivalent to <rev>^^^ which is equivalent to <rev>^1^1^1. See below for an illustration of the usage of this form.

```

类似这样的一个符号的 ～  和  ^  都是表示父提交，  
两个符号 ～～  和 ^^ 都是表示的爷爷提交。  

他们的不同是体现在后面跟上了数字。  
～2 表示的是上面2层。这个是在层级上的。~2 means up two levels in the hierarchy。  
^2 表示的第二个父提交。一个^ 表示第一个父提交，两个^表示第二个父提交。^2 means the second parent where a commit has more than one parent (i.e. because it's a merge)  
这2个符号也可以组合起来使用的。HEAD~2^3 means HEAD's grandparent commit's third parent commit  


下面我们看一个提交树状图。从下往上，从左往右。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```bash

       Here is an illustration, by Jon Loeliger. Both commit nodes B and C are parents of commit node A. Parent commits are ordered left-to-right.

           G   H   I   J
            \ /     \ /
             D   E   F
              \  |  / \
               \ | /   |
                \|/    |
                 B     C
                  \   /
                   \ /
                    A

           A =      = A^0
           B = A^   = A^1     = A~1    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           C = A^2  = A^2              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           D = A^^  = A^1^1   = A~2    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           E = B^2  = A^^2              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           F = B^3  = A^^3              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           G = A^^^ = A^1^1^1 = A~3     欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           H = D^2  = B^^2    = A^^^2  = A~2^2  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           I = F^   = B^3^    = A^^3^    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           J = F^2  = B^3^2   = A^^3^2   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```




















