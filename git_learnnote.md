##### 安装git
sudo apt-get install git

##### 创建版本库
- 创建git仓库目录
   - makdir xxx 
   - git init
   - git add readme.txt
   - git commit -m "描述"
   - 
注：add 可以反复多次添加
    commit可以一次提交很多文件

##### 查看工作区状态
- 查看本地代码相较于工作区和服务器是否有改动
   - git status
- 查看本地代码相较于工作区和服务器改动的地方
   - git diff
 
##### 版本回退
- 显示从最近到最远的提交日志
    - git log
    - git log --pretty=oneline
- 启动版本回退
    - git reset --hard HEAD^
    - git reset --hard HEAD^^
    - git reset --hard HEAD~3
    - git reset --hard xxxx     //xxxx即为文件commit id
    - 
    - git checkout 三个用法： 取消工作区修改/修改分支/修改同分支version
    - git checkout -- xxxx     //xxxx即为文件commit id //如果没有-- 就是切换成分支
    - git checkout master   //回到回退之前，不一定非要指定master,可以指定别的
- 查看记录的每次命令
    - git reflog
- 小结
    - HEAD指向的版本就是当前版本
    - 穿梭前使用git log 查看递交版本，以便确定要回到哪里
    - 穿梭后使用git reflog 查看历史命令，以便回到未来
   
##### 工作区和暂存区
- 初始化指定的文件目录即为工作区
- 工作区目录.git为Git的版本库
- git add 实际是把文件修改添加到暂存区
- git commit 把暂存区的所有内容提交到当前分支

##### 撤销修改
- 丢弃工作区的修改
   - git checkout -- xxxx  //xxxx即为文件名
      - 还没有add ,撤销后回到版本库一模一样的状态
      - 已经add添加到缓存区，然后又做了修改。现在回到暂存区的状态
     
- add后还没有提交
   - git reset HEAD xxxx    //xxxx即为文件名
      - 可以把暂存区的修改撤销掉，重新放回工作区
      - 然后git checkout 文件名

##### 删除文件
- git rm xxxx    //xxxx即为文件名称
   - 从本地删除文件后，使用本命令删除暂存区
   - 然后使用 git commit -m "描述"删版本库
   - 如果是误删，则同样使用 git checkout -- xxxx //xxxx即为文件名
   
##### 和远程库的交互
###### 本地库和远程库关联
- git remote add origin git@172.16.1.220:/opt/ben/dpdk.git
- git remote add origin git@172.16.1.220:/opt/ben/rule-machine.git
   - origin 是自己给起得远程库名称
###### 将本地库推送到远程
- git push -u origin master
   - 第一次推送使用 -u参数，不但会把本地的mater分支推送到远程，还会把本地的master分支和远程的master分支关联起来，以后的推送拉取就可以简化命令。
- git push origin master
   - 把本地master分支的最新修改推送至git
###### 从远程库克隆
- git clone git@172.16.1.220:/opt/ben/dpdk.git

##### 分支管理
###### 创建分支，然后切换到dev分支
- git checkout -b dev
   - git branch dev + git checkout dev
   - git branch    //列出所有分支，查看当前分支
   - git branch 分支名字  //建立分支
###### 把dev分支的成果合并到master上
- git checkout master
- git merge dev     //合并指定分支到当前分支
###### 删除分支
- git branch -d dev
###### 查看分支合并图
- git log --graph
###### 合并dev,禁用fast forward
- git merge --no-ff -m  "merge with no-ff" dev
###### 丢弃一个没有被合并过的分支
- git branch -D xxxx //xxxx 即为分支名称
- 
###### 隐藏工作区和分支部分合并
- git stash //将改动的没有add 的工作区隐藏
- git stash list // 显示所有隐藏的工作区
- git stash apply stash@{0}// 应用工作区
- git stash drop
- git stash pop //弹出并删除
- git cherry-pick 4c805e2 // 后边编号是在另一个分支中的修改后提交编号//可以保证只改变本次修改

##### 多人协作
###### 查看远程仓库信息
- git remote
- git remote -v
###### 推送分支
- git push origin master
- git push origin dev
###### 抓取分支
- git clone git@github.com:michaelliao/learngit.git
   - 默认情况下，只能看到master分支
   - git branch查看
###### 抓取服务器除master的其他分支
- git checkout -b dev /origin/dev
###### 解决分支冲突/设置dev和origin/dev的连接/
- git branch --set-upstream-to=origin/dev dev
- git branch --set-upstream branch-name origin/branch-name
- git pull
- 正常修改，提交
- git push origin dev
- 
##### 创建和使用标签
- git tag - a "tagname" -m "描述" xxx //xxx是commit 号码，默认是HEAD
- git tag         //显示所有tag，按照字母排序
- git show "tagname"  //显示tagname 详细信息
- 
- git push Origin "tagname" //推送一个本地标签
- git push Origin --tags //推送全部本地标签
- git tag -d "tagnmae"  //删除一个本地标签
- git push Origin :refs/tags/"tagname" 可以删除一个远程标签。
- 
##### 使用 tips
- 忽略某些文件时，需要编写.gitignore；
- .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理
- 
##### 搭载服务器
1. 安装git
   - sudo apt-get isntall git
2. 创建一个gi用户，用来运行git服务
   - sudo adduser git
3. 创建证书登录
   - 收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
4. 初始化Git仓库
   - sudo git init --bare sample.git
   - Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git 
   - sudo chown -R git:git sample.git
5. 克隆远程仓库
   - git clone git@server:/srv/sample.git
6. 管理公钥/权限
   - Gitosis/Gitolite


