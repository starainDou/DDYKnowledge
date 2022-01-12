# 简介

现在在MAC OS X 系统中有一个跟安全相关的模式叫 SIP(System Integrity Protection)，它禁止让软件以 root 身份来在 mac 上运行，并且对于目录 /System 、/sbin、/usr（不包含/usr/local/） 仅仅供系统使用，其它用户或者程序无法直接使用，例如 安装cocoaPods时如果直接安装 /usr 目录可能出问题。

# 禁止SIP方式

1. 重启系统，按住 Command + R 进入恢复模式；
2. 点击顶部菜单栏 实用工具 中的 终端；
3. 输入 ```csrutil disable``` 来禁用 SIP 保护机制；
4. 重启系统
5. 如果想重新开启按上述步骤，然后执行 ```csrutil enable```

# App已损坏，无法打开

```
sudo xattr -rd com.apple.quarantine /Applications/MyApp.app
```