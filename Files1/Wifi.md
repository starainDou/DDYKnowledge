如果有这样的奇葩要求：wifi连接只能硬件，然后2/3/4/5G上网。看似简单。。。
问题来了：默认情况wifi连接后2/3/4/5G流量就被阻止了。。。


不过可以进行下面设置，完成上面需求

1，先连接上WiFi，打开详情可看到DHCP；
2，把IP地址和子网掩码，就是前两行抄下来；
3，点击静态，就是DHCP右边第二个按钮，把刚才抄下来的IP地址和子网掩码写进去；
4，确保静态下的路由器以及DNS什么的都不要填写；

至此返回就可以体验iPhone手机同时连接WiFi和3G/4G了，也达到了APP想要的效果；

https://discussions.apple.com/thread/3703817?tstart=0
https://support.apple.com/en-us/HT201373