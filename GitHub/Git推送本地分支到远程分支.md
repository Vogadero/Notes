# Git本地分支与远程分支操作

[git上项目代码拉到本地方法_git拉代码到本地-CSDN博客](https://blog.csdn.net/Steriles_/article/details/83022608)

### 开新分支（同步远程）

> 有时候我们开发需要开一个分支，这样可以有效的并行开发

- 开分支有两种方式

  - 一种是在远程开好分支，本地直接拉下来

    ```shell
    $ git checkout -b feature-branch origin/feature-branch //	检出远程的feature-branch分支到本地
    ```

  - 一种是本地开好分支，推送到远程

    ```shell
    $ git checkout -b feature-branch //	创建并切换到分支feature-branch
    $ git push origin feature-branch:feature-branch //	推送本地的feature-branch(冒号前面的)分支到远程origin的feature-branch(冒号后面的)分支(没有会自动创建)
    ```


### 本地开发新功能

1. 运行如下的命令，基于 `master` 分支在本地创建 `goodsdetail` 子分支，用来开发商品详情页相关的功能：

   ```shell
   git checkout -b goodsdetail
   ```

2. 不放心的话，或者想查看当前分支在哪的话，可以追加命令，查看当前在哪个分支上：

   ```shell
   git branch
   ```

3. 将 `goodsdetail` 分支进行本地提交：

   ```shell
   git add .
   git commit -m "完成了商品详情页面的开发"
   ```

4. 将本地的 `goodsdetail` 分支推送到远程：

   ```shell
   git push -u origin goodsdetail
   ```

5. 将本地 `goodsdetail` 分支中的代码合并到 `master` 分支：

   ```shell
   git checkout master
   git merge goodsdetail
   git push
   ```

6. 删除本地的 `goodsdetail` 分支：

   ```shell
   git branch -d goodsdetail
   ```

7. 查看当前所有分支

   ```shell
   git branch
   ```

   