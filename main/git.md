Git study notes：

1. ##### git组成部分

   - git的操作的步骤描述：

     ```
     .git是本地的版本库，版本库中包含三部分：stage暂存区，head指向master分支的指针，master;
     修改的部分为工作区；
     
     提交的步骤分两步:
     第一步：是将文件放入暂存区，git add ；
     第二步：是将暂存区的文件提交到master分支中，git commit操作；
     ```

     git local repository 结构图：

     ![本地仓库结构图](..\pic\git\git本地仓库组成.jpg)

   - git管理的是修改，不是管理的文件

2. ##### git常用基本操作：

   - 创建本地仓库

     ```
     git init
     ```

   - 添加文件

     ```
     git  add  <文件名>     （可以多个文件）
     
     git  add .  添加所以文件
     ```

   - 提交修改

     ```
     git commit  -m  <多提交内容的描述>
     ```

   - 查看工作区状态

     ```
     git status 
     ```

   - 查看文件的不同

     ```
     git diff
     
     git diff HEAD --文件名   查看与最新版本的文件有什么不同
     ```

   - 查看提交历史

     ```
     git log
     
     git log --pretty=oneline   一行查看
     
     $ git log --graph --pretty=oneline --abbrev-commit   查看分支情况
     
     git log --graph 查看分支情况
     ```

   - 版本回退

     ```
     git  reset --hard HEAD^
     
     git  reset --hard  <commit id>
     ```

     备注：

     - 回退到上一版本，HEAD^，上上版本，HEAD^^；

     - 回退到指定版本，commit Id（commit id 是每次提交根据 内容用SHA算法生成的唯一的ID）

   - 查看每一步操作

     ```
     git reflog
     ```

   - 丢掉工作区的修改

     ```
     git checkout -- file  撤销工作区的修改（到上次提交的版本内容）
     
     git reset HEAD readme.txt  撤销暂存区的修改
     ```

   - 删除文件

     ````
     git rm <文件名>
     ````

3. ##### 远程仓库

   - 生成SSH keys

     ```
     $ ssh-keygen -t rsa -C "youremail@example.com"   
     ```

   - 绑定远程仓库

     ```
     git remote add origin git@github.com:michaelliao/learngit.git
     ```

   - 推送到远程仓库

     ```
     第一次：git push -u origin master  （-u  参数会将本地master分支和远程master分支关联起来）
     非第一次：git push origin master
     ```

   - 克隆远程仓库

     ```
     要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。
     
     eg: git clone git@github.com:michaelliao/gitskills.git
     ```

     **备注：Git支持多种协议，包括`https`，但`ssh`协议速度最快。**
     
   - 将本地分支与远程分支绑定

     ```
     git branch --set-upstream-to=origin/dev dev
     ```

   4. 查看远程库信息

      ```
   git remote -v
      
      ```

   git remote
      ```
   
   - 创建与远程分支相对应的本地分支
   
      ```
      git checkout -b branch-name origin/branch-name

      ```
   
      ```

4. ##### 分支管理

   **git分支的概念：**

   ​	[git分支详解](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424?_blank)

   - git中的每次提交会构成一条时间线，head指向当前分支master，master指向当前时间线都最后一次提交的时间点。

   - git中的默认分支为master，master其实是一个指针，版本库里有head指针，head指向当前分支。

   - 新建的分支，就是创建一个新的指针，然后将head指针指向它，然后切换到它，就可以使用新的分支，master分支不会变。

     ![默认分支图](..\pic\git\默认分支.png)

     ![新建分支](..\pic\git\新建并切换分支.png)

     ![](..\pic\git\新分支提交.png)

     ![](..\pic\git\主分支合并子分支.png)

   

   2. ###### 创建分支

      ```
      git checkout -b dev    创建dev分支并切换到dev分支
      相当于以下两步：
      git branch dev     创建分支
      git checkout dev   切换分支
      git switch -c dev  切换分支
      git switch master 
      
      git branch  查看分支
      ```

   3. ###### 合并分支

      ```
      git merge dev   将dev分支合并到当前分支
      ```

   4. ###### 删除分支

      ```
      $ git branch -d dev   
      ```

   5. ###### 分支管理策略

      - 默认策略  （Fast forward模式）:就是将主分支的指针指向子分支的merge的时间点

        ```
        git merge dev 
        ```

      - 普通模式：将子分支的内容合并到主分支上后，重新新增一个commit点，提交；

        ```
        git merge --no-ff -m "merge with no-ff" dev
        ```

   5. ##### Bug分支

      - 将工作区的内容存下来：

        ```
        git stash
        ```

      - 查看暂存的工作区内容列表

        ```
        git stash list
        ```

      - 恢复暂存的工作区内容

        ```
        方式一：先恢复，再删除
        git stash apply  <stash@{0}>
        git stash drop
        方式二：恢复同时删除
        git stash pop
        ```

      - 将其他分支提交的某个点，复制到当前分支并提交

        ```
         git cherry-pick <commit Id>
        ```

   6. 强行删除分支

      ```
      普通删除分支； git branch -d <name>
      当此分支还未合并时，就会
      git branch -D <name>
      ```

5. 标签Tag

   - 打新的tag

     ```   
     git tag <tagname>   默认在Head处打tag
     git tag <tagname>  <commit id>
     git tag -a <tagname> -m "描述"    打带描述的tag
     ```

   - 查看tag

     ```
     git tag
     git show <tagName>  查看tag的信息
     ```

   - 删除Tag

     ```
     git tag -d <tagName>
     ```

   - 推送到远程仓库

     ```
     推送单个tag到远程：git push origin <tagName>
     一次性将本地的tag全部推送到远程：git push origin --tags
     ```

   - 删除远程仓库中的tag

     1. 先删除本地的tag

        ```
        git tag -d <tagName>
        ```

     2. 在删除远程仓库的tag

        ```
        git push origin :refs/tags/v0.9
        ```

        

6. ##### 常用的vi命令

   - 编辑模式 切换到 一般模式

   - 一般模式 切换到 编辑模式
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

