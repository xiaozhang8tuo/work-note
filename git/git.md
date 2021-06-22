

[TOC]

# 图解git #

https://oschina.gitee.io/learn-git-branching/  可视化git

## 基础 ##

### 1 git commit ###

Git 仓库中的提交记录保存的是你的目录下所有文件的**快照**，就像是把整个目录复制，然后再粘贴一样，但比复制粘贴优雅许多！

Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将**当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录**。

Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面**都有父节点**的原因 —— 我们会在图示中用箭头来表示这种关系。对于项目组的成员来说，维护提交历史对大家都有好处。

关于提交记录太深入的东西咱们就不再继续探讨了，现在你可以把提交记录看作是项目的快照。提交记录非常轻量，可以快速地在这些提交记录之间切换！

![image-20210622233438170](.assets/image-20210622233438170.png)

**git commit** 

我们刚才修改了代码库，并把这些修改保存成了一个提交记录 `C2`。`C2` 的父节点是 `C1`，父节点是当前提交中变更的基础。

<img src=".assets/image-20210622234005304.png" alt="image-20210622234005304" style="zoom:67%;" />

### 2 git branch ###

Git 的分支也非常轻量。它们只是简单地指向某个提交纪录 —— 仅此而已。所以许多 Git 爱好者传颂：早建分支！多用分支！

这是因为即使**创建再多分的支也不会造成储存或内存上的开销**，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单多了。

在将分支和提交记录结合起来后，我们会看到两者如何协作。现在只要记住使用分支其实就相当于在说：“我想基于这个提交以及它所有的父提交进行新的工作。”

<img src=".assets/image-20210622234324035.png" alt="image-20210622234324035" style="zoom:67%;" />

**git branch newImage**

<img src=".assets/image-20210622234452372.png" alt="image-20210622234452372" style="zoom:67%;" />

创建的新分支指向的是目前的提交记录C1。分支上的*****代表当前所在分支是master. git commit 会使master向下产生一个子节点，newImage不变

<img src=".assets/image-20210622234843457.png" alt="image-20210622234843457" style="zoom:67%;" />

**git checkout newImage** ;  //切换分支

git commit;

<img src=".assets/image-20210622234824784.png" alt="image-20210622234824784" style="zoom:67%;" />

**git checkout -b branch-name**;  //创建新分支同时切换到对应分支

![image-20210622235241372](.assets/image-20210622235241372.png)

git checkout -b bugFix;

![image-20210622235232626](.assets/image-20210622235232626.png)

### 3 git merge 分支的合并 ###

太好了! 我们已经知道如何提交以及如何使用分支了。接下来咱们看看如何将两个分支合并到一起。就是说我们新建一个分支，在其上开发某个新功能，开发完成后再合并回主线。

咱们先来看一下第一种方法 —— `git merge`。在 Git 中合并两个分支时会产生一个特殊的提交记录，它有两个父节点。翻译成自然语言相当于：“我要把这两个父节点本身及它们所有的祖先都包含进来。”

两个分支，每个分支上各有一个独有的提交。这意味着没有一个分支包含了我们修改的所有内容。咱们通过合并这两个分支来解决这个问题。我们要把 `bugFix` 合并到 `master` 里

<img src=".assets/image-20210622235532822.png" alt="image-20210622235532822" style="zoom:80%;" />

**git merge bugFix** //merge 的主语是当前分支(*)，所以变化的只有当前分支

<img src=".assets/image-20210622235826774.png" alt="image-20210622235826774" style="zoom:80%;" />

首先，`master` 现在指向了一个拥有两个父节点的提交记录。假如从 `master` 开始沿着箭头向上看，在到达起点的路上会经过所有的提交记录。这意味着 `master` 包含了对代码库的所有修改。

再把master 分支合并到 bugFix

**git checkout bugFix; git merge master;**

<img src=".assets/image-20210623000023396.png" alt="image-20210623000023396" style="zoom:80%;" />

merge是**找到/创建**两个父节点公共的子节点

### 4 git rebase ###

第二种合并分支的方法是 `git rebase`。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

Rebase 的优势就是可以**创造更线性的提交历史**，这听上去有些难以理解。如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。

![image-20210623000550847](.assets/image-20210623000550847.png)

还是准备了两个分支；注意当前所在的分支是 bugFix（星号标识的是当前分支）

我们想要把 bugFix 分支里的工作直接移到 master 分支上。移动以后会使得两个分支的功能看起来像是按顺序开发，但实际上它们是并行开发的。

咱们这次用 `git rebase` 实现此目标

**git rebase master**

![image-20210623000702609](.assets/image-20210623000702609.png)

现在 bugFix 分支上的工作在 master 的最顶端，同时我们也得到了一个更线性的提交序列。

注意，提交记录 C3 依然存在（树上那个半透明的节点），而 C3' 是我们 Rebase 到 master 分支上的 C3 的副本。

现在唯一的问题就是 master 还没有更新

**git checkout master; git rebase bugFix;**

由于 `bugFix` 继承自 `master`，所以 Git 只是简单的把 `master` 分支的引用向前移动了一下而已。

<img src=".assets/image-20210623000931935.png" alt="image-20210623000931935" style="zoom:80%;" />

rebase 顾名思义，相当于更换当前分支的父节点

## 高级 ##

### 1 Head ###

在接触 Git 更高级功能之前，我们有必要先学习在你项目的提交树上前后移动的几种方法。

我们首先看一下 “HEAD”。 HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

HEAD 通常情况下是指向分支名的（如 bugFix）。在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。

git checkout c1; git checkout master; git commit; git checkout c2;

<img src=".assets/image-20210623001655885.png" alt="image-20210623001655885" style="zoom:50%;" />

实际这些命令并不是真的在查看 HEAD 指向，看下一屏就了解了。如果想看 HEAD 指向，可以通过 `cat .git/HEAD` 查看， 如果 HEAD 指向的是一个引用，还可以用 `git symbolic-ref HEAD` 查看它的指向。

**分离的HEAD**

分离的 HEAD 就是让其指向了**某个具体的提交记录而不是分支名**。在命令执行之前的状态如下所示：

HEAD -> master -> C1   //HEAD 指向 master， master 指向 C1

git checkout c1; 现在变成了 

<img src=".assets/image-20210623001907489.png" alt="image-20210623001907489" style="zoom:50%;" />

HEAR -> C1

###  2 相对引用 ###

git checkout xxx            改变HEAD的值

git branch -f  xxx  yyy   强制移动xxx分支，如果xxx是当前分支，那么HEAD也会跟着变

