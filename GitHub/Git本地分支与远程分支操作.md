# Git本地分支与远程分支操作

[git上项目代码拉到本地方法](https://blog.csdn.net/Steriles_/article/details/83022608)

[vscode无法获取切换git上最新的远程分支解决办法_vscode git 更新远程分支信息失效](https://blog.csdn.net/gxh0816/article/details/124659164)

[vscode切换分支及合并分支操作教程](https://blog.csdn.net/weixin_45687201/article/details/138676302)

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

### 远程有新分支，本地看不到，如何更新分支

```shell
# 更新远程主机origin 整理分支
git remote update origin --prune 
Fetching origin
# 这里会罗列出新分支信息
```

### 本地开发新功能

1. 运行如下的命令，基于 `master` 分支在本地创建 `goodsdetail` 子分支，用来开发商品详情页相关的功能：

   ```shell
   git checkout -b goodsdetail
   ```

2. 不放心的话，或者想查看当前分支在哪的话，可以追加命令，查看当前在哪个分支上：

   ```shell
   # 查看所有本地分支
   git branch
   
   # 列出所有远程主机
   git remote 
    
   # 更新远程主机origin 整理分支
   git remote update origin --prune 
    
   # 列出远程分支  
   git branch -r  
    
   # 查看本地分支和远程分支对应关系    
   git branch -v
   
   # 列出所有本地分支和远程分支
   git branch -a
    
   # 新建本地分支gpf与远程gpf分支相关联
   git checkout -b gpf origin/gpf    
   
   # 如果命令行退不出，输入:q
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

8. 或者在步骤6创建 `Pull Reques`t：

   - 在 GitHub 或 GitLab 上创建 PR，进行代码审查。

9. 合并 PR 并删除分支：

   - 合并 PR 到 `master` 分支。

   - 删除远程功能分支：

     ```shell
     git push origin --delete goodsdetail
     ```

### 本地 `dev` 分支最新代码合并到远程新分支

为了将本地 `dev` 分支的最新代码合并到远程新创建的 `hcms-1.33` 分支，你可以按照以下步骤操作：

1. **确保本地代码是最新的**：

   ```shell
   git checkout dev
   git pull origin dev
   ```

2. **切换到远程 `hcms-1.33` 分支**：

   - 如果你本地看不到远程新创建的 `hcms-1.33` 分支，可以先更新远程主机 `origin` 整理分支：

     ```shell
     # 更新远程主机origin 整理分支
     git remote update origin --prune 
      
     # 列出远程分支  
     git branch -r  
      
     # 查看本地分支和远程分支对应关系    
     git branch -v
     
     # 列出所有本地分支和远程分支
     git branch -a
     ```

   - 然后新建本地分支`hcms-1.33`与远程`hcms-1.33`分支相关联：

     ```shell
     # 新建本地分支hcms-1.33与远程hcms-1.33分支相关联
     git checkout -b hcms-1.33 origin/hcms-1.33
     ```

   - 如果你已经有本地的`hcms-1.33`分支，可以直接切换：

     ```shell
     git checkout hcms-1.33
     ```

3. **本地创建并切换到`hcms-1.33`分支后，`pull`拉取同步一下，然后将 `dev` 分支的最新代码合并到 `hcms-1.33` 分支**：

   ```shell
   git pull
   git merge dev
   ```

4. **解决可能的冲突**：

   - 如果在合并过程中出现冲突，Git 会提示你哪些文件有冲突，你需要手动解决这些冲突。

   - 解决冲突后，添加已解决的文件：

     ```shell
     git add < conflicted-file >
     ```

5. **提交合并后的代码**：

   ```shell
   git commit -m "Merge latest changes from dev into hcms-1.33"
   ```

6. **推送合并后的代码到远程 `hcms-1.33` 分支**：

   ```shell
   git push origin hcms-1.33
   ```