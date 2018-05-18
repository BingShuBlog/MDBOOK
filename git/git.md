# Git 学习笔记

## 安装 Git

### CentOS 7

#### 用 Yum 安装

```
$ sudo yum install git
```

命令执行完成并且没有报错，表示安装完成。

用以下命令进行验证：

```
$ git --version
```

### 配置 Git

Git 安装完成后我们需要进行一些配置，比如个人信息等。这些配置可以用 git config 命令完成。

```
$ git config --global user.name "Your Name"
$ git config --global user.email "your@example.com"
```

使用 git config --list 命令查看配置是否生效

```
$ git config --list
```

```
user.name=Your Name
user.email=you@example.com
```

出于个有习惯把 Git 默认编辑器改成 vim

```
$ git config --global core.editor vim
```

## 一般用法

### 创建本地库

1. 在工程目录下运行下面命令，来初始化本地库

```
git init
```

1. 添加目录到本地库

```
git add src/
```

1. 提交操作到本地库

```
git commit -a
```

1. 添加远程库

```
git remote add origin [remote-repository-url]
```

1. 将代码同步到远程库中

```
git push origin master
```

## 远程仓库相关命令

### 检出仓库：

```
$ git clone git://github.com/jquery/jquery.git
```

### 查看远程仓库：

```
$ git remote -v
```

### 添加远程仓库：

```
$ git remote add [name] [url]
```

### 删除远程仓库：

```
$ git remote rm [name]
```

### 修改远程仓库：

```
$ git remote set-url --push [name][newUrl]
```

### 拉取远程仓库：

```
$ git pull [remoteName] [localBranchName]
```

### 推送远程仓库：

```
$ git push -u [remoteName] [localBranchName]
```

## 分支(branch)操作相关命令

### 查看本地分支：

```
$ git branch
```

### 查看远程分支：

```
$ git branch -r
```

### 创建本地分支：

```
$ git branch [BranchName] -- 注意新分支创建后不会自动切换为当前分支
```

### 切换分支：

```
$ git checkout [BranchName]
```

### 创建新分支并立即切换到新分支：

```
$ git checkout -b [BranchName]
```

### 删除本地分支：

```
$ git branch -d [BranchName] -- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用 -D 选项
```

### 删除远程分支

```
$ git push --delete origin [BranchName]
```

### 重命名本地分支

```
$ git branch -m [OldBranchName] [NewBranchName]
```

### 合并分支：

```
$ git merge [BranchName] -- 将名称为[BranchName]的分支合并到当前分支
```

### 创建远程分支(或本地分支push到远程)：

```
$ git push origin [BranchName]
```

## 操作文件相关命令

### 删除 git repository 中的文件

```
git rm --cached [file-name]
```

## 注释规范

一般情况下，提交 GIT 时的注释可以分成几类，可以用几个动词开始：

```
Added ( 新加入的需求 )
Fixed ( 修复 bug )
Changed ( 完成的任务 )
Updated ( 完成的任务，或者由于第三方模块变化而做的变化 )
```

尽量将注释缩减为一句话，不要包含详细的内容。 假如有 Issues 系统，其中可以包含 Issue 的 ID。比如：Issue #123456 包含作者的信息。比如 by lynn 完整例子：

```
    git commit -m 'Issue #[issue number] by [username]: [Short summary of the change].'
```

## 提交部分代码

### 场景

修改了三个文件，分别涉及到二个功能，所其中两个文件当做一个功能修改的 commit 来提交，另一个则需要作为另一个功能修改的 commit。 如果使用 git add . 当前所有修改都会被加入到 commit。 解决方法：使用 git stash 命令。

### 步聚

1. 使用 git add 命令添加第一功能修改需要 commit 的文件

```
$ git add file1
$ git add file2
```

1. 隐藏其他修改，git stash 的参数中 -k 开关告诉仓库保持文件的完整 -u 开关告诉仓库包括无路径的文件(那些新的和未添加到git的) 这时git status就只能看到file1 和file2，并且当你切换到实际的文件目录，file3 的修改也随之不见。

```
$ git stash -k -u
```

### commit 第一个修改

```
$ git commit -m "commit function1"
```

1. 恢复之前隐藏的修改,这时再git status，file3的修改又回来。

```
$ git stash pop
```

1. commit 第二个修改

```
$ git add .
$ git commit -m "commit function2"
```