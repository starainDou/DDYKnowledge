* Xcode 工具按需安装 ``` xcode-select --install ```
* 卸载Homebrew ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```
* 安装Homebrew ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
* 帮助 ``` brew help ```
* 版本 ``` brew -v ``` 或 ``` brew --version ```
* 更新Homebrew自己 ``` brew update ```
* 安装软件包 ``` brew install 包名 ```
* 重置Homebrew ``` brew update-reset ```
* 查看哪些安装包需要更新 ``` brew outdated ```
* 更新安装包 ``` brew upgrade # 更新所有的包  brew upgrade $FORMULA # 更新指定的包 ```
* 清理旧版本 ``` brew cleanup   # 清理所有包的旧版本
brew cleanup $FORMULA   # 清理指定包的旧版本
brew cleanup -n   # 查看可清理的旧版本包，不执行实际操作
```
* 锁定不想更新的包 ``` brew pin $FORMULA # 锁定某个包 brew unpin $FORMULA  # 取消锁定 ```
* 查看安装包的相关信息 ``` brew info $FORMULA   # 显示某个包的信息
brew info   # 显示安装了包数量，文件数量，和总占用空间
brew deps --installed --tree   # 查看已安装的包的依赖，树形显示
```
* 列出已安装包 ``` brew list ```
* 搜索 ``` brew search ffmpeg ```
* 删除 ``` brew rm $FORMULA   # 删除某个包
brew uninstall --force $FORMULA   # 删除所有版本 ```

brew tap --repair 

brew doctor



```
Error: Another active Homebrew update process is already in progress.
Please wait for it to finish or terminate it to continue.
1
2
解决方法：rm -rf /usr/local/var/homebrew/locks
```

```
LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443

sudo xcode-select --reset

// git config --global http.proxy 127.0.0.1:9909
// git config --global https.proxy 127.0.0.1:9909
git config --global --unset http.proxy
git config --global --unset https.proxy
```

升级Python3.x

```
brew install python@3.9

brew link python@3.9 

echo "alias python=/usr/local/bin/python3.9" >> ~/.zshrc
  
source ~/.zshrc
```


* [Mac下Homebrew的基本命令](https://zhuanlan.zhihu.com/p/90765541)
* [2020-01-20 Homebrew换源和更新](https://www.jianshu.com/p/906e10bb6a21)
* [Homebrew教程](https://www.knowledgedict.com/tutorial/homebrew-command-update.html)
* [Homebrew总结](https://blog.csdn.net/tom_ke/article/details/79321210)