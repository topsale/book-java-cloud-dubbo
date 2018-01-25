# Git 基本操作

---

## 获取与创建项目命令

### git init

用 git init 在目录中创建新的 Git 仓库。 你可以在任何时候、任何目录中这么做，完全是本地化的。

```
git init
```

### git clone

使用 git clone 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改。

```
git clone [url]
```

## 基本快照

### git add

git add 命令可将该文件添加到缓存

```
git add <filename>
```

### git status

git status 以查看在你上次提交之后是否有修改。

```
git status
git status -s
```

### git diff

执行 git diff 来查看执行 git status 的结果的详细信息。

git diff 命令显示已写入缓存与已修改但尚未写入缓存的改动的区别。git diff 有两个主要的应用场景。

* 尚未缓存的改动：git diff
* 查看已缓存的改动： git diff --cached
* 查看已缓存的与未缓存的所有改动：git diff HEAD
* 显示摘要而非整个 diff：git diff --stat

### git commit

使用 git add 命令将想要快照的内容写入缓存区， 而执行 git commit 将缓存区内容添加到仓库中。

Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步需要配置用户名和邮箱地址。

```
git config --global user.name 'yourname'
git config --global user.email youremail
```

将文件写入缓存区并提供提交注释

```
git commit -m 'update message'
```

### git reset HEAD

git reset HEAD 命令用于取消已缓存的内容。

```
git reset HEAD -- <filename>
```

## 拉取与推送

### git pull

git pull命令用于从另一个存储库或本地分支获取并集成(整合)。git pull命令的作用是：取回远程主机某个分支的更新，再与本地的指定分支合并，它的完整格式稍稍有点复杂。

```
git pull <远程主机名> <远程分支名>:<本地分支名>
```

将远程存储库中的更改合并到当前分支中。在默认模式下，`git pull`是`git fetch`后跟`git merge FETCH_HEAD`的缩写。更准确地说，`git pull`使用给定的参数运行`git fetch`，并调用`git merge`将检索到的分支头合并到当前分支中。 

### git push

`git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相似。

```
git push <远程主机名> <本地分支名>:<远程分支名>
```