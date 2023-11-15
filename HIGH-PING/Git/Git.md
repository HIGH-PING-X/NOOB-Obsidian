创建本地仓库
![[Pasted image 20231113155445.png]]

```bash
#友好的查看文件
git cat-file -p 文件路径

#在当前的路径下创建仓库
git init

#从远程仓库拉取到本地(当前文件路径下)
git clone 远程仓库路径 新的仓库名(可选择添加)

#全局配置名称和邮箱地址
git congif --global user.name NOOB
git cpnfig --global user.email highping@games.com

#查看工作区的内容
git status

#将工作区的文件添加到暂存区
git add 文件名

#将暂存区内容添加到本地仓库中
git commit 添加tag(可选)

#查看提交日志
git log --oneline

#文件的删除
git add 删除的文件名

#文件的误删除(将存储区的文件恢复到工作区)
git restore 文件名
#如果不仅删除还commit了就使用下面的
git reset --hard 版本号

#创建分支(分支是基于提交的git commit -m 分支名)
git branch 分支名
#查看全部分支
git branch -v
#将创建分支和切换分支的操作合并
git checkout -b 分支名
#删除分支
git branch -d 分支名
#合并分支和文件
#先切换到一个分支
git merge 要合并的另外一个分支名

#将本地仓库的内容推送到远程仓库
git push 
#反之
git pull

```

