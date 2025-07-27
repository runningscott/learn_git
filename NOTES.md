# Welcome to my Git tutorial and playground.

## Git 基本概念与用法

* 基本操作
```python
mkdir learn-git
git --version
git 
git --help
git config --list
```
* 基本设置

```python
git config --global user.email ""
git config --global user.name ""
git config --global core.editor "code -w"
git config --list
ls -a
ls -Force #Windows 系统。
code .gitconfig 
```

* 建立本地 repo
```python
git init
ls -a
git status
code README.md
ls
git status
git add README.md
git status
git commit #在第一行写 commit 注释。
git status
ls
cat README.md
git log
code . #VSCode 打开当前目录,创建 2 个 Python 文件。
git status
git add . #加入所有修改到 Git 。
git status
git restore --staged README.md #将除 README.md 文件以外修改加入暂存区。
git status
git commit #Add 2 Python samples.
git status #README.md 文件未加入。
git add . #将其加入。
git status
git commit #Add note to README.
git status
git log # 版本号前七位。
```

* 版本历史与切换
```python
git checkout 0364f6e #版本号前七位。
ls 
cat README.md
git checkout master #返回根目录 master 分支。
ls
git log #列出版本仓库中已有的 commit history，每一个 commit 一段，从新到旧排列（最后的 commit 在最上面）。最新的 commit 后面有个 HEAD -> master 的标志。每一段开头都是 commit 字样后面跟着很长的 commit id，一般用前几位代替；之后是提交作者、提交时间，然后是 commit message。信息一目了然，是非常合适的“Proof of Work”。按 q 键退回到命令行界面。
git diff fd3fe30 README.md #比较该文件与前版本的区别。
git diff 0364f6e fd3fe30 README.md #比较两版本 README 的区别。
```

如果我们发现改动是**误操作**，希望恢复到该文件的上一个 commit 的状态，可以使用命令：`git checkout -- README.md`
`git checkout` 从版本仓库中取得一个文件的指定版本，然后覆盖到工作目录中。命令后面可以指定一个 *commit id*，上面我们用 `--`就表示从仓库里最新的一个 commit 中取，最后是要取出的文件路径（当前目录下 `README.md`文件）。运行上面的命令会废弃掉（*discard*）刚才对 `README.md` 做的修改。

`git reset HEAD <file>...` to unstage 

## 附注：关于 git 新版本的变化
在 *git* 2.23 及以后的版本中引入了两个实验性的新命令：`git switch` 和 `git restore`，其目的是以后替代语焉不详的 `git checkout` 命令，这些新的命令（尤其是 `git restore`）从 2.23 版本开始出现在交互提示中：代替 `git reset HEAD <file>...` 用于从 staging area 撤出修改，以及代替 `git checkout -- <file>...` 用于回退修改。

1. 老的命令在相当长时间内都还有效；
2. 关于 2.23 引入的变化，git 官网有一篇[详细的说明](https://github.blog/open-source/git/highlights-from-git-2-23/)。


## 分支与协同
**标签**（tag）和**分支**（branch）都是 *git*提供的重要工具，二者都是指向 *commit* 的指针。区别在于 *tag*指向一个固定的 *commit*，而 *branch* 会随着你的操作始终跟着最新的 *commit*。

`git tag` 命令处理与 tag 有关的操作。如果不带任何参数执行 `git tag` 会输出仓库中存在的所有 tag，如果我们想在目前最新的 commit 上打一个标签 v1.0，表示这是我们的 1.0 版本的状态，可以这么做：

`git tag v1.0`

这种相当于 commit 别名的 tag 叫“轻量标签（lightweight tag）”，Git 还支持一种带标注的标签（annotated tag），这里就不详细介绍了，本质差不多，只是增加了标签本身的属性，比如是谁打的，什么时候打的等等。不管是哪种标签，我们都可以将其用在需要指定一个 commit 的场合，比如 checkout、diff 等操作中。
我们还可以给某个更早的 commit 打标签，在标签名后面加上 commit id 即可，比如：
`git tag v0.1 0364f6e`
如果要删除某个标签用 `git tag -d tag_name` 就可以了。

**标签**（tag）给一个 commit 打上标记，帮助我们记忆，方便我们使用，而**分支**（branch）更进一步，能帮助我们标记一串 commit，是并行开发（或者叫并行创作）的必备工具。

并行开发就是要做的事有不止一条线索，有的人在做一条线，有的人在做另一条线；或者一个人的一段时间在做一条线，另一段时间在做另一条线。

举个例子来说，一个产品的 v1.0 版本上线之后，开始继续开发 v2.0 的新功能，这是一条线；当发现 v1.0 的一些错误时（不论自己发现还是用户报告），就需要立刻处理立刻修复，这时候不可能在新功能开发的基础上去修问题，而是要在 v1.0 那个版本基础上去修复，修复好的版本 v1.0.1 发布到生产环境上。这样新功能开发和老功能修错互不影响，到合适时候把 v1.0.1 的修改合并到 v2.0 的代码中就好了。这个逻辑大致上就像这样：

<img src="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-1.png" />

分支是在比较复杂的工作任务中一定会出现的需求，事实上 git 以前的版本控制工具也都支持分支，但是它们建立分支的代价很高，要占用很多存储空间，执行也很慢，而 git 使用了一种非常聪明的方法来处理分支，使得分支变得很轻，可以随心所欲的使用分支，事实上 git 的理念是高度鼓励使用分支的，凡要做点新事情都可以开分支，在不影响其他人和其他任务的情况下用一个独立的分支去试，试好了再合并回主分支就好了，那么 git 是怎么做到的呢？

在 git 中，branch 和 tag 一样，都是指向 commit 的指针，区别在于 tag 是固定的，而 branch 是会移动的。

每个 git 版本仓库创建时都会创建一个 branch 叫 `master`，即主分支，在我们创建其他 branch 之前，这个主分支会跟着我们提交的 commit 走，永远指向最新的一个 commit。

现在假定进展到某一个时刻，在主分支之外需要一个另外的分支来修某个特定的 bug，于是创建了一个新分支叫 `bug/fix1`，这时候实际上就有两个 branch 指针指向同一个 commit，那么问题就来了，我们下一次提交 commit 之后，这两个指针会怎么走？都跟着最新 commit 走吗？这么“亦步亦趋”还叫什么分支呢？

这时候那个前面露过一面的 `HEAD` 就出现了，这也是一个指针，不过这是指向一个 branch 的指针，`HEAD` 指向哪个 branch，哪个 branch 就会跟着我们的最新 commit 走。

假定我们当前最新 commit 为 `51b61b3`，此时我们创建新分支 `bug/fix1`：

```
git branch bug/fix1
```
`git branch` 在当前 commit 上创建分支，注意创建分支并不会自动切换到新创建的分支，目前我们还工作在 `master` 分支，`HEAD` 指针指向 `master` 分支，这时候 repo 的状态是这样的：

<img src="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-2.png" />

由于我们现在需要尽快修复 bug，所以我们需要切换到新创建的 `bug/fix1` 分支，这需要用 `checkout` 命令：
```
git checkout bug/fix1
```

切换分支其实就是让 `HEAD` 指针指向该分支：

<img src="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-3.png" />

这时候 `HEAD` 指向的 `bug/fix1` 分支指针就会跟着我们提交的新 commit 走了，比如我们提交了新的 `f843665` 号 commit，就会变成这样：

<img src="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-4.png" />

假定 `f843665` 号 commit 成功修复了 bug，我们在 `bug/fix1` 分支上的工作告一段落，需要切回主分支：

```
git checkout master
```

于是 `HEAD` 指针指回 `master` 分支：

<img src="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-5.png" />

现在轮到 `master` 分支指针跟着我们提交的最新的 commit 走了：

<img scr="https://raw.githubusercontent.com/neolee/wop/8803016c69eb14cde00c122c0caf3778a5c395c2/assets/git-branches-6.png" />

如果是多人协同的情况，上述两个分支上的工作甚至可以同时进行，只要一个人在自己机器上让 `HEAD` 指向 `master` 分支，TA就工作在 `master` 分支上；另一个人让 `HEAD` 指向 `bug/fix1` 分支，TA就工作在 `bug/fix1` 分支上；两人的工作都可以 push 回 team repo，互不影响。

在合适的时间，团队里某位成员可以进行分支合并，假定是把 `bug/fix1` 分支合并到 `master` 分支（实际上可以把任意分支合并到任意分支），那么大致的流程如下：

- 从 team repo 同步所有 branch 及 commit：`git fetch origin`
- 确认工作在 `master` 分支下：`git checkout master`
- 将 `bug/fix1` 分支合并到 `master` 分支：`git merge bug/fix1`

合并中可能出现冲突等问题，需要手工解决，就不在这里展开细说了。

这就是 git 的多分支工作模型，这个模型既可以用于单人工作，也可以用于多人协作，在多人协作时更具价值，让多人并行工作成为顺理成章、几乎没有额外代价的事情。

## 进阶学习资料

网上有不少关于 Git 和 GitHub 的优质学习资料，除了前面提到的官方教科书 [Pro Git](https://git-scm.com/book/en/v2) 以外，我们比较喜欢的还有这些：

- [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/)
- [Git How To](https://githowto.com/)
- [Git pretty - Solve Git Mess](http://justinhileman.info/article/git-pretty/)

颇受欢迎的内容推荐列表 [awesome-git](https://github.com/dictcp/awesome-git) 中还有很多不错的资料，也可以参考。





















* 在 VSCode 安装 GitLens 图形化对比插件

