

# 图解git #

https://oschina.gitee.io/learn-git-branching/  可视化git

## 基础 ##

### 1 git commit ###

> Git 仓库中的提交记录保存的是你的目录下所有文件的**快照**，就像是把整个目录复制，然后再粘贴一样，但比复制粘贴优雅许多！
>
> Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将**当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录**。
>
> Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面**都有父节点**的原因 —— 我们会在图示中用箭头来表示这种关系。对于项目组的成员来说，维护提交历史对大家都有好处。
>
> 关于提交记录太深入的东西咱们就不再继续探讨了，现在你可以把提交记录看作是项目的快照。提交记录非常轻量，可以快速地在这些提交记录之间切换！

![image-20210622233438170](.assets/image-20210622233438170.png)
