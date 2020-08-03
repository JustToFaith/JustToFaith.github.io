# Git 命令完整版

[TOC]



### 环境配置

1. 设置用户名和邮箱

   ```shell
   git config --global user.name “kevin”
   git config --global user.email “justtofaith@gmail.com”
   ```

2. 查看配置信息

   ```shell
   git config --list
   git config user.name
   ```



### 获取仓库

#### 本地创建

1. 在电脑的任意位置创建一个空目录（例如repo1）作为我们的本地Git仓库

2. 进入这个目录中，点击右键打开Git bash窗口

3. 执行命令`git init`

#### 远程获取

1. 从远程仓库获取

   ```shell
   git clone git@github.com:Kevin-Liz/MyNotes.git
   ```



### 本地仓库操作

1. 查看文件状态

   ```shell
   git status
   git status -s
   ```

2. 将文件加入暂存区

   ```shell
   git add file.txt
   git add .    # 添加所有
   ```

3. 将暂存区的文件取消

   ```shell
   git reset file.txt
   ```

   

4. 将暂存区文件提交到本地仓库

   ```shell
   git commit -m "日志信息"
   git commit -a -m "日志信息"	 # 提交已跟踪但未提交到暂存区的文件
   ```

5. 删除文件

   ```shell
   git rm file.txt
   ```

6. 将文件添加到忽略列表

   新建`.gitignore`文件，然后需要忽略的文件添加到此文件中。例如：

   ```
   # no .a files
   *.a
   # but do track lib.a, even though you're ignoring .a files above
   !lib.a
   # only ignore the TODO file in the current directory, not subdir/TODO
   /TODO
   # ignore all files in the build/ directory
   build/
   # ignore doc/notes.txt, but not doc/server/arch.txt
   doc/*.txt
   # ignore all .pdf files in the doc/ directory
   doc/**/*.pdf
   ```

7. 查看日志记录

   ```shell
   git log
   ```



### 远程仓库操作

1. 查看远程仓库

   ```shell
   git remote
   git remote -v 
   ```

2. 添加远程仓库

   ```shell
   git remote add remoteName url 
   ```

3. 从远程仓库克隆

   ```shell
   git clone url
   ```

4. 移除无效的远程仓库

   ```shell
   git remote rm remoteName
   ```

   注意：此命令只是从本地移除远程仓库的记录，并不会真正影响到远程仓库

5. 从远程仓库抓取

   git fetch 是从远程仓库获取最新版本到本地仓库，不会自动merge

   ```shell
   git fetch url
   ```

6. 从远程仓库拉取

   git pull 是从远程仓库获取最新版本并merge到本地仓库

   ```shell
   git pull url
   ```

7. 推送到远程仓库

   ```shell
   git push remoteName branch
   ```



### 分支

1. 列出所有本地分支

   ```shell
   git branch
   ```

2. 列出所有远程分支

   ```shell
   git branch -r
   ```

3. 列出所有本地与远程分支

   ```shell
   git branch -a
   ```

4. 创建分支

   ```shell
   git branch branch-name
   ```

5. 切换分支

   ```shell
   git checkout branch-name
   ```

6. 推送至远程仓库分支

   ```shell
   git push remoteName branch-name
   ```

7. 合并分支

   ```shell
   git merge brance-name
   ```

8. 删除本地分支

   ```shell
   git branch -d branch-name
   git branch -D branch-name # 强制删除有修改的分支
   ```

9. 删除远程分支

   ```shell
   git push origin -d branch-name
   ```

   

### 标签

1. 列出已有标签

   ```shell
   git tag
   ```

2. 查看标签信息

   ```shell
   git show [tag]
   ```

3. 新建标签

   ```shell
   git tag tagName
   ```

4. 推送标签至远程仓库

   ```shell
   git push remoteName tagName
   ```

5. 检出标签

   新建一个分支，并将标签内容赋给这个分支

   ```shell
   git checkout -b branch-name tagName
   ```

6. 删除本地标签

   ```shell
   git tag -d tagName
   ```

7. 删除远程标签

   ```shell
   git push origin :refs/tags/[tagName]
   ```

   