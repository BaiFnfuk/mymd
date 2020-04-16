## git

###  设置用户信息

``` shell
=> git config --global user.name "your name"
=> git config --global user.email "your email"
```



### 设置文本编辑器

``` shell
=> git config --global core.editor "'E:/software/notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```



### 检查配置信息

``` shell
=> git config --list
```



###  初始化仓库

``` shell
=> cd gitdemo
=> git init
Initialized empty Git repository in E:/gitdemo/.git/
```



### 查看工作区文件状态

``` shell
=> git status .
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.c

nothing added to commit but untracked files present (use "git add" to track)

```

**untracked files 下的文件是没有被跟踪的文件，也就是没有git add的文件**

**changes to be committed下的文件是已经git add放到暂存区的文件，但是还没有提交**

**changes not staged for commit下的文件是修改了已经提交过的文件，还没有放到暂存区**



### 查看工作区文件状态的简略信息

```shell
=> git status -s
MM hello.c
```



### 将文件添加到暂存区

``` shell
=> git add hello.c
warning: LF will be replaced by CRLF in hello.c.
The file will have its original line endings in your working directory

```



### 提交

``` shell
=> git commit -m "add a hello.c file"
[master (root-commit) 2af7b5d] add a hello.c file
 1 file changed, 7 insertions(+)
 create mode 100644 hello.c

```



### 查看以前的提交纪录

``` shell
=> git log
commit 2af7b5d6e6b2075b4220f0a1b144310d13999207 (HEAD -> master)
Author: bai <15950140@qq.com>
Date:   Mon Apr 13 22:02:32 2020 +0800

    add a hello.c file

```



### 以补丁的形式查看历史提交

```shell
=> git log --patch
commit ea0f87cc25f3d8764ee0ba7c12cda11c111dda03 (HEAD -> master)
Author: bai <15950140@qq.com>
Date:   Mon Apr 13 22:50:11 2020 +0800

    add a CONTRIBUTING.md file

diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
new file mode 100644
index 0000000..170a64e
--- /dev/null
+++ b/CONTRIBUTING.md
@@ -0,0 +1 @@
+现在的暂存区已经准备就绪，可以提交了。 在此之前，请务必确认还有什么已修改或新建的文件还没有 git add 过， 否则提交的时候不会记录这些尚未暂存的变化。 这些已修改
但未暂存的文件只会保留在本地磁盘。 所以，每次准备提交前，先用 git status 看下，
你所需要的文件是不是都已暂存起来了， 然后再运行提交命令 git commit

```





### 查看当前工作区与某次提交时的差异

``` shell
=> git diff2af7b5d6e hello.c
warning: LF will be replaced by CRLF in hello.c.
The file will have its original line endings in your working directory
diff --git a/hello.c b/hello.c
index 962b5a6..102e0c9 100644
--- a/hello.c
+++ b/hello.c
@@ -2,6 +2,11 @@

 int main(int argv, char *args[])
 {
+       int i;
+       // 打印运行程序时的参数
+       for (i = 0; i < argv; i++){
+               printf("num = %d, val = %s\n", i, *(args + i));
+       }
        printf("Hello World!\n");
        return 0;
 }

```



### 查看已暂存和未暂存的修改

```shell
=> git diff
warning: LF will be replaced by CRLF in hello.c.
The file will have its original line endings in your working directory
diff --git a/hello.c b/hello.c
index ac0a705..102e0c9 100644
--- a/hello.c
+++ b/hello.c
@@ -3,6 +3,7 @@
 int main(int argv, char *args[])
 {
        int i;
+       // 打印运行程序时的参数
        for (i = 0; i < argv; i++){
                printf("num = %d, val = %s\n", i, *(args + i));
        }


```



### 忽略文件

``` shell
=> vim .gitignore
# 忽略所有以.o或.a结尾的文件
*.[oa]

# 忽略所有以~结尾的文件
*~

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf

```



### 退回到上一次提交

```shell
=> git reset --hard HEAD^
HEAD is now at c991b34 修改CONTRIBUTING.md文件
```

**HEAD^**：HEAD代表当前提交，HEAD^代表上一次提交，以此类推HEAD^^表示上上次提交



### 退回到某一次提交

```shell
=> git reset --hard ea0f87cc2
```



###  将暂存区文件回到工作区

``` shell
=> git reset hello.c
```



### 撤消工作区的修改

```shell
=> git checkout hello.c
```

例：如果修改了hello.c文件，并执行了git add hello.c将文件加入到暂存区。这时想撤消hello.c的修改，要先将hello.c从暂存区放出来到工作区，再将hello.c的修改内容撤消。

所以要执行下面的两条命令

``` shell
=> git reset hello.c
=> git checkout hello.c
```



### 删除文件

``` shell
=> git rm README
```



### 修改文件名或移动文件

```shell
=> git mv README README.md
```



### 创建分支

```shell
=> git branch dev
```



### 查看分支

```shell
=> git branch
```



### 切换分支

```shell
=> git checkout dev
```



## github

### 查看远程仓库

```shell
=> git remote -v
```



### 添加远程仓库

```shell
=> git remote add 远程连接名称 url
例： git remote add origin https://github.com/baifnfuk/gitdemo
```

### 强制合并本地仓库与远程仓库

```shell
=> git pull origin master --allow-unrelated-histories

# origin是远程连接的名字
# master是本地仓库的分支名
```

### 从远程仓库更新代码

```shell
=> git pull origin master
# 或者
=> git fetch origin
=> git merge origin/master master
# git fetch origin 只会把内容下载到本地，并不会主动合并分支，所以要要自己合并分支才会显示最新的下载内容
```

### 将本地仓库更新到远程仓库

```shell
=> git push origin master
```

### 查看标签

```shell
=> git tag
```



