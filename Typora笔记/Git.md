# Git

作用:用来将本地库推送到远端库,多人协同开发

## 版本库的三个区域

* 工作区(代码编辑区域)
* 缓存区(修改待提交区)
* 仓库区(代码保存区)

## 基本操作

* git init   (初始化本地库)
* git add -A (将所有修改后的代码提交到缓存区)
* git commit -m '注释'  (将缓存区的代码,提交到本地仓库)

## 常用命令

* git status 状态查看
* git diff  查看工作区和暂存区的差异
* git diff --cached 查看暂存区和仓库区的差异
* git log --oneline  查看历史记录(版本号) 注:只能查看当前版本之前的
* git reflog  查看所有的操作记录(找不到版本号的情况下)
* git ls-files  查看暂存区的文件内容
* git rm --cached  文件名      :可以删除版本库中的文件
* git restore --staged  文件名        :撤销命令
* git branch name   创建分支       -name-  为分支名
* git branch       查看分支
* git checkout name     切换分支      -name-   为分支名
* git merge name     合并分支   将指定分支合并到当前分支
* git branch -d name    删除分支     -name-  为分支名
* git checkout -b name    创建并切换分支   -name-  为分支名

<span style="color:red;">注: 每次切换分支前,提交工作区的修改</span>

常见错误

```js
fatal: remote origin already exists.
远程起源已存在 错误
解决办法:
删除远程:
	git remote rm origin #删除
    然后再添加远程库
```





## 配置忽略文件

项目开发过程中有些文件不应该存储到版本库中,这个时候需要配置忽略这些文件

常见情况:

1. 临时文件
2. 多媒体文件
3. 编辑器生成的配置文件(.idea)
4. npm安装的第三方模块

**配置忽略**

在git中需要创建一个文件     <span style='color:red;'>.gitignore</span>    配置忽略  与  .git  文件同级

* 文件夹名         忽略指定文件夹
* *.js                   忽略所有以.js为后缀的文件
* /1.html            只忽略当前文件夹下的文件

## 将不必要的文件加入到了版本库中如何补救

1. git rm --cached name   将库中的文件删除  -name-  为文件名
2. 在 .gitignore  中配置忽略
3. add -A   和   commit  -m  提交即可

## 分支冲突

当多个分支修改同一个文件后,合并分支的时候就会产生冲突

##### 解决冲突  和  定位冲突文件

1. git status     可以定位产生冲突的文件
2. 将冲突的内容修改为最终正确的结果
3. git add -A        git commit -m         提交即可

# Git Hub

git hub 是一个git仓库管理网站     可以创建远程中心仓库,为多人合作开发提供便利

## 使用流程

#### 本地有仓库

1. 远程仓库管理:      git remote add  仓库别名  仓库url地址
2. 确认代码已经提交到本地仓库
3. 将本地仓库内容推送到远程中心仓库        git push -u 仓库别名  本地仓库分支



#### 本地没有仓库

1. 克隆仓库 git clone 远端仓库url地址
2. 修改代码
3. 本地提交到本地仓库
4. 推送到远程中心库

<span style="color:red;">注:在提交之前一定要进行一次本地仓库更新  git pull 然后在进行git push提交</span>