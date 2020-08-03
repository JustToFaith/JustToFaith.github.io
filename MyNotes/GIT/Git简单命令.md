#### 从网上克隆仓库

- git clone https://xxxxxx  用http网站克隆仓库



#### 本地版本管理

分为工作区和暂存区，工作区就是你在电脑上可以看到的那些文件夹；暂存区就是你提交后看不到，其实被提交到了一个隐藏文件夹下面，然后你提交到暂存区后就可以通过git管理，比如上传到github仓库，或进行版本回退和分支管理。

- git add xxx.html  就是把你文件添加到暂存区
- git add .           是把所有你刚才修改的都提交了，一次性的，不用像上面那个一个一个写、
- git diff                看暂存区里面你几次添加不同的地方
- git commit -m "提交的备注信息"  提交更改，把暂存区的文件提交到当前分支
- git status  查看你add之后，暂存库的状态
- git log      查看你的提交的历史记录
- git log --pretty=oneline            也是查看日志，不过看起好看点
- git reset --hard comit_id          回退到commit_id那一个版本，commit id 可以在日志中查看

#### 分支管理

- git branch xx  				添加分支(xx为分支名)
- git checkout xx              切换到xx分支
- git checkout -b xx         添加xx分支，并切换到xx分支，相当于前面两个命令一起（用于第一次创建分支）
- git merge --no-ff -m "备注" xxx         把xxx分支合并到当前分支
-  git push origin xxx                                 推送xxx分支到github仓库
- git pull origin xxx                                     从仓库上下拉xxx分支
- git checkout -b dev origin/dev          如果仓储上有其他分支，则第一此从仓库克隆下来，只能看到master主分支，需要用这个命令切换到其他到分支





> 上面是些简单的git命令，你自己在dev上创建一个分支，或者直接在dev上也行，我们两的代码应该不冲突。你正常写完了之后就add，然后commit。觉得差不多了就git push origin dev 推到远程仓库上，我就直接从上面拉取。如果你另外建了一个分支，就先git checkout dev切换到dev分支，然后git merge --no-ff -m "备注" xxx  把xxx分支合并到dev分支上。然后在git checkout dev推送。
>
> [廖雪峰官网](https://www.liaoxuefeng.com/wiki/896043488029600) 上面有git教程



允许合并无关的历史

git merge --no-ff -m "" dev --allow-unrelated-histories