# Welcome to my Git tutorial (or playground).

* 创建目录
```python
mkdir learn-git
git --version
git 
git --help
git config --list
```
* 配置 Git

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

* 不同版本的切换
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
---

如果我们发现改动是**误操作**，希望恢复到该文件的上一个 commit 的状态，可以使用命令：`git checkout -- README.md`
`git checkout` 从版本仓库中取得一个文件的指定版本，然后覆盖到工作目录中。命令后面可以指定一个 *commit id*，上面我们用 `--`就表示从仓库里最新的一个 commit 中取，最后是要取出的文件路径（当前目录下 `README.md`文件）。运行上面的命令会废弃掉（*discard*）刚才对 `README.md` 做的修改。

`git reset HEAD <file>...` to unstage 

---

## 分支与协同
**标签**（tag）和**分支**（branch）都是 *git*提供的重要工具，二者都是指向 *commit* 的指针。区别在于 *tag*指向一个固定的 *commit*，而 *branch* 会随着你的操作始终跟着最新的 *commit*。

`git tag` 命令处理与 tag 有关的操作。如果不带任何参数执行 `git tag` 会输出仓库中存在的所有 tag，如果我们想在目前最新的 commit 上打一个标签 v1.0，表示这是我们的 1.0 版本的状态，可以这么做：

`git tag v1.0`

这种相当于 commit 别名的 tag 叫“轻量标签（lightweight tag）”，Git 还支持一种带标注的标签（annotated tag），这里就不详细介绍了，本质差不多，只是增加了标签本身的属性，比如是谁打的，什么时候打的等等。不管是哪种标签，我们都可以将其用在需要指定一个 commit 的场合，比如 checkout、diff 等操作中。
我们还可以给某个更早的 commit 打标签，在标签名后面加上 commit id 即可，比如：
`git tag v0.1 0364f6e`
如果要删除某个标签用 `git tag -d tag_name` 就可以了。

**标签**（tag）给一个 commit 打上标记，帮助我们记忆，方便我们使用，而**分支**（branch）更进一步，能帮助我们标记一串 commit，是并行开发（或者叫并行创作）的必备工具。

## 附注：关于 git 新版本的变化
在 *git* 2.23 及以后的版本中引入了两个实验性的新命令：`git switch` 和 `git restore`，其目的是以后替代语焉不详的 `git checkout` 命令，这些新的命令（尤其是 `git restore`）从 2.23 版本开始出现在交互提示中：代替 `git reset HEAD <file>...` 用于从 staging area 撤出修改，以及代替 `git checkout -- <file>...` 用于回退修改。

1. 老的命令在相当长时间内都还有效；
2. 关于 2.23 引入的变化，git 官网有一篇[详细的说明](https://github.blog/open-source/git/highlights-from-git-2-23/)。

* 在 VSCode 安装 GitLens 图形化对比插件