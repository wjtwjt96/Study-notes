Git的基本用法：

1. 创建本地仓库

   ```
   git init
   ```

2. 添加文件

   ```
   git  add  <文件名>     （可以多个文件）
   
   git  add .  添加所以文件
   ```

3. 提交修改

   ```
   git commit  -m  <多提交内容的描述>
   ```

4. 查看文件的不同

   ```
   git diff
   
   git diff HEAD --文件名   查看与最新版本的文件有什么不同
   ```

5. 查看提交历史

   ```
   git log
   
   git log --pretty=oneline   一行查看
   
   $ git log --graph --pretty=oneline --abbrev-commit   查看分支情况
   
   git log --graph 查看分支情况
   ```

6. 版本回退

   ```
   git  reset --hard HEAD^
   
   git  reset --hard  <commit id>
   ```

   备注：

   - 回退到上一版本，HEAD^，上上版本，HEAD^^；

   - 回退到指定版本，commit Id（commit id 是每次提交根据 内容用SHA算法生成的唯一的ID）

7. 查看每一步操作

   ```
   git reflog
   ```

8. 丢掉工作区的修改

   ```
   git checkout -- file  撤销工作区的修改（到上次提交的版本内容）
   
   git reset HEAD readme.txt  撤销暂存区的修改
   ```

9. 删除文件

   ```
   git rm <文件名>
   ```

10. 生成SSH keys

    ```
    $ ssh-keygen -t rsa -C "youremail@example.com"   
    ```

11. 绑定远程仓库

    ```
    git remote add origin git@github.com:michaelliao/learngit.git
    ```

12. 推送到远程仓库

    ```
    第一次：git push -u origin master  （-u  参数会将本地master分支和远程master分支关联起来）
    非第一次：git push origin master
    ```

13. 克隆远程仓库

    ```
    要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。
    
    eg: git clone git@github.com:michaelliao/gitskills.git
    ```

    **备注：Git支持多种协议，包括`https`，但`ssh`协议速度最快。**

14. git的操作的步骤描述：

    ```
    .git是本地的版本库，版本库中包含三部分：stage暂存区，head指向master分支的指针，master;
    
    提交的步骤分两步:
    第一步是将文件放入暂存区，git add ；
    第二步，是将暂存区的文件提交到master分支中，git commit操作；
    ```

15. git管理的是修改，不是管理的文件

16. git分支的概念

    - git中的每次提交会构成一条时间线，head指向当前分支master，master指向当前时间线都最后一次提交的时间点。

    - git中的默认分支为master，master其实是一个指针，版本库里有head指针，head指向当前分支。git中的分支其实就是head 
    - 新建的分支，就是创建一个新的指针，然后将head指针指向它，然后切换到它，就可以使用新的分支，master分支不会变。

    [git分支](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424)

17. 创建分支

    ```
    git checkout -b dev    创建dev分支并切换到dev分支
    相当于以下两步：
    git branch dev     创建分支
    git checkout dev   切换分支
    git switch -c dev  切换分支
    git switch master 
    
    git branch  查看分支
    ```

18. 合并分支

    ```
    git merge dev   将dev分支合并到当前分支
    ```

19. 删除分支

    ```
    $ git branch -d dev
    ```

20. 常用的vi命令

    - 一般模式切换到编辑模式
      - i:进入插入模式，在光标前插入 ；I：是在第一个非空格符处插入
      - a:进入插入模式，在光标下一个字符插入；A ：是在所在行最后一个字符插入
      - o:进入插入模式，在下面一行插入 ：O：是在上面一行出入
      - r:进入替换模式，类似于insert键

    - 一般模式到命令模式
      - :w 保存
      - :w! 强制保存
      - q 退出
      - q! 强制退出
      - :wq :x 保存并退出
      - ZZ 保存并退出
      - :set number 显示行号
      - :set nonu 取消显示行号

