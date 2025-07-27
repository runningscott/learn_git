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

* 在 VSCode 安装 GitLens 图形化对比插件

如果我们发现改动是**误操作**，希望恢复到该文件的上一个 commit 的状态，可以使用命令：`git checkout -- README.md`
`git checkout` 从版本仓库中取得一个文件的指定版本，然后覆盖到工作目录中。命令后面可以指定一个 *commit id*，上面我们用 `--`就表示从仓库里最新的一个 commit 中取，最后是要取出的文件路径（当前目录下 `README.md`文件）。运行上面的命令会废弃掉（*discard*）刚才对 `README.md` 做的修改。

`git reset HEAD <file>...` to unstage 

