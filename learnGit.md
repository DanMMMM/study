## git常用操作
+ 进入本地库文件夹
>cd 文件夹
+ 克隆远程库
>git clone https://github.com/DanMMMM/study.git [本地目录]
+ 初始化本地库
>git init
+ 提交到本地库
>git add "文件名"
>git commit -m '提交备注'
+ 查看本地文件
>ls -al
+ 查看本地库状态
>git status
+ github中创建repository study
+ 本地仓库下关联github中的远程库
>$ git remote add origin git@github.com:DanMMMM/study.git  
origin是远程库的名字，可更改
+ 拉取github数据
>$ git pull origin master
+ 修改文件后，把本地库的内容推送到远程库中
>$ git push (-u) origin master  
上传进度信息，-u只在第一次提交时需要
## 分支
+ 创建分支，切换分支
>$ git branch '分支'  
>$ git checkout '分支'  
>$ git repo start
repo start如果存在分支则切换分支，若不存在则创建并切换分支
+ 将分支添加到github上
>$ git --set-upstream origin '分支'
+ 查看分支的文件夹列表，删除分支上的单个文件夹（首先要切换到该分支）
>dir
>$ git rm -r --cached '文件夹'
>$ git commit -m '删除备注'
>$ git push origin master
+ 克隆github中的分支
>$ git checkout -b '分支' origin/'分支'