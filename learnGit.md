## git 常用操作

- 进入本地库文件夹
  > cd 文件夹
- 克隆远程库
  > \$ git clone https://github.com/DanMMMM/study.git [本地目录]
- 拉取远程库所有分支信息
  > $ git fetch --al
  >$ git merge
- 放弃本地修改使用远程库的内容
  > $ git fetch --all
  >$ git reset --hard origin/master
- 初始化本地库
  > \$ git init
- 提交到本地库
  > $ git add "文件名"
  >$ git commit -m '提交备注'
- 查看本地文件
  > ls -al
- 查看本地库状态
  > \$ git status
- 查看修改
  > \$ git diff
- 提交历史
  > \$ git log --pretty=oneline
- 版本回退
  > \$ git reset --hard commit_id(HEAD^)
- 记录命令
  > \$ git reflog
- 撤销暂存区修改
  > \$ git checkout -- '文件'
- 删除文件
  > $ git rm '文件'
  >$ git commit -m '备注'
- github 中创建 repository study
- 本地仓库下关联 github 中的远程库
  > \$ git remote add origin git@github.com:DanMMMM/study.git  
  > origin 是远程库的名字，可更改
- 拉取 github 数据
  > \$ git pull origin master
- 修改文件后，把本地库的内容推送到远程库中
  > \$ git push (-u) origin master  
  > 上传进度信息，-u 只在第一次提交时需要

## 分支

- 创建分支，切换分支
  > $ git branch '分支'  
  >$ git checkout '分支'  
  > $ git checkout -b '分支'   
  >$ git repo start  
  > repo start 如果存在分支则切换分支，若不存在则创建并切换分支
- 查看分支
  > \$ git branch
- 合并其他分支的内容到当前分支（首先要切换到当前分支）
  > \$ git merge '分支'
- 删除分支
  > \$ git branch -d '分支'
- 将分支添加到 github 上
  > \$ git --set-upstream origin '分支'
- 查看分支的文件夹列表，删除分支上的单个文件夹（首先要切换到该分支）
  > dir
  > $ git rm -r --cached '文件夹'
  >$ git commit -m '删除备注'
  > \$ git push origin master
- 克隆 github 中的分支
  > \$ git checkout -b '分支' origin/'分支'
