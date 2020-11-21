## 暂存(Staging)

<a href="#i-need-to-add-staged-changes-to-the-previous-commit"></a>
### 我需要把暂存的内容添加到上一次的提交(commit)

```sh
(my-branch*)$ git commit --amend

```

<a name="commit-partial-new-file"></a>
### 我想要暂存一个新文件的一部分，而不是这个文件的全部

一般来说, 如果你想暂存一个文件的一部分, 你可这样做:

```sh
$ git add --patch filename.x
```

`-p` 简写。这会打开交互模式， 你将能够用 `s` 选项来分隔提交(commit)； 然而, 如果这个文件是新的, 会没有这个选择， 添加一个新文件时, 这样做:

```sh
$ git add -N filename.x
```

然后, 你需要用 `e` 选项来手动选择需要添加的行，执行 `git diff --cached` 将会显示哪些行暂存了哪些行只是保存在本地了。

<a href="stage-in-two-commits"></a>
### 我想把在一个文件里的变化(changes)加到两个提交(commit)里

`git add` 会把整个文件加入到一个提交. `git add -p` 允许交互式的选择你想要提交的部分.

<a href="unstaging-edits-and-staging-the-unstaged"></a>
### 我想把暂存的内容变成未暂存，把未暂存的内容暂存起来

多数情况下，你应该将所有的内容变为未暂存，然后再选择你想要的内容进行commit。
但假定你就是想要这么做，这里你可以创建一个临时的commit来保存你已暂存的内容，然后暂存你的未暂存的内容并进行stash。然后reset最后一个commit将原本暂存的内容变为未暂存，最后stash pop回来。

```sh
$ git commit -m "WIP"
$ git add .
$ git stash
$ git reset HEAD^
$ git stash pop --index 0
```

注意1: 这里使用`pop`仅仅是因为想尽可能保持幂等。
注意2: 假如你不加上`--index`你会把暂存的文件标记为为存储.这个[链接](https://stackoverflow.com/questions/31595873/git-stash-with-staged-files-does-stash-convert-staged-files-to-unstaged?answertab=active#tab-top) 解释得比较清楚。（不过是英文的，其大意是说，这是一个较为底层的问题，stash时会做2个commit，其中一个会记录index状态，staged的文件等东西，另一个记录worktree和其他的一些东西，如果你不在apply时加index，git会把两个一起销毁，所以staged里就空了）。