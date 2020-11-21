## 杂项(Miscellaneous Objects)

<a name="clone-submodules"></a>
### 克隆所有子模块

```sh
$ git clone --recursive git://github.com/foo/bar.git
```

如果已经克隆了:

```sh
$ git submodule update --init --recursive
```

<a name="delete-tag"></a>
### 删除标签(tag)

```sh
$ git tag -d <tag_name>
$ git push <remote> :refs/tags/<tag_name>
```

<a name="recover-tag"></a>
### 恢复已删除标签(tag)

如果你想恢复一个已删除标签(tag), 可以按照下面的步骤: 首先, 需要找到无法访问的标签(unreachable tag):

```sh
$ git fsck --unreachable | grep tag
```

记下这个标签(tag)的hash，然后用Git的 [update-ref](http://git-scm.com/docs/git-update-ref):

```sh
$ git update-ref refs/tags/<tag_name> <hash>
```

这时你的标签(tag)应该已经恢复了。

<a name="deleted-patch"></a>
### 已删除补丁(patch)

如果某人在 GitHub 上给你发了一个pull request, 但是然后他删除了他自己的原始 fork, 你将没法克隆他们的提交(commit)或使用 `git am`。在这种情况下, 最好手动的查看他们的提交(commit)，并把它们拷贝到一个本地新分支，然后做提交。

做完提交后, 再修改作者，参见[变更作者](%E6%9D%82%E9%A1%B9(Miscellaneous%20Objects)#commit-wrong-author)。 然后, 应用变化, 再发起一个新的pull request。