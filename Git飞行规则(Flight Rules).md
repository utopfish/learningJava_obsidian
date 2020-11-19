# Git飞行规则(Flight Rules)


#### 什么是"飞行规则"?

这是一篇给宇航员（这里就是指使用Git的程序员们）的指南，用来指导问题出现后的应对之法。

>  *飞行规则(Flight Rules)* 是记录在手册上的来之不易的一系列知识，记录了某个事情发生的原因，以及怎样一步一步的进行处理。本质上, 它们是特定场景的非常详细的标准处理流程。 [...]

> 自20世纪60年代初以来，NASA一直在捕捉(capturing)我们的失误，灾难和解决方案, 当时水星时代(Mercury-era)的地面小组首先开始将“经验教训”收集到一个纲要(compendium)中，该纲现在已经有上千个问题情景，从发动机故障到破损的舱口把手到计算机故障，以及它们对应的解决方案。
&mdash; Chris Hadfield, *一个宇航员的生活指南(An Astronaut's Guide to Life)*。

#### 这篇文章的约定

为了清楚的表述，这篇文档里的所有例子使用了自定义的bash 提示，以便指示当前分支和是否有暂存的变化(changes)。分支名用小括号括起来，分支名后面跟的`*`表示暂存的变化(changes)。


**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

  - [[编辑提交(editting commits)]]
    - [[编辑提交(editting commits)#我刚才提交了什么]]
    - [[编辑提交(editting commits)#我的提交信息 commit message 写错了]]
    - [[编辑提交(editting commits)#我提交 commit 里的用户名和邮箱不对]]
    - [[编辑提交(editting commits)#我想从一个提交 commit 里移除一个文件]]
    - [[编辑提交(editting commits)#我想删除我的的最后一次提交 commit]]
    - [[编辑提交(editting commits)#删除任意提交 commit]]
    - [[编辑提交(editting commits)#我尝试推一个修正后的提交 amended commit 到远程，但是报错：]]
    - [[编辑提交(editting commits)#我意外的做了一次硬重置 hard reset ，我想找回我的内容]]
  
  - [[暂存(Staging)]]
    - [[暂存(Staging)#我需要把暂存的内容添加到上一次的提交 commit]]
    - [[暂存(Staging)#我想要暂存一个新文件的一部分，而不是这个文件的全部]]
    - [[暂存(Staging)#我想把在一个文件里的变化 changes 加到两个提交 commit 里]]
    - [[暂存(Staging)#我想把暂存的内容变成未暂存，把未暂存的内容暂存起来]]

  - [[未暂存(Unstaged)的内容]]
    - [[未暂存(Unstaged)的内容#我想把未暂存的内容移动到一个新分支]]
    - [[未暂存(Unstaged)的内容#我想把未暂存的内容移动到另一个已存在的分支]]
    - [[未暂存(Unstaged)的内容#我想丢弃本地未提交的变化 uncommitted changes]]
    - [[未暂存(Unstaged)的内容#我想丢弃某些未暂存的内容]]
 
 - [[分支(Branches)]]
  	- [[分支(Branches)#我从错误的分支拉取了内容，或把内容拉取到了错误的分支]]
  	- [[分支(Branches)#我想扔掉本地的提交 commit ，以便我的分支与远程的保持一致]]
  	- [[分支(Branches)#我需要提交到一个新分支，但错误的提交到了master]]
 	 - [[分支(Branches)#我想保留来自另外一个ref-ish的整个文件]]
 	 - [[分支(Branches)#我把几个提交 commit 提交到了同一个分支，而这些提交应该分布在不同的分支里]]
  	- [[分支(Branches)#我想删除上游 upstream 分支被删除了的本地分支]]
  	- [[分支(Branches)#我不小心删除了我的分支]]
 	 - [[分支(Branches)#我想删除一个分支]]
 	 - [[分支(Branches)#我想从别人正在工作的远程分支签出 checkout 一个分支]]
  - [[Rebasing 和合并(Merging)]]
  	- [[Rebasing 和合并(Merging)#我想撤销rebase merge]]
  	- [[Rebasing 和合并(Merging)#我已经rebase过 但是我不想强推 force push]]
  	- [[Rebasing 和合并(Merging)#我需要组合 combine 几个提交 commit]]
  		- [[Rebasing 和合并(Merging)#安全合并 merging 策略]]
  		- [[Rebasing 和合并(Merging)#我需要将一个分支合并成一个提交 commit]]
  		- [[Rebasing 和合并(Merging)#我只想组合 combine 未推的提交 unpushed commit]]
  	- [[Rebasing 和合并(Merging)#检查是否分支上的所有提交 commit 都合并 merge 过了]]
  	- [[Rebasing 和合并(Merging)#交互式rebase interactive rebase 可能出现的问题]]
  		- [[Rebasing 和合并(Merging)#这个rebase 编辑屏幕出现'noop']]
  		- [[Rebasing 和合并(Merging)#有冲突的情况]]
  - [[Stash]]
  	- [[Stash#暂存所有改动]]
  	- [[Stash#暂存指定文件]]
  	- [[Stash#暂存时记录消息]]
  	- [[Stash#使用某个指定暂存]]
  	- [[Stash#暂存时保留未暂存的内容]]
  
  - [[杂项(Miscellaneous Objects)]]
	  - [[杂项(Miscellaneous Objects)#克隆所有子模块]]
	  - [[杂项(Miscellaneous Objects)#删除标签 tag]]
	  - [[杂项(Miscellaneous Objects)#恢复已删除标签 tag]]
	  - [[杂项(Miscellaneous Objects)#已删除补丁 patch]]
	
- [[跟踪文件(Tracking Files)]]
	- [[跟踪文件(Tracking Files)#我只想改变一个文件名字的大小写，而不修改内容]]
	- [[跟踪文件(Tracking Files)#我想从Git删除一个文件，但保留该文件]]
  - [[配置(Configuration)]]
  	- [[配置(Configuration)#我想给一些Git命令添加别名 alias]]
  	- [[配置(Configuration)#我想缓存一个仓库 repository 的用户名和密码]]
  - [[我不知道我做错了些什么]]
- [[其它资源(Other Resources)]]
	- [[其它资源(Other Resources)#书 Books]]
	- [[其它资源(Other Resources)#教程 Tutorials]]
	- [[其它资源(Other Resources)#脚本和工具 Scripts and Tools]]
	- [[其它资源(Other Resources)#GUI客户端 GUI Clients]]













