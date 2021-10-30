Git fetch和git pull区别为：远端跟踪分支不同、拉取不同、commitID不同。

一、远端跟踪分支不同

1、Git fetch：Git fetch能够直接更改远端跟踪分支。

2、git pull：git pull无法直接对远程跟踪分支操作，我们必须先切回本地分支然后创建一个新的commit提交。

二、拉取不同

1、Git fetch：Git fetch会将数据拉取到本地仓库 - 它并不会自动合并或修改当前的工作。

2、git pull：git pull是从远程获取最新版本并merge到本地，会自动合并或修改当前的工作。