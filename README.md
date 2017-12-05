# 多PC同步管理博客

很多人可能家里一台笔记本，公司一个台式机，想两个同时管理博客，同时达到备份的博客主题、文章、配置的目的。下面就介绍一下用github来备份博客并同步博客。

### 1. A电脑备份博客内容到github
配置.gitignore文件。进入博客目录文件夹下，找到此文件，用sublime text 打开，在最后增加两行内容/.deploy_git和/public

### 2. 初始化仓库。
在博客根目录下，在git bash下依次执行git init和git remote add origin 为远程仓库地址。

### 3. 同步到远程仓库。
gitbash下依次执行以下命令

>* git add . #添加目录下所有文件
>* git commit -m "更新说明" #提交并添加更新说明
>* git push -u origin master #推送更新到远程仓库
### 4. B电脑拉下远程仓库文件
在B电脑上同样先安装好node、git、ssh、hexo，然后建好hexo文件夹，安装好插件，（然后选做：将备份到远程仓库的文件及文件夹删除），然后执行以下命令：

>* git init
>* git remote add origin <server>
>* git fetch --all
>* git reset --hard origin/master
### 5. 发布博客后同步
在B电脑发布完博客之后，记得将博客备份同步到远程仓库
执行以下命令：

>* git add .
>* #可以用git master 查看更改内容
>* git commit -m "更新信息"
>* git push -u origin master  #以后每次提交可以直接git push
### 6. 平时同步管理
每次想写博客时，先执行：git pull进行同步更新。发布完文章后同样按照上面的 发布博客后同步 同步到远程仓库。
