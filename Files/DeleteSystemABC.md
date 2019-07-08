# 起因

作为一个中国人，使用国内输入法必不可少，但是Mac 系统自带的ABC/U.S 输入法严重干扰了我们的使用。很想把它弄屎。。。

# 经过

* 禁止SIP(系统完整性保护-System Integrity Protection)

1. 重启系统，按住 Command + R 进入恢复模式；
2. 点击顶部菜单栏 实用工具 中的 终端；
3. 输入 ```csrutil disable``` 来禁用 SIP 保护机制；
4. 重启系统
5. 如果想重新开启按上述步骤，然后执行 ```csrutil enable```

* 修改

1. 安装国内好用的输入法，例如搜狗输入法、手心输入法
2. 禁止SIP后正常进入系统，终端输入 ```sudo open ~/Library/Preferences/com.apple.HIToolbox.plist```(最好备份下)
3. 找到AppleEnabledInputSources 条目下 ABC 或 U.S item项，并删除整个item
4. 重启电脑(有时候重启不生效，那就关机重新开机) 
5. 如果生效，以后就可以只保留已安装的三方输入法了，按默认shift键就快速切换英汉输入
6. 恢复ABC输入法 偏好设置->键盘->输入法，点 + 号，重新添加就可以了

