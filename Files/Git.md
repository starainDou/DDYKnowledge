> #### 基础命令含义

* clone 克隆远程仓库
* init 初始化仓库
* remote 连接远程仓库
* pull 从远程仓库下拉获取新数据
* push 将本地仓库新增或修改文件上传到远程仓库
* add 添加文件或者修改文件，commit以及push之前使用
* log 当前仓库提交过的日志信息
* status 当前仓库版本状态
* commit 提交到当前仓库中
* branch 分支命令，相关增删查操作
* checkout 使用远程仓库最后一个版本完全覆盖当前仓库内容／选择分支branch
* diff 对比版本内容
* merge 合并版本内容

> #### 使用前配置

* ##### 设置基本信息
	
	###### 初始化仓库
	
	```
	# 在项目中创建.git文件
	git init
	
	```
	
	###### 设置信息	
	
	```
	# 建议局部配置(防止因为不同Email导致无法正常生成记录)
	# 设置用户名
	git config user.name "MyName"
	# 设置Email
	git config user.email "My@Email.com"
	# 显示配置信息
	git config --list
	# 开启颜色
	git config color.ui true
	# 如果全局配置加 --global
	# git config --global user.name "MyName"
	# git config --global user.email "My@Email.com"
	# git config --global --list
	# git config --global color.ui true
	# 配置选项有三种：–system，–global 和 –local
	```
	
	###### 创建忽略文件
	
	```
	touch .gitignore
	```
	
	添加忽略文件内容
	(切记,如果想忽略的文件已经进行版本管控了,再添加新的忽略,也不会起作用)
	
	
	```
	# Xcode
	.DS_Store
	build/
	*.pbxuser
	!default.pbxuser
	*.mode1v3
	!default.mode1v3
	*.mode2v3
	!default.mode2v3
	*.perspectivev3
	!default.perspectivev3
	#*.xcworkspace
	#!default.xcworkspace
	xcuserdata
	profile
	*.moved-aside
	DerivedData
	.idea/
	*.xcuserstate
	# Pods - for those of you who use CocoaPods
	#Pods
	```
	
	###### 将项目中所有修改过的工作文件提交暂存区
	
	```
	# 注意空格
	git add .
	```
	
	###### 提交新增内容
	
	```
	git commit -m 'git初始化操作'
	```
	
	###### 密钥管理
	
	```
	# 检查 SSH key 是否存在
	ls -al ~/.ssh
	# 生成新的SSH Key文件(为防止错误,特别公用电脑,最好重新生成)
	ssh-keygen -t rsa -C "My@Email.com"
	# 此时会生成id_rsa.pub(RSA加密公钥)和id_rsa(RSA加密私钥)
	# 如果没有配置过可以三个回车
	# 如果配置过，提示overwrite (y/n)? 则输入y并回车
	# 提示Enter passphrase 可以不设置密码直接回车
	# 添加key到SSH中去
	ssh-add ~/.ssh/id_rsa
	# 复制SSH key
	pbcopy < ~/.ssh/id_rsa.pub
	# 打开储存SSH key的文件夹
	open ~/.ssh
	# 如果想删除key可以把文件夹内容删除(最好别删除)
	# 将copy的key添加到远程git的配置中
	```
	
	###### 本地关联远程
	
	```
	# git remote add 仓库名称 仓库地址
	git remote add origin project@MyProject.git
	
	```
	
	###### 查看仓库名称
	
	```
	git remote
	```
	
	###### 将origin仓库推送到master分支
	
	```
	git push origin master
	```
	
	###### 回到指定版本
	
	```
	# 回到当前版本
	git reset --hard HEAD
	# 回到上上个版本
	git reset --hard HEAD^^
	# 回到往前2个版本
	git reset --hard HEAD~2
	# 回到指定版本(七位版本号)
	git reset --hard 1234567
	```
	
	###### 日志信息
	
	```
	# 查看日志版本信息
	git log
	# 查看日志七位版本号及回退信息
	git reflog
	```
	
	###### 查看项目的提交状态
	
	```
	git status
	```
	
	###### 差异化
	
	```
	git diff <file> # 比较当前文件和暂存区文件差异 git diff
	git diff <id1><id2># 比较两次提交之间的差异
	git diff <branch1>..<branch2> # 在两个分支之间比较
	git diff --staged # 比较暂存区和版本库差异
	git diff --cached # 比较暂存区和版本库差异
	git diff --stat # 仅仅比较统计信息
	```
	
	###### 设置代理
	
	```
	# socks5代理
	git config --global http.proxy 'socks5://127.0.0.1:1080' 
	git config --global https.proxy 'socks5://127.0.0.1:1080'
	// http/https 代理
	git config --global https.proxy http://127.0.0.1:1080
	git config --global https.proxy https://127.0.0.1:1080
	
	#取消
	git config --global --unset http.proxy
	git config --global --unset https.proxy
	```

> #### SourceTree设置忽略文件不生效问题

* 点击工程管理界面右上角终端按钮
* 删除缓存(注意有个实心点) ``` git rm -r --cached . ```
* 本工程忽略文件：点击右上角设置->高级->编辑gitignore
* 全局忽略文件：点击左上角菜单SourceTree->首选项(可能有的交偏好设置)->Git->编辑文件
* git add .
* git commit -m 'ignore something'


> #### 修改已提交但未推送的message

```
Git bush中执行命令 git commit --amend
然后直接修改备注保存即可。
保存方式：
1）按Esc退出编辑
2）输入 :wq （保存并退出）
不保存退出输入 :q!
```

> #### 删除已经提交但未推送的更改

```
撤销git commit，撤销git add，保留编辑器改动代码 git reset --mixed HEAD~1
撤销git commit，撤销git add，保留编辑器改动代码 git reset --soft HEAD~1
撤销git commit，撤销git add，删除编辑器改动代码 git reset --hard HEAD~1
```

> #### 从零开始关联提交到git

```
mkdir images
cd images
git init
touch README.md
git add README.md // git add .
git commit -m "first commit"
git remote add origin https://gitee.com/rainopen/images.git
git push -u origin master
```

> #### 附	
	
* [参考](https://git-scm.com/book/zh/v1/自定义-Git-配置-Git)
* [参考](http://www.cocoachina.com/articles/21163)    
* [参考](https://blog.csdn.net/hmh007/article/details/50726318)   
* [参考](https://www.jianshu.com/p/f46aa9d0dc43)
* [git自动补全](https://github.com/git/git/tree/35f6318d44379452d8d33e880d8df0267b4a0cd0/contrib/completion)  
* [gitforwindows淘宝镜像](http://npm.taobao.org/mirrors/git-for-windows/)
* [如何删除未推送的git commit？](https://blog.csdn.net/asdfgh0077/article/details/103596340) 
* [Git删除已提交但未推送的commit](https://blog.csdn.net/u013986317/article/details/107106702)