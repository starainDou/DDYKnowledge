本文对应github地址[CocoaPods](https://github.com/starainDou/DDYKnowledge/blob/master/Files/CocoaPods.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/starainDou/DDYKnowledge)


> #### 一、源设置

1. 查看源，终端输入: ``` sudo gem sources -l ```
2. 删除已有的源，如: ``` sudo gem sources --remove https://rubygems.org/ ```
3. 添加国内镜像，如: ``` sudo gem sources --add https://gems.ruby-china.com```
4. 看是否真的添加上: ``` sudo gem sources -l ```
5. 若出现 ``` https://gems.ruby-china.com ``` 则添加成功

> #### 二、升级(按需选中执行)

* ##### 升级homebrew 

	1. 查看版本 ``` brew --version ```
	2. 卸载brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```  
	3. 安装brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
	4. 查看版本 ``` brew --version ``` 
	5. 安装目录	 ``` which brew ```

* ##### 升级rvm
	
	1. 查看版本 ``` rvm --version ```
	2. 升级RVM ``` curl -L https://get.rvm.io | bash -s stable ```
	3. 生效RVM ``` source ~/.rvm/scripts/rvm ```
	4. 重载RVM ``` rvm reload ```
	5. 查看版本 ``` rvm --version ```
	6. 安装目录 ``` which rvm ```

	```
	rvm use ruby-3.0.0
	```

* ##### 升级ruby

	1. 查看版本 ``` ruby --version ```
	2. 已知版本 ``` rvm list known ```
	3. 安装指定 ``` rvm install 2.6.3 ``` 或者 ``` rvm install ruby-2.6.3 ```
	4. 已安列别 ``` rvm list ```
	5. 设定默认 ``` rvm use 2.6.3 --default ```
	6. 删除多余 ``` rvm remove 2.0.0 ```
	7. 安装目录 ``` which ruby ```

* ##### 手动安装ruby
	
	1. 下载ruby [ruby下载地址](http://www.ruby-lang.org/en/downloads/)
	2. 进入解压文件夹执行 ``` ./configure ```
	3. 编译 ``` sudo make ```
	4. 安装 ``` sudo make install ```

* ##### 升级openssl 
	
	1. 查看版本 ``` openssl version ```
	2. 升级版本 ``` brew upgrade openssl ```
	3. 链接新版 ``` brew link openssl --force ```
	4. 查看版本 ``` openssl version ```
	5. 安装目录 ``` which openssl ```

```
$ echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.zshrc
$ source ~/.zshrc
```

* ##### 升级gem
	
	1. 查看版本 ``` gem --version ```
	2. 升级版本 ``` gem update --system ``` 
	3. 查看版本 ``` gem --version ```

* ##### 升级cocoaPods
	
	1. 查看版本 ``` pod --version ```
	2. 升级版本 ``` sudo gem update cocoapods ```
	3. 查看版本 ``` pod --version ```
	4. 安装目录 ``` which pod ```

> #### 三、安装

* 安装xcodeproj ``` sudo gem install xcodeproj ``` 
* 安装cocoapods ``` sudo gem install cocoapods ```
* 安装pre版 ``` sudo gem install cocoapods --pre ```
* 执行setup ``` pod setup ```

> #### 四、错误解决
	
* 权限问题  

	- 添加 sudo
	- 关闭SIP
	
		1. 重启系统，按住 Command + R 进入恢复模式；
		2. 点击顶部菜单栏 实用工具 中的 终端；
		3. 输入 ```csrutil disable``` 来禁用 SIP 保护机制；
		4. 重启系统
		5. 如果想重新开启按上述步骤，然后执行 ```csrutil enable```

* undefined method 'size' for nil :NilClass(No MethodError)

	- 删除缓存
		
		1. 查看gem环境: ``` gem env ```
			
			显示中找到Gem Paths
			
			```
			- GEM PATHS:
     			- /Library/Ruby/Gems/2.3.0
     			- /Users/zsl/.gem/ruby/2.3.0
			```
		2. 删除对应目录下cache
		
			进入第一个目录 ``` cd /usr/local/ruby/lib/ruby/gems/2.1.0 ```    
			删除第一个目录cache  ``` rm -rf cache ```  
			进入第二个目录 ``` cd /home/vagrant/.gem/ruby/2.1.0 ```
			删除第二个目录cache  ``` rm -rf cache ``` 
			
		3. 回到用户目录: ``` cd ~ ```

* error: RPC failed; curl56SSLRead()returnerror -36

	- 终端输入 ``` sudo xcode-select --print-path ``` 查看Xcode路径
	- 复制路径，执行 ```sudo xcode-select -switch 路径 ```     
		如： ```sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer ```
	- 执行  ```sudo rm -fr ~/.cocoapods/repos/master  ``` 删除repos目录下master
	- 再执行  ```pod setup  ```
	- 如果路径是 ``` /Library/Developer/CommandLineTools ```可切换到Xcode.app路径

* Operation not permitted - /usr/bin/xcodeproj
	
	自定义GEM_HOME
	
	命令行操作：
	- 建立目录 ``` mkdir -p $HOME/Software/ruby ``` 
	- 添加路径 ``` export GEM_HOME=$HOME/Software/ruby ``` 
	- 执行安装xcodeproj ``` gem install xcodeproj ``` (出现 1 gem installed)
	- 指定目录安装cocoapods ``` sudo gem install -n /usr/local/bin cocoapods ``` 
	- 添加路径 ``` export PATH=$PATH:$HOME/Sofware/ruby/bin ``` 
	- 查看版本 ``` pod --version ``` 

* RPC failed; curl 56 SSLRead() return error -9806

	终端依次执行下面命令(不包括美元符号)
	
	```
	$ brew remove git
	$ brew remove curl
	$ brew install openssl
	$ brew install --with-openssl curl
	$ brew install --with-brewed-curl --with-brewed-openssl git
	```
* 搜索不到存在的某个库(特别是刚提交的)

	执行 ``` rm /Users/zsl/Library/Caches/CocoaPods/search_index.json ``` zsl改成自己的

> #### 四、使用cocoaPods

* 进入工程目录 ``` cd ~/DeskTop/TestProject  ``` 
* 建立Podfile:  ``` touch Podfile ``` (必须Podfile)
* 搜索三方库：``` pod search AFNetworking ``` (复制内容 ‘AFNetworking’， ‘~>2.5.3’)
* 打开Podfile:  ``` open -e Podfile ``` 并粘贴复制的内容
* Podfile格式如下
	
	```
	# 私有
	source 'http://git.idengyun.com/iOSTeam/KNSpec.git'
	# 三方	
	source 'https://github.com/CocoaPods/Specs.git'
	
	platform :ios, '8.0'
	
	inhibit_all_warnings!
	
	# 如果@import Charts; 方式导入可以不用use_frameworks!生成库文件
	# use_frameworks!
	
	# 每个工程使用自己的cocopods版本
	# gem 'cocoapods', '1.7.5'
	
	xcodeproj 'TestProject.xcodeproj'
	
	target 'Target1' do
	    
	    pod 'DDYQRCode', '~> 1.1.1'
	end
	
	target 'Target2' do
	    
	    pod 'DDYCamera', '~> 1.1.1'
	end
	
	=begin
	这里写多行注释
	=end
	
	```
	
* 执行 ``` pod install --verbose --no-repo-update ```     
* 需要更新才用此步骤，更新 ``` pod update --verbose --no-repo-update ``` 
* 若新tag没拉取下来就删除缓存 ``` ~/Library/Caches/CocoaPods/Pods/Release ```

> #### 五、让自己的库支持cocoapods管理

* 进入工程目录
* 建立 .podspec 文件 ``` pod spec create DDYQRCode ```
* 填写内容 
	
	```
	Pod::Spec.new do |ddyspec|
	    ddyspec.name         = 'DDYQRCode'
	    ddyspec.version      = '1.1.1'
	    ddyspec.summary      = '二维码/条形码生成'
	    ddyspec.homepage     = 'https://github.com/RainOpen/DDYQRCode'
	    ddyspec.license      = 'MIT'
	    ddyspec.authors      = {'Rain' => '634778311@qq.com'}
	    ddyspec.platform     = :ios, '8.0'
	    ddyspec.source       = {:git => 'https://github.com/RainOpen/DDYQRCode.git', :tag => ddyspec.version}
	    ddyspec.source_files = 'DDYQRCode/DDYQRCode/*.{h,m}'
	    ddyspec.resource     = 'DDYQRCode/DDYQRCode/DDYQRCode.bundle'
	    ddyspec.requires_arc = false
	    # s.framework  = "AVFoundation"
	    # s.frameworks = "UIKit", "AVFoundation"
	    # s.dependency "JSONKit", "~> 1.4"
	end
	```
* 校验格式是否正确 ``` pod spec lint DDYQRCode.podspec --verbose ```
* 注册Trunk ``` pod trunk register 634778311@qq.com 'Rain' --description='iOS' --verbose ```
* 邮箱点击链接验证
* Trunk信息 ``` pod trunk me ```
* 多人维护添加成员 ``` pod trunk add-owner ARAnalytics kyle@cocoapods.org ```
* 发布到pod Trunck ``` pod trunk push DDYQRCode.podspec ```
* 如果必要的话更新pod ``` pod setup ```
* 删除单个版本 ``` pod trunk delete PODNAME VERSION ```
* 删除所有版本 ``` pod trunk deprecate PODNAME ``` (有提示回复y)

> #### 六、卸载指定版本

* 查看cocoapods相关安装 ``` gem list --local | grep cocoapods ```
* 删除相关 ``` sudo gem uninstall cocoapods-deintegrate ```
* 移除指定 ``` sudo gem uninstall cocoapods -v 1.7.5 ```

> #### setup加速

1. ##### gitee镜像
	
	```
	pod repo remove master   
	pod repo add master https://gitee.com/mirrors/CocoaPods-Specs   
	pod repo update   
	```
2. ##### 清华镜像


	```
	pod repo remove master   
	pod repo add master https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git   
	pod repo update   
	```

3. ##### iOS工程如果有Podfile要更改

	```
	source 'https://gitee.com/mirrors/CocoaPods-Specs.git'
	source 'https://mirrors.tuna.tsinghua.edu.cn/git/CocoaPods/Specs.git'
	```

> #### 参考

* [cocoapods版本](https://rubygems.org/gems/cocoapods/versions)
* [CocoaPods清华镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/CocoaPods/)
* [Ruby Gems 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/rubygems/)
* [编写podspec文件](https://segmentfault.com/a/1190000012269307)


<!-- sudo gem update fourflusher -->