title: git 常用命令总结
---

我通常都是用github做远程仓库提交和同步我的代码。这里总结一些常用的git命令，以后再慢慢补充。

## 初始仓库
在github上初始化一个仓库并提交代码通常有两种方法。两种方法都需要先在github上面建立仓库（repository）。
建立好了远程仓库怎么从本地提交代码呢？有两种方法，第一种，先在本地clone远程仓库。
``` bash
$git clone https://github.com/your-username/your-repository-name.git
```
使用这个命令可以把远程仓库clone到你的本地，然后本地的仓库里面就有了这个仓库的信息，在你的本地进行修改，再提交就可以了。

第二种方法是，在你的本地的某个目录下：
``` bash
$git init //初始化一个git仓库
$git add //一个你要提交的文件或文件夹
$git remote add origin https://github.com/your-username/your-repository-name.git //由于本地仓库并不知道你的远程仓库在哪里，需要设置这个信息
$git push --set-upstream origin master //把push的upstream设置为origin master
```
通过这两种方法设置都可以把远程仓库和本地仓库同步。接下来需要提交你的修改

## 提交修改
在你已经修改了文件或者文件夹之后怎么提交到本地的版本和远程的仓库呢？
``` bash
$git status //查看修改状态
$git add filename（or folder name） //把修改添加到cache中（并没有进入版本库）
$git commit -m "some-massege" //把所有add后的文件提交到本地的版本库
$git push //把这次本地的版本库push到远程仓库里面
```

## 同步修改
从远程仓库同步版本到本地比较简单
``` bash
$git pull
```
## .gitignore
如果有些文件不想放在版本控制下面怎么办呢？先编写一个.gitignore文件，把需要忽略的文件或者文件夹描述在这个文件里面，比如：
db.json  //某个文件
*.log    //所有log文件
node_modules/  //node_module文件夹下的所有文件
等等

## 分支的操作
查看所有分支
``` bash
$git branch -A
```
切换到某个分支
``` bash
$git checkout branch-name
```
在当前分支下，合并某个分支
``` bash
$git merge branch-name
```