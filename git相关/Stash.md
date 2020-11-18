## Stash

### 暂存所有改动

暂存你工作目录下的所有改动

```sh
$ git stash
```

你可以使用`-u`来排除一些文件

```sh
$ git stash -u
```

### 暂存指定文件

假设你只想暂存某一个文件

```sh
$ git stash push working-directory-path/filename.ext
```

假设你想暂存多个文件

```sh
$ git stash push working-directory-path/filename1.ext working-directory-path/filename2.ext
```

<a name="stash-msg"></a>
### 暂存时记录消息

这样你可以在`list`时看到它

```sh
$ git stash save <message>
```
或
```sh
$ git stash push -m <message>
```
<a name="stash-apply-specific"></a>
### 使用某个指定暂存

首先你可以查看你的`stash`记录

```sh
$ git stash list
```

然后你可以`apply`某个`stash`

```sh
$ git stash apply "stash@{n}"
```

此处， 'n'是`stash`在栈中的位置，最上层的`stash`会是0

除此之外，也可以使用时间标记(假如你能记得的话)。

```sh
$ git stash apply "stash@{2.hours.ago}"
```

<a href="stage-and-keep-unstaged"></a>
### 暂存时保留未暂存的内容

你需要手动create一个`stash commit`， 然后使用`git stash store`。

```sh
$ git stash create
$ git stash store -m "commit-message" CREATED_SHA1
```

<a name="miscellaneous-objects"></a>