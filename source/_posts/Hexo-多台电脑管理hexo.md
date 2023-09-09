---
title: Hexo-多台电脑管理hexo
date: 2023-09-09 22:36:20
tags: Hexo，Hexo管理
categories: Hexo
cover: https://pic.imgdb.cn/item/64fc841d661c6c8e54043dc0.png
---

最初在公司使用`hexo`搭建了自己的博客网站，并且部署到了`gitHub`上。但是回到家我想更新我自己博客却发现`gitHub`上的目录却是`hexo d`命令之后生成的`.deploy.git`文件夹下的目录

远程仓库的目录

![image](https://s2.loli.net/2023/09/10/inl42bzT6CEwMDv.png)

我本地的目录
![image](https://s2.loli.net/2023/09/10/PZ12k5YHaiLtmS8.png)

那么如何解决我在公司和家更新博客的痛点呢？我的思路是创建一个新的分支`hexo`分支去存储，而`main`分支还是部署博客的分支。

1. 确保自己`github`上已经有了自己的博客
![image](https://s2.loli.net/2023/09/10/inl42bzT6CEwMDv.png)

2. 创建一个新的分支`hexo`
![image](https://s2.loli.net/2023/09/10/mz7Fu8vKsyi2I1c.png)
![image](https://s2.loli.net/2023/09/10/9wo5ZIP2S7OH8Ap.png)
![image](https://s2.loli.net/2023/09/10/UITlfOBvKZ7saGx.png)

3. 将`hexo`分支设置为仓库的默认分支
![image](https://s2.loli.net/2023/09/10/vfICRjyD2Hc93QV.png)
![image](https://s2.loli.net/2023/09/10/34jf5oxYaNXepJb.png)

4. 使用`git clone`克隆`hexo`分支的代码，克隆下的文件夹名称应该是`username.github.io`

5. 将除了`.git`文件其他文件全部删除，替换自己本地博客的文件，替换之后的文件目录如下
![image](https://s2.loli.net/2023/09/10/PZ12k5YHaiLtmS8.png)

6. 将`themes`中主题中如果存在`.git`文件删除，不存在则省略这步，因为一个`git`仓库不能包含另一个git仓库，提交主题文件回失败。

7. 现在就是要提交`hexo`分支的文件，执行`git add .``git commit -m "提交信息"``git push`即可将自己写博客的所有文件提交到远程了，现在可以看到远程分支的差异了。
![image](https://s2.loli.net/2023/09/10/IzneHd764YR2O1B.png)
用来部署的分支
![image](https://s2.loli.net/2023/09/10/pBt9KxOHl5dWmhy.png)

结下来就可以随时在家或者公司更新自己的博客啦！🤪
