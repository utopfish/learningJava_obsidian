## 分支(Branches)

<a name="pull-wrong-branch"></a>
### 我从错误的分支拉取了内容，或把内容拉取到了错误的分支

这是另外一种使用 `git reflog` 情况，找到在这次错误拉(pull) 之前HEAD的指向。

```sh
(master)$ git reflog
ab7555f HEAD@{0}: pull origin wrong-branch: Fast-forward
c5bc55a HEAD@{1}: checkout: checkout message goes here
```

重置分支到你所需的提交(desired commit):

```sh
$ git reset --hard c5bc55a
```

完成。

<a href="discard-local-commits"></a>
### 我想扔掉本地的提交(commit)，以便我的分支与远程的保持一致

先确认你没有推(push)你的内容到远程。

`git status` 会显示你领先(ahead)源(origin)多少个提交:

```sh
(my-branch)$ git status
# On branch my-branch
# Your branch is ahead of 'origin/my-branch' by 2 commits.
#   (use "git push" to publish your local commits)
#
```

一种方法是:

```sh
(master)$ git reset --hard origin/my-branch
```

<a name="commit-wrong-branch"></a>
### 我需要提交到一个新分支，但错误的提交到了master

在master下创建一个新分支，不切换到新分支,仍在master下:

```sh
(master)$ git branch my-branch
```

把master分支重置到前一个提交:

```sh
(master)$ git reset --hard HEAD^
```

`HEAD^` 是 `HEAD^1` 的简写，你可以通过指定要设置的`HEAD`来进一步重置。

或者, 如果你不想使用 `HEAD^`, 找到你想重置到的提交(commit)的hash(`git log` 能够完成)， 然后重置到这个hash。 使用`git push` 同步内容到远程。

例如, master分支想重置到的提交的hash为`a13b85e`:

```sh
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

签出(checkout)刚才新建的分支继续工作:

```sh
(master)$ git checkout my-branch
```

<a name="keep-whole-file"></a>
### 我想保留来自另外一个ref-ish的整个文件

假设你正在做一个原型方案(原文为working spike (see note)), 有成百的内容，每个都工作得很好。现在, 你提交到了一个分支，保存工作内容:

```sh
(solution)$ git add -A && git commit -m "Adding all changes from this spike into one big commit."
```

当你想要把它放到一个分支里 (可能是`feature`, 或者 `develop`), 你关心是保持整个文件的完整，你想要一个大的提交分隔成比较小。

假设你有:

  * 分支 `solution`, 拥有原型方案， 领先 `develop` 分支。
  * 分支 `develop`, 在这里你应用原型方案的一些内容。

我去可以通过把内容拿到你的分支里，来解决这个问题:

```sh
(develop)$ git checkout solution -- file1.txt
```

这会把这个文件内容从分支 `solution` 拿到分支 `develop` 里来:

```sh
# On branch develop
# Your branch is up-to-date with 'origin/develop'.
# Changes to be committed:
#  (use "git reset HEAD <file>..." to unstage)
#
#        modified:   file1.txt
```

然后, 正常提交。

Note: Spike solutions are made to analyze or solve the problem. These solutions are used for estimation and discarded once everyone gets clear visualization of the problem. ~ [Wikipedia](https://en.wikipedia.org/wiki/Extreme_programming_practices).

<a name="cherry-pick"></a>
### 我把几个提交(commit)提交到了同一个分支，而这些提交应该分布在不同的分支里

假设你有一个`master`分支， 执行`git log`, 你看到你做过两次提交:

```sh
(master)$ git log

commit e3851e817c451cc36f2e6f3049db528415e3c114
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:27 2014 -0400

    Bug #21 - Added CSRF protection

commit 5ea51731d150f7ddc4a365437931cd8be3bf3131
Author: Alex Lee <alexlee@example.com>
Date:   Tue Jul 22 15:39:12 2014 -0400

    Bug #14 - Fixed spacing on title

commit a13b85e984171c6e2a1729bb061994525f626d14
Author: Aki Rose <akirose@example.com>
Date:   Tue Jul 21 01:12:48 2014 -0400

    First commit
```

让我们用提交hash(commit hash)标记bug (`e3851e8` for #21, `5ea5173` for #14).

首先, 我们把`master`分支重置到正确的提交(`a13b85e`):

```sh
(master)$ git reset --hard a13b85e
HEAD is now at a13b85e
```

现在, 我们对 bug #21 创建一个新的分支:

```sh
(master)$ git checkout -b 21
(21)$
```

接着, 我们用 *cherry-pick* 把对`bug #21`的提交放入当前分支。 这意味着我们将应用(apply)这个提交(commit)，仅仅这一个提交(commit)，直接在HEAD上面。

```sh
(21)$ git cherry-pick e3851e8
```

这时候, 这里可能会产生冲突， 参见[交互式 rebasing 章](%E5%88%86%E6%94%AF(Branches)#interactive-rebase) [**冲突节**](%E5%88%86%E6%94%AF(Branches)#merge-conflict) 解决冲突.

再者， 我们为bug #14 创建一个新的分支, 也基于`master`分支

```sh
(21)$ git checkout master
(master)$ git checkout -b 14
(14)$
```

最后, 为 bug #14 执行 `cherry-pick`:

```sh
(14)$ git cherry-pick 5ea5173
```

<a name="delete-stale-local-branches"></a>
### 我想删除上游(upstream)分支被删除了的本地分支
一旦你在github 上面合并(merge)了一个pull request, 你就可以删除你fork里被合并的分支。 如果你不准备继续在这个分支里工作, 删除这个分支的本地拷贝会更干净，使你不会陷入工作分支和一堆陈旧分支的混乱之中。

```sh
$ git fetch -p
```

<a name='restore-a-deleted-branch'></a>
### 我不小心删除了我的分支

如果你定期推送到远程, 多数情况下应该是安全的，但有些时候还是可能删除了还没有推到远程的分支。 让我们先创建一个分支和一个新的文件:

```sh
(master)$ git checkout -b my-branch
(my-branch)$ git branch
(my-branch)$ touch foo.txt
(my-branch)$ ls
README.md foo.txt
```

添加文件并做一次提交

```sh
(my-branch)$ git add .
(my-branch)$ git commit -m 'foo.txt added'
(my-branch)$ foo.txt added
 1 files changed, 1 insertions(+)
 create mode 100644 foo.txt
(my-branch)$ git log

commit 4e3cd85a670ced7cc17a2b5d8d3d809ac88d5012
Author: siemiatj <siemiatj@example.com>
Date:   Wed Jul 30 00:34:10 2014 +0200

    foo.txt added

commit 69204cdf0acbab201619d95ad8295928e7f411d5
Author: Kate Hudson <katehudson@example.com>
Date:   Tue Jul 29 13:14:46 2014 -0400

    Fixes #6: Force pushing after amending commits
```

现在我们切回到主(master)分支，‘不小心的’删除`my-branch`分支

```sh
(my-branch)$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
(master)$ git branch -D my-branch
Deleted branch my-branch (was 4e3cd85).
(master)$ echo oh noes, deleted my branch!
oh noes, deleted my branch!
```

在这时候你应该想起了`reflog`, 一个升级版的日志，它存储了仓库(repo)里面所有动作的历史。

```
(master)$ git reflog
69204cd HEAD@{0}: checkout: moving from my-branch to master
4e3cd85 HEAD@{1}: commit: foo.txt added
69204cd HEAD@{2}: checkout: moving from master to my-branch
```

正如你所见，我们有一个来自删除分支的提交hash(commit hash)，接下来看看是否能恢复删除了的分支。

```sh
(master)$ git checkout -b my-branch-help
Switched to a new branch 'my-branch-help'
(my-branch-help)$ git reset --hard 4e3cd85
HEAD is now at 4e3cd85 foo.txt added
(my-branch-help)$ ls
README.md foo.txt
```

看! 我们把删除的文件找回来了。 Git的 `reflog` 在rebasing出错的时候也是同样有用的。

<a name="i-want-to-delete-a-branch"></a>
### 我想删除一个分支

删除一个远程分支:

```sh
(master)$ git push origin --delete my-branch
```

你也可以:

```sh
(master)$ git push origin :my-branch
```

删除一个本地分支:

```sh
(master)$ git branch -D my-branch
```

<a name="i-want-to-checkout-to-a-remote-branch-that-someone-else-is-working-on"></a>
### 我想从别人正在工作的远程分支签出(checkout)一个分支

首先, 从远程拉取(fetch) 所有分支:

```sh
(master)$ git fetch --all
```

假设你想要从远程的`daves`分支签出到本地的`daves`

```sh
(master)$ git checkout --track origin/daves
Branch daves set up to track remote branch daves from origin.
Switched to a new branch 'daves'
```

(`--track` 是 `git checkout -b [branch] [remotename]/[branch]` 的简写)

这样就得到了一个`daves`分支的本地拷贝, 任何推过(pushed)的更新，远程都能看到.
