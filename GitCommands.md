## Git Commands

### Git Config 指令

- `/etc/gitconfig`: 这个不常用，主要是设置给多用户操作系统，针对不同用户，统一更改参数的。
- `~/.gitconfig`: 用户自定义git参数，可以通过指令修改该用户所有repo的参数。
- `.git/config`: 是每个仓库的配置参数的地方。

#### 实例

```bash
#查看全局git所有配置
git config --global --list
or 
git config --global -l 

#如何分别显示，系统，本地和用户的git配置
git config --list --local  # 本地仓库
git config --list --system # 操作系统
git config --list --global # 用户配置


#修改全局Git的某个配置
git config --global user.name "Frank Wang"

# 设置参与开发人员的邮件和姓名
git config --global user.email "xuegangwang@amazon.com"
```

### Git Three States

> Git has three main states that your files can reside in: Committed, modified, and staged. 
>
> __Committed__: the data is safely stored in your local database.
>
> __Modified__: you have changed the file but have not committed it to your local database yet. 
>
> __Staged__: marked the modified file in its current version. 

- *Git directory*: is where git stores the metadata and object database for your project. It is what is copied when you clone a repository from another computer. 
- *Working directory*: a single checkout of one version of the project, is in sync with the local filesystem and is representative the immediate changes made to content in files and directories. 
- *Staging area*: a simple file, generally contained in your git directory, stores information about what will go into your next commit. 

### Git Init 指令

- `git init your_project`: 这个指令会直接创建一个git的空项目，项目名称就是`your_project`.
- `git init`: 处于某个项目文件的根目录下，这个指令就会把这个项目创建成一个仓库，本质上就是创建了一个`.git`的隐藏文件。

### Git Reset 指令

> 这个指令主要是用于撤销更改的内容的。有三个主要的参数`--soft`, `--hard`, 和`--mixed`，分别对应不同的工作区。

- `git reset --soft`: 跟hard的区别，就是保留了更改的内容
- `git reset --hard`: 这种情况最常用，但是也是最危险的，因为清空了你的工作目录。
- `git reset --mixed`: 默认情况下是这种。

恢复暂存区的内容： 

```hash
git reset HEAD
# 把暂存区的内容放回到工作区

git reset --hard 
# 把暂存区的内容直接删除
```

### 重命名Git文件

`git mv home.html index.html`: 修改home的html文件名称。

### Git log查看版本演变

> Show the commit logs

- `git log` : 查看所有的提交历史。
- `git log --oneline`: 查看简介版本的提交历史
- `git log --oneline --graph`: 查看简洁版本并且把输出图形化。
- `git log -n2 --online`: 查看简洁版本的两条记录。

### 分支相关的内容

>分支用的很多，很好用。

- `git branch -v`: 查看当前有多少个分支。
- `git checkout -b branch_name 43752c5a643abf3791fe69f043718a39f495f3b2`: 根据已有的提交hashcode，创建新的分支。默认情况下，不加commit ID会在当前分支的最新commit进行创建。
- `git checkout branch_name`: 切换到另外一个分支。
- `git branch -d branch_name`: 输出某个分支，需要进行下一步的确认`git branch -D branch_name`. 注意`master`分支是可以被删除的。`git branch -D master`,指令就可以把主分支删除掉。

### 查看帮助文档

- `git help --web 参数`: 可以打开帮助文档的浏览器。
- `git --help`: 快速查看帮助文档。

```bash
git help --web branch
git help --web log 
... 
```

### 探究一下.git的文件夹

```bash
drwxr-xr-x  14 xwang  staff  448  6 Oct 18:46 .
drwxr-xr-x   6 xwang  staff  192  6 Oct 18:46 ..
-rw-r--r--   1 xwang  staff   38  6 Oct 18:42 COMMIT_EDITMSG
-rw-r--r--   1 xwang  staff   28  6 Oct 18:46 HEAD
-rw-r--r--   1 xwang  staff   41  6 Oct 11:05 ORIG_HEAD
-rw-r--r--   1 xwang  staff  137  6 Oct 09:45 config
-rw-r--r--   1 xwang  staff   73  6 Oct 09:45 description
drwxr-xr-x  13 xwang  staff  416  6 Oct 09:45 hooks
-rw-r--r--   1 xwang  staff  360  6 Oct 18:46 index
drwxr-xr-x   3 xwang  staff   96  6 Oct 09:45 info
drwxr-xr-x   4 xwang  staff  128  6 Oct 09:45 logs
drwxr-xr-x  26 xwang  staff  832  6 Oct 18:42 objects
drwxr-xr-x   4 xwang  staff  128  6 Oct 09:45 refs
-rw-r--r--@  1 xwang  staff  174  6 Oct 18:32 sourcetreeconfig
```

- **HEAD**: a reference to the last commit in the currently checkout branch. When you checkout another branch, the HEAD revision changes to point to the new branch. 其实就是当前分支的最新提交。

  ```bash
  cat .git/HEAD 
  >> ref: refs/heads/new_feature
  git checkout master
  cat .git/HEAD
  >> ref: refs/heads/master
  git diff 22ef44ad975548f779ea34c32da079f67b0a70cf 22ef44ad975548f779ea34c32da079f67b0a70cf 
  >> 直接生成两个commit的不同之处
  git diff HEAD HEAD~1
  >> 生成两个不同commit的区别之处
  ```

- **config**: 保存当前的仓库配置信息

- **ref**: 该文件夹保存的是heads和tags。其中heads表示的存储所有的分支，而tags则表示的是里程碑的开发，项目到一定的阶段时候，就会打上一个tag。

- **objects**: 常用的指令

  > objects文件夹保存的是对象数据库（object database），并且提供the unique key that now refers to that data object.

  ```hash
  git cat-file -t f9f935b66ec317a46e545cac50d0b67d25e4adc2
  >> Get the object type of the hashcode
  git cat-file -p f9f935b66ec317a46e545cac50d0b67d25e4adc2
  >> Get the content of the hashcode.
  ```

### Git commit, tree 和blob三个对象之间关系

- **commit**就是包含**一个**树和其他的meta信息。

  ```
  # 运行指令 git commit -m "[descriptive message"
  git commit -m "Bug fixes: [HER-639]"
  
  # 指令运行
  ```

  

- **tree**存储的就是文件夹

- **blob**存储的就是文件

### Detached head

>我的理解其实就是一个也指针，但是这个野指针只能指向某一个commit。你可以在那个commit之上进行修改。

```bash
git branch -v
# * master      f9f935b Change index.html to home.html
#  new_feature 5acc525 Add style and script to the home page
#  temp        22ef44a Add js script file

git checkout 5acc525
# Note: checking out '5acc525'.

# You are in 'detached HEAD' state. You can look around, make experimental
# changes and commit them, and you can discard any commits you make in this
# state without impacting any branches by performing another checkout.

# If you want to create a new branch to retain commits you create, you may
# do so (now or later) by using -b with the checkout command again. Example:

#  git checkout -b <new-branch-name>

# HEAD is now at 5acc525 Add style and script to the home page
```

当切换回已有的分支时，就会出现野指针丢失的这种情况。

### 修改Commit的内容信息-rebase

- 修改最近一条提交的信息

  ```
  git commit --amend
  >> 弹出来更改信息的窗口
  >> 在这个窗口进行信息的更改即可。
  ```

- 如果修改的是旧的commit的信息，需要用到rebase这个指令

  ```bash
  git rebase -i 041e8c4f0 # 选择要修改的commit上一条的提交ID
  # 这条运行后就会弹出来交互式的信息，然后把要更改的那条之前加上相应的命令
  r e11b63f Show to content on the page
  # 然后在下一个交互式的窗口，输入要更改的提交信息
  Show the content on the HTML page
  ```

### 合并多个连续commit的信息

- 第一种方式：使用`reset --soft`指令，这种的方式运行只能合并最上的几条提交

  ```bash
  #首先清楚需要合并几条连续的commit，然后把这几条commit进行合并
  git reset --soft @~2 #这里的2表示合并最近连续的两条提交
  #然后运行一下commit指令，就可以把最近的这两条合并了
  git commit -m '提交的信息'
  ```

- 第二种方式：使用`rebase`指令

  ```bash
  #需要留下第一条的Commit，然后，把后面要合并的commit的参数修改为s，squash
  git log --oneline
  # ec312d4 (HEAD -> temp) Revert "Add js script file"
  # 22ef44a (master) Add js script file
  # 4c0844a Add the body background color
  # 43752c5 Move style.css file into css folder
  # bdf8b7c Modified the homepage html file
  # ac2b9b6 Add index.html and style.css files
  # 想要合并中间四项应该怎么弄呢？
  git rebase -i ac2b9b6
  pick bdf8b7c Modified the homepage html file
  s 43752c5 Move style.css file into css folder
  s 4c0844a Add the body background color
  s 22ef44a Add js script file
  pick ec312d4 Revert "Add js script file"
  # 把中间三项都修改指令为s
  # 然后再修改一下提交的信息即可了。
  ```

### 删除多个Commit

```hash
#删除多个commit，或者直接回退到之前的某个commit应该怎么做呢？
git reset --hard [commitID]

# 如果不加commit的ID就只删除工作区的内容
git reset --hard #工作区的内容被清空了。
```

### 为什么要有Stage区呢？

- 如果做了某些修改，直接提交的话，就直接到版本控制器了，这个时候发现不需要放进控制器，那么只需要更新一下stage区就可以了。

  ```shell
  # 在添加进Stage区之前可以比较一下修改的内容，使用这个指令
  # 对比工作区的文件不同之处
  git diff 
  
  #比如修改了某个文件，然后使用git add . 把文件添加到了缓存区
  git add js/script.js 
  
  #这个时候对比一下发现，修改的内容不是必须的
  git diff --staged 
  
  #把文件从stage区移除，但是保留相应的修改: git reset [filename]
  git reset js/script.js
  #或者使用
git reset HEAD js/script.js
  ```
  

### 使用暂存区文件更新工作区文件-checkout

```bash
#如果使用暂存区的某个文件更新工作区的文件，使用checkout这个文件
git checkout -- [filename]
```

### Git 备份 - clone

- 本地的仓库：快速备份策略

  ```bash
  git clone /path/to/.git [folder-name]
  ```

  ```bash
  git clone file:///path/to/.git [folder-name]
  ```

- 远程仓库，如何进行push呢

  ```bash
  git remote add 
  git push 
  ```

### Git Glossary

- *Origin*: an alias on your system for a remote repository. 

> Reference: https://linuxacademy.com/blog/linux/git-terms-explained/

