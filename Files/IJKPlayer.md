#### 进入桌面(也可用自己喜欢的目录)

1. 进入桌面 ``` cd ~/Desktop/ ```
2. 创建文件夹 ``` mkdir ijk ```
3. 进入ijk文件夹 ``` cd ijk/ ```

#### 安装Homebrew(可能用到，按需执行，如果有则可以不执行某些命令)

1. 查看版本 ``` brew --version ```
2. 卸载brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```  
3. 安装brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
4. 查看版本 ``` brew --version ``` 
5. 安装目录	 ``` which brew ```

#### 安装git(可能用到，按需执行，如果有则可以不执行某些命令)

1. 查看版本 ``` git --version ```
2. 安装git ``` brew install git ```

#### 安装yasm(可能用到，按需执行，如果有则可以不执行某些命令)

1. 查看版本 ``` yasm --version ``` 
2. 安装yasm  ``` brew install yasm ```

#### 克隆项目与检出分支

1. github克隆 ``` git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-ios ```
2. gitee克隆 ``` git clone https://gitee.com/mirrors/ijkplayer.git ijkplayer-ios ```
3. 进入ijkplayer-ios ``` cd ijkplayer-ios ```
4. 检出并命名分支 ``` git checkout -B latest k0.8.8 ```

#### 准备活动，防止不必要问题

config/module-lite-hevc.sh文件添加更多支持

```
# OpenSSL
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-openssl"
# RTSP
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-demuxer=rtsp"
# MJPEG
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-decoder=mjpeg"
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-demuxer=mjpeg"
# TCP
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-protocol=tcp"
# SDP
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-demuxer=sdp"
# RTP
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-demuxer=rtp"
# Nellymoser
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-decoder=nellymoser"
# mp3_header_decompress
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-bsf=mp3_header_decompress"
# mpeg4_unpack_bframes
export COMMON_FF_CFG_FLAGS="$COMMON_FF_CFG_FLAGS --enable-bsf=mpeg4_unpack_bframes"
```
 
ios/compile-ffmpeg.sh 和 ios/compile-openssl.h

```
// 去掉armv7(32位)支持 如
FF_ALL_ARCHS_IOS8_SDK="armv7 arm64 i386 x86_64" 
改为 
FF_ALL_ARCHS_IOS8_SDK="arm64 i386 x86_64"

"$FF_TARGET" = "armv7" -o "$FF_TARGET" = "armv7s" -o "$FF_TARGET"
改为
"$FF_TARGET" = "armv7s" -o "$FF_TARGET" = "arm64"

echo "  compile-ffmpeg.sh armv7|arm64|i386|x86_64"
改为
echo "  compile-ffmpeg.sh arm64|i386|x86_64"
```

修改 xcode-select

```
sudo /usr/bin/xcode-select -switch /Applications/Xcode.app/Contents/Developer
```

修改init-ios-openssl.sh

```
该更为镜像
#IJK_OPENSSL_UPSTREAM=https://github.com/openssl/openssl
IJK_OPENSSL_UPSTREAM=https://gitee.com/mirrors/openssl

去除armv7支持 pull_fork "armv7"
```

修改hosts文件

```
sudo vim /etc/hosts

浏览器访问 www.ipaddress.com 和 http://tool.chinaz.com/dns 
分别查找github.com 和 github.global.ssl.fastly.net 对应ip

写入hosts文件(wq退出并保存)

格式为:  199.232.5.194 github.global.ssl.fastly.net

刷新DNS缓存 sudo killall -HUP mDNSResponder
然后强制退出终端或者重启大法
```

#### 编译openssl和ffmpeg

1. 进入ios目录执行

```
./compile-openssl.sh clean
./compile-ffmpeg.sh clean
```

2. 进入ijkplayer-ios目录执行

```
// 下载openssl
./init-ios-openssl.sh

// 下载ffmpeg
./init-ios.sh
```

3. 进入ios目录执行

```
// 编译openssl
./compile-openssl.sh all

// 编译ffmpeg
./compile-ffmpeg.sh all
```

/build/universal/include/libavutil/avconfig.h:34 
去掉行 include "armv7/avconfig.h"

/build/universal/include/libffmpeg/config.h:34
去掉行 include "armv7/config.h"

./compile-ffmpeg.sh all x86_64 编译单个指令集