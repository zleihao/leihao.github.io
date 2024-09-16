+++
title = 'Git常用操作'
date = 2024-09-16T19:29:07+08:00
draft = false
+++

## 一、前期准备

### 1、用户配置

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

### 2、**文本编辑器**

当 Git 需要你输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑。

如果你想使用不同的文本编辑器，例如 Emacs，可以这样做：

```bash
git config --global core.editor emacs
```

在 Windows 系统上，如果你想要使用别的文本编辑器，**那么必须指定可执行文件的完整路径**。 它可能随你的编辑器的打包方式而不同。

```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

更多编辑器配置见：https://git-scm.com/book/zh/v2/附录-C%3A-Git-命令-设置与配置#ch_core_editor

在windows上也可以使用 gvim：

```bash
git config --global core.editor "C:/Vim/vim90/gvim.exe"
```

## 二、本地仓库相关操作

### 1、初始化本地仓库

新建一个文件夹 git_study，进到该文件夹中，使用命令 **git init** 进行初始化本地仓库：

```bash
mkdir git_study
cd git_study
git init
```

执行完后，会在该目录下生成一个 .git 目录。

### 2、查看仓库状态

```bash
git status
```

### 3、版本回退

```bash
git reset <command> <commit>
```

1. **--mixed**

   ```bash
   git reset --mixed <commit> (默认选项)
   ```

   移动 HEAD 指针到指定的提交，并**重置暂存区**，但不改变工作目录的内容。

   修改的文件保留在工作目录中，需要重新添加到暂存区。

   这个选项常用于取消暂存的文件，重新添加需要提交的文件。

   ```bash
   git reset HEAD^ 或 git reset --mixed HEAD^
   //**将索引重置为当前 HEAD 的状态，但保留工作区的更改。这允许你取消暂存的文件**
   ```

2. **--soft**

   ```bash
   git reset --soft <commit>
   ```

   移动 HEAD 指针到指定的提交，但不改变暂存区和工作目录的内容。

   修改的文件仍然保留在**暂存区**中，可以直接提交。

   这个选项常用于撤销之前的提交，但保留修改内容以便重新提交。

3. **--hard**

   ```bash
   git reset --hard <commit>
   ```

   移动 HEAD 指针到指定的提交，并且重置暂存区和工作目录的内容。

   修改的文件在工作目录中会被完全删除，慎用！

   这个选项常用于完全撤销之前的提交，回到指定提交时的状态。

### 4、恢复暂存区文件

```bash
git restore --staged .
```

这将移除所有文件的暂存状态，并将它们恢复到工作目录。

### 5、创建分支

1. 从某个提交（commit）创建一个新的分支

   ```c
   git branch <new-branch-name> <commit>
   ```

- <new-branch-name> 是你想要创建的新分支的名称。
- <commit> 是要基于的提交的标识符（提交哈希、分支名或标签名）。

### 6、cherry-pick

`git cherry-pick`命令的作用，就是将指定的提交（commit）应用于其他分支。

```bash
git cherry-pick <command> <commit>
```

1. **`-n`** 或 **`--no-commit`**

   ```bash
   git cherry-pick -n <commit>
   ```

   只执行 **`git cherry-pick`** 的操作，但不自动提交结果。这个选项允许你在应用了提交后进行其他修改或调整，然后手动提交。

2. **`-e`** 或 **`--edit`**

   ```bash
   git cherry-pick -e <commit>
   ```

   启动编辑器，允许对提交消息(commit)进行编辑或修改。

3. **`-x`**

   ```bash
   git cherry-pick -x <commit>
   ```

   在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。

4. **`-s` 或 `--signoff`**

   ```bash
   git cherry-pick -s <commit>
   ```

   在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。

### 7、查看仓库版本号

- 查看上个版本

  ```bash
  git rev-parse HEAD
  ```

- 查看指定个数上个版本

  ```bash
  git rev-parse HEAD~n   //n代表个数，1 2 3...
  ```

### 8、查看远程仓库地址

```bash
git remote -v
```

### 9、对比不同分支下的文件

- 对比不同分支下的同一个文件

  ```bash
  git diff branch1:file.txt branch2:file.txt
  ```

- 对比不同分支下的文件

  ```bash
  git diff branch1 branch2
  ```

- 以文件夹的形式对比不同分支下的文件

  ```bash
  git difftool branch1..branch2 /path/to/folder
  ```

### 10、查看提交记录

1. 查看某分支最初的提交

   ```bash
   git log --reverse --pretty=oneline  | tail -n 1
   ```

2. 查看仓库提交日志并以树状图显示

- 标注每个提交所在的分支

  ```bash
  git log --graph --oneline --decorate --all
  ```

- 默认显示

  ```bash
  git log --graph
  ```

3. 显示指定分支和其他分支的提交历史

   ```bash
   git show-branch
   ```

4. 查看分支从哪个分支拉出来的

   `````bash
   git reflog show <branch name>
   `````
