# git-novel

> 之前在群里面和群友开脑洞的时候想到了这个项目，大体是类似于 git 一样的小说，有主仓库，如果到某一个情节读者不喜欢，可以拉出来一个分支自己写

## 基本思想

将小说仿照 git 的代码树，从第一章开始，中途可由任何人分支，也可以由作者自行分支，然后合并进主线。类似于 galgame 的选项，共同线（合并）与个人线（分支）

举个例子

```text
章节一
||
章节二
||
章节三 ====
||      ||
章节四   章节四（分支）
||      ||
章节五   章节五（分支）
```

## 问题

那么第一个问题来了，基础的后端如何实现

- 用 git 做基础，每本小说一个仓库

- 写轮子

我个人来说是比较纠结的，用 git 做后端意味着少量的工作，我只需要写好前端，做好按钮即可。但是这意味着我也少了很多自由性，以及需要大量的性能。就那上面那个例子来说，假如我用 git 作为后端，那么我在主仓库只能知道是哪个人拉了 fork，我就要比对两个仓库的文件校验码才能知道从哪一章开始分支的。假如我使用 git flow 的方法，那么我在为用户呈现的时候就要频繁地切换分支，这样性能的消耗会很大

第二个问题，分支

- 一章为一个节点

- 一段为一个节点

- 由作者自定义的一个情节为一个节点

- 由读者自己选择任意区域作为一个节点

这里的问题就在于，如果选择以一章为一个节点的话，那我用 git 做后端，我一章一个文件就完事，比对下文件校验码就知道哪一章有分支，但是就像上一个问题说的一样，性能消耗，也就是说，除了第一个以外，都要重新写轮子。

第三个问题，分支记录

- 数据库主从表

- JSON or XML

先来说一下这个是什么问题吧。假设我需要自己造轮子，那么我就需要有一种方法来记录分支的位置，是在哪一行的哪一个字，这里两个选项的区别就在于前后端性能花销以及可维护性。

如果使用的是数据库，那么我可以很方便地进行批量操作，但是在阅读的时候就要频繁地向后端发送询问。

如果使用的是 JSON，那么阅读的时候我只要发送一个 JSON，接下来的事情交给前端（浏览器 js）去做就好，但是批量操作会很麻烦。

所以目前的考虑是两者并用，数据库只来储存一本书的作者信息，包括分支作者，而其他信息一律储存在 JSON 中，这样节省了后端花销。

## todo
- [x] 开坑
- [ ] 写第一行代码
