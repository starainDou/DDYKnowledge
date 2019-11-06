> #### OTA（Over_The_Air Installation）内部测试版分发

有很多方式

* www.pgyer.com（蒲公英，简单粗暴，需要认证）
* www.fir.im (风起云飞，简单粗暴，需要认证)
* TestFlight (苹果官方，麻烦还得下载TestFlight)
* 自建分发


> #### 用gitee建立分发功能

本来用github，但是gh pages刷新相对麻烦且可能存在无法访问，所以用了gitee。用哪个都一样，只要是https能访问下载plist的都可以(ipa可以不用https，甚至可以放本地体验超快下载安装)。


* 登录gitee 创建工程，我这里命名idengyun (你想要的合法不重复的就行)
* 上传57×57和512×512尺寸的Icon图，并获取实际原始地址(即下载地址)
* 上传plist文件，内容见附加
* 制定相应静态网页，用来触发下载
* 开启gitee pages：服务-->gitee pages

> #### 本地Apache

* 开启本地服务器 ``` sudo apachectl start ```
* 重启本地服务器 ``` sudo apachectl restart  ``` 
* 关闭本地服务器 ``` sudo apachectl stop ```
* 测试是否已开启 ``` 127.0.0.1 ``` 或者 ``` localhost ``` 或 局域网的ip
* 访达前往 Command+Shift+G ``` /Library/WebServer/Documents ```
* 复制粘贴命名ipa包 ``` local360.ipa ```


> #### 附加

* 每次更新ipa后要重启本地服务器
* ip地址可通过系统偏好设置中网络查看
* 如果更新了git工程文件要在gitee工程pages页更新部署


* index.html

```
<!doctype html>
<html>
    <head>
        <meta charset="UTF-8"><title>妃子校</title>
        <meta name='viewport' content='width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0'>
        <meta name='apple-mobile-web-app-capable' content='yes'>
        <meta name='apple-mobile-web-app-status-bar-style' content='black'>
        <meta name='format-detection' content='telephone=no'>

        <script>
	    	function jumpLocal() {
	    		window.location.href="itms-services://?action=download-manifest&url=https://gitee.com/starainDou/idengyun/raw/master/local/360.plist";
	    	}
	    	function jumpOnline() {
	    		window.location.href="itms-services://?action=download-manifest&url=https://gitee.com/starainDou/idengyun/raw/master/online/360.plist";
	    	}
    	</script>

    	<style>
    		.divCenter {
    			text-align: center;
    		}
    		.buttonRed { 
    			color: #fff; 
    			background-color: #FF5E84; 
    			border-color: #FF5E84; 
    			text-shadow: 0 -1px 0 rgba(0,0,0,0.12); 
    			-webkit-box-shadow: 0 2px 0 rgba(0,0,0,0.045); 
    			box-shadow: 0 2px 0 rgba(0,0,0,0.045);
    			border: 2px solid;
    			border-radius: 12px;
    			padding-top: 6px;
    			padding-bottom: 6px;
    			padding-left: 22px;
    			padding-right: 22px;
    		}
            h1 {
                text-shadow: 5px 5px 5px #FF5E84;
            }
            p.titleFont {
                font:italic bold 11px/24px Georgia,serif;
            }
            p.customFont {
                font:18px arial,sans-serif;
            }
    	</style>
    </head>

	<body>
        <p style="text-align: center;" class="titleFont">
            <h1 style="text-align: center;">妃子校</h1>
        </p>
        <p style="text-align: center;" class="customFont">
            <b>注意:</b> 请用浏览器打开链接
        </p>        
        <p style="align-content: center;" class="divCenter">
            <img src="https://gitee.com/starainDou/idengyun/raw/master/icon/qrcode.jpeg" alt="Norway" width="300" height="300">
        </p>
    	<p style="text-align: center;" class="divCenter">
	    	<button id="0001" onclick=javascrtpt:jumpLocal() class="buttonRed" style="font-size:24px">本地测试 点我下载</button> 
	    </p>
	    <p style="text-align:center;" class="divCenter">
	    	<button id="0002" onclick=javascrtpt:jumpOnline() class="buttonRed" style="font-size:24px">线上测试 点我下载</button> 
	    </p>
    </body>
</html>
```

* 360.plist内容(名字任意和下载中一致合法即可)

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>
		<dict>
			<key>assets</key>
			<array>
				<dict>
					<key>kind</key>
					<string>software-package</string>
					<key>url</key>
					<string>http://172.50.3.209/360.ipa</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>display-image</string>
					<key>url</key>
					<string>https://gitee.com/starainDou/idengyun/raw/master/icon/icon1.png</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>full-size-image</string>
					<key>url</key>
					<string>https://gitee.com/starainDou/idengyun/raw/master/icon/icon2.png</string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string>com.kenuo.training</string>
				<key>bundle-version</key>
				<string>3.5.0</string>
				<key>kind</key>
				<string>software</string>
				<key>platform-identifier</key>
				<string>com.apple.platform.iphoneos</string>
				<key>title</key>
				<string>妃子校</string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
```


