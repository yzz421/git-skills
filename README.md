### 初次运行Git前配置

git自带一个 `git conifg` 的工具来帮助设置控制Git外观和行为的配置变量。这些变量存储在三个不同的位置：

1. `/etc/gitconfig`文件：包含系统上每一个用户及他们仓库的通用配置。如果在执行`gut config` 时带上 `--system`选项，那么它就会读取该文件中的配置变量
2. `~/.gitconfig`或`~/.config/git/config`文件：只针对当前用户。你可以传递`--global`选项让 Git 读写次文件，这会对你系统上 *所有* 的仓库生效。
3. 当前使用仓库的 Git 目录中的`config`文件（即`.git/config`)：针对该仓库。你可以传递`--local`选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。

可以通过以下命令查看所有的配置以及它们所在的文件：

`$ git config --list --show-origin`

### 用户信息

安装完 Git 后，要做的第一件事就是设置你的用户名和邮箱地址。每一次 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```
$ git config --global user.name 'yzz421'
$ git config --global user.email yzz.ice@example.com
```



### 检查配置信息

```
$ git config --list
user.name=yzz421
user.email=yzz.ice@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

可以通过输入 `git config <key>` 来检查 git 的某一项配置

```
$ git config user.name
yzz421
```



### 获取帮助

```
$ git help <verb>
$ git <verb> --help
$ mant git-<verb>
```

例如想要获取 `git config` 命令的手册，执行

```
$ git help config
```

此外，如果你不需要全面的手册，只需要可用选项的快速参考，那么可以用`-h`选项获得更简明的“help"输出

```
$ git add -h
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch           select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    --renormalize         renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod (+|-)x        override the executable bit of the listed files
```



### 获取 Git 仓库

通常有两种获取 Git 项目的方式：

1. 将尚未进行版本控制的本地目录转换为 Git 仓库
2. 从其他服务器 **克隆** 存在的 Git 仓库

```
$ cd /code/my_project
```

之后执行

```
$ git init
```

该命令将创建一个名为 `.git`的子目录，这个子目录含有你的初始化 Git 仓库中所有的必须文件。注意，此时，你的项目还没有被跟踪。

如果在一个已存文件的文件夹中进行版本控制，你应该开始追踪这些文件并进行初始化提交。可以通过 `git add`命令来制定所需的文件来进行跟踪，然后执行`git commit`

```
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

克隆现有的仓库

` $ git clone https://github.com/libgit2/libgit2 `

如果你想在克隆远程仓库的时候自定义本地仓库的名字，你可以通过额外的参数指定新的目录名

`$ git clone https://github.com/libgit2/libgit2 mylibgit`

只会执行与上一条命了相同的操作，但目标目录名变成了 `mylibgit`

Git 支持多种数据传输协议。上面所使用的是 `https://`协议，不过你也可以使用`git://`协议或者使用 SSH 传输协议，比如 `user@server:path/to/repo.git`



### 检查当前文件状态

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

```
$ echo 'My Project' > README
$ git status
On branch master
Your branch is update-to-date with 'origin/master'.
Untracked files:
	(use "git add <file>..." to include in what will be committed)
	
	  README
	 
nothing added to commit but untracked files present (use "git add" to track)
```



### 跟踪新文件

```
$ git add README
```

此时再运行 `git status`

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

    new file:   README
```



### 状态简览

```
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

前面有 `??`标记的代表该文件还未跟踪

`A` 代表新添加到暂存区的文件

`M` 代表修改过的文件



### 忽略文件

`.gitignore`

```
$ cat .gitignore
*.[oa]
*~
```

第一行告诉 Git 忽略 `.o` 或 `.a` 结尾的文件。第二行告诉 Git 忽略所有名字以（~）结尾的文件。

文件 `.gitignore`的格式规范

- 所有空行或者以 # 开头的行都会被 Git 忽略
- 可以使用标准的 glob模式匹配，他会递归地应用在整个工作区中
- 匹配模式可以以（/）开头防止递归
- 匹配模式可以以（/）结尾指定目录
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（`*`）匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）。 使用两个星号（`**`）表示匹配任意中间目录，比如 `a/**/z` 可以匹配 `a/z` 、 `a/b/z` 或 `a/b/c/z` 等。

`.gitignore` 例子

```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a 即使前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件夹，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下为 build 的文件夹
build/

# 忽略 doc/notes.txt， 但不需略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有自目录下的 .pdf 文件
doc/**/*.pdf
```



### 查看已暂存和未暂存的修改

`git diff` 命令

```
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's

```

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged` 命令

用 `git diff --cached` 查看已经暂存起来的变化（ `--staged` 和 `--cached` 是同义词）



### 提交更新

可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行

`$ git commit -m 'Story 182: Fix benchmarks for speed'`



### 移除文件

删除工作目录中的指定文件

`$ git rm PORJECTS.md`

如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。

如果只是从暂存区域移除并保留在当前工作目录中（即保留在本地）

```console
$ git rm --cached README
```

`git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式

```console
$ git rm log/\*.log
```

注意到星号 `*` 之前的反斜杠 `\`， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开

删除所有名字以 `~` 结尾的文件。

```console
$ git rm \*~
```



### 移动文件

```console
$ git mv file_from file_to
```

其实，运行 `git mv` 就相当于运行了下面三条命令：

```console
$ mv README.md README
$ git rm README.md
$ git add README
```

### 查看提交历史

`git log`

不带任何参数的情况下 `git log`会按时间先后顺序列出所有的提交，最近的更新排在上面。这个命令回列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明

`git log -p` 或者 `--patch`， 它会现实每次提交所引入的差异

你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交

如果想看到每次提交的简略统计信息，可以使用 `--stat` 选项

还有一个非常有用的选项是 `--pretty`。这个选项可以使用不同于默认格式的方式展示提交历史。比如 `oneline`会讲每个提交放在一样，另外还有`short`，`full`和`fuller`选项，他们展示的格式基本一致，但是详尽程度不一

`format`可以定制记录的显示格式

| 选项 | 说明                                        |
| :--- | ------------------------------------------- |
| %H   | 提交的完整哈希值                            |
| %h   | 提交的简写哈希值                            |
| %T   | 树的完整哈希值                              |
| %t   | 树的简写哈希值                              |
| %P   | 父提交的完整哈希值                          |
| %p   | 父提交的简写哈希值                          |
| %an  | 作者的名字                                  |
| %ae  | 作者的电子邮件地址                          |
| %ad  | 作者修订日期（可以用 --date=选项 来定制格式 |
| %ar  | 作者修订日期，按多久以前的方式显示          |
| %cn  | 提交者的名字                                |
| %ce  | 提交者的电子邮件地址                        |
| %cd  | 提交日期                                    |
| %cr  | 提交日期（距今多长时间）                    |
| %s   | 提交说明                                    |

`作者 `和 `提交者` 之间的区别，作者指的是实际作出修改的人，提交者是将此工作成果提交到仓库的人。 所以，当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是作者，而那个核心成员就是提交者。

git log没啥意思之后在补充这段



### 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者是提交信息写错了。此时，可以运行带有 `--amend` 选项的命令来重新提交

` $ git commit --amend`

这个命令会将暂存区中的文件提交。如果自上次提交你还未做任何修改，那么快照会保持不变，而你所修改的只是提交信息

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作

```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终你只会有一个提交 —— 第二次提交将代替第一次提交的结果

> Note 
>
> 当你修补最后提交时，并不是通过用改进后的提交 `原位替换` 掉旧有提交的方式来修复的，理解这一点非常重要。从效果上来说，就像是旧有的提交从未存在过一样，它并不会出现在仓库的历史中。
>
> 修补提交最明显的角质是可以稍微改进你最后的提交，而不是让“啊，忘了添加一个文件”或者“小修补，修正笔误”这种提交信息弄乱你的仓库历史



#### 取消暂存的文件

例如，你已经修改了两个文件并且想要将他们作为两次独立的修改提交，但是却意外的输入了`git add *`暂存了他们两个。如何只取消暂存两个钟的一个呢？`git status` 命令提示了你

```
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
```

在“Changes to be committed” 文字的正下方，使用`git reset HEAD <file> ... ` 来取消暂存，所以我们可以这样来取消暂存 `CONTRIBUTING.md` 文件

```
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

这个命令有点奇怪，但是起作用了。CONTRIBUTING.md文件已经是修改未暂存的状态了。

> Note
>
> git reset 确实是个危险的命令，如果加上了 --hard选项更是如此。然而在上述场景中，工作目录中的文件尚未修改，因此相对安全一些

### 撤销对文件的修改

如果并不想保留对 `CONTRIBUTING.md` 文件的修改怎么办？在最后一个例子中，为暂存的区域是这样的（`git status`):

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

它非常清楚地告诉了你如何撤销之前所做的修改。

```
$ git checkout -- CONTRIBUTING.md
$ git status
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

可以看到那些修改已经被撤销了。

>
>
>important
>
>请务必记得 `git checkout -- <file>` 是一个危险的命令。你对那个文件在本地的任何修改都会消失 —— Git 会用最近提交的版本覆盖掉它。除非你确实清楚不想要对那个文件的本地修改了。

