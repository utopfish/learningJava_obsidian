## 跟踪文件(Tracking Files)

<a href="i-want-to-change-a-file-names-capitalization-without-changing-the-contents-of-the-file"></a>
### 我只想改变一个文件名字的大小写，而不修改内容

```sh
(master)$ git mv --force myfile MyFile
```

<a href="remove-from-git"></a>
### 我想从Git删除一个文件，但保留该文件

```sh
(master)$ git rm --cached log.txt
```
