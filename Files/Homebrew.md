* Xcode 工具按需安装 ``` xcode-select --install ```
* 卸载Homebrew ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```
* 安装Homebrew ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
* 帮助 ``` brew help ```
* 版本 ``` brew -v ``` 或 ``` brew --version ```
* 更新Homebrew ``` brew update ```
* 安装软件包 ``` brew install 包名 ```
* 重置Homebrew ``` brew update-reset ```

```
Error: Another active Homebrew update process is already in progress.
Please wait for it to finish or terminate it to continue.
1
2
解决方法：rm -rf /usr/local/var/homebrew/locks
```

* [Mac下Homebrew的基本命令](https://zhuanlan.zhihu.com/p/90765541)
* [2020-01-20 Homebrew换源和更新](https://www.jianshu.com/p/906e10bb6a21)
* [Homebrew教程](https://www.knowledgedict.com/tutorial/homebrew-command-update.html)
* [Homebrew总结](https://blog.csdn.net/tom_ke/article/details/79321210)