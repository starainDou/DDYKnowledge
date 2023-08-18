![官方示意图](https://upload-images.jianshu.io/upload_images/2145623-f600f40468740b55.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![细化流程图](https://upload-images.jianshu.io/upload_images/2145623-1309cd8d97298a7a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)


目前 SKAdNetwork2.2 支持浏览式广告曝光归因（View-Through Ads）和渲染型广告（StoreKit-Rendered Ads，App Store渲染式广告，可以理解为需要点击跳转App Store 网页页面或者客户端应用落地页的广告）

StoreKit-Rendered Ads ，主要的归因方式链路式归因，苹果IOS 系统取消IDFA之后，应用的归因由系统提供追踪ID（签名）来实现

第一步，进行广告应用的签名，广告平台（Ad Network）在收到媒体端的广告请求时为广告应用进行签名（可以理解为生成一个广告的追踪ID）。

第二步，在广告即将曝光前，调用系统的 ``` loadProduct(withParameters:completionBlock:)``` 函数，目的在于预加载一个App Store的web端或者客户端的广告展示落地页。这样可以提升广告展示的速度，同时，将广告的应用相关签名信息和广告网络ID（Ad network ID）等信息传给App Store。

第三步，当用户点击广告时，会调起App Store 客户端广告应用的页面或者web 端页面，同时进行签名和广告网络ID信息的透传；


第四步，广告主发送激活通知，当广告主App安装后进行激活时，需要调用系统的函数，发起应用安装验证通知，调用方法 ```registerAppForAdNetworkAttribution() ```或 ``` updateConversionValue(_:) ```。目的是将广告的安装、激活以及转化信息回传给到广告平台。此时默认会启动一个24小时的计时器（Start timer）。超出时间再调用安装验证通知就会无效。当然，广告主可以通过调用updateConversionValue(_:)更新转化的值以及重启一个计时器。

第五步，广告平台接收激活和转化信息；当用户安装并启动一个属于广告的应用程序App时，广告主会调用```registerAppForAdNetworkAttribution()```或```updateConversionValue (_:)```。这些调用会将激活和转化信息发送到广告平台注册的回包网址（Postback URL），广告平台收到激活和转化通知后对信息进行确认，以此来完成一次的广告生命周期。

从上面的链路流程我们可以看到，这个归因完全不依赖于系统的隐私用户ID。核心关键在于系统提供了Storekit 和对应的API使得广告签名信息可以从广告的曝光一直传递到广告的激活和转化。


这个链路归因的方案涉及到各方的职责如下：


一、广告平台或者广告网络（Ad Network）的职责

在Apple 上注册Network ID，用于在广告活动时使用归因验证的各种API。

为流量媒体侧App（Source App）提供已签名的广告应用；

通过注册的回包网址（Postback URL）接收来自广告主回传的激活和转化信息；

确认已接收广告主回传的信息；

 

二、媒体APP（Source App）

需要在媒体应用的Info.plist 上添加广告网络的ID；

展示已签名的广告；

 

三、广告主应用（Advertised App）的职责：

在应用安装激活后，调用系统的API ```registerAppForAdNetworkAttribution()``` or ```updateConversionValue(_:)```发送激活归因信息；

调用```updateConversionValue(_:)```，更新应用后端转化的值，这是一个可选项，广告主根据实际情况决定是否使用；

苹果对于应用归因提供了StoreKit-Rendered Ads——基于App Store渲染的广告归因以及View-Through Ads（曝光广告归因）的能力

```
- (void)loadProductWithParameters:(NSDictionary<NSString *,id> *)parameters 
                  completionBlock:(void (^)(BOOL result, NSError *error))block;
                  
                  该方法用来达到从广告展示页面跳转到苹果商店的功能。在之前已经广泛被使用。在此次更新之前，主要传递parameters包含SKStoreProductParameterITunesItemIdentifier，广告应用的商店ID。
在此次更新后，需要传递的parameters内容增多了。具体如下：

SKStoreProductParameterAdNetworkVersion -- 版本号，最终会归因后透传给AdNetwork
SKStoreProductParameterAdNetworkIdentifier--AdNetwork在苹果处的识别ID
SKStoreProductParameterAdNetworkCampaignIdentifier-- 广告Campaign id，只能填1到100
)
SKStoreProductParameterITunesItemIdentifier-- 广告主应用的苹果商店ID
SKStoreProductParameterAdNetworkNonce -- 该impression在AdNetwork处的唯一ID，可用来校验真伪
SKStoreProductParameterAdNetworkSourceAppStoreIdentifier -- Publisher应用的苹果商店ID
SKStoreProductParameterAdNetworkTimestamp -- 展示广告的时间戳
SKStoreProductParameterAdNetworkAttributionSignature -- 签名，供给苹果和AdNetwork用来校验真伪
```


* [IOS广告归因](https://www.appganhuo.com/4235273222/)
* [申请过AdNetwork ID](https://developer.apple.com/documentation/storekit/skadnetwork/registering_an_ad_network?language=objc)
* [SkAdNetwork———归因路径从此串联了起来？](https://news.16p.com/892684.html)
* [SKAN - 更新转化值 conversion value](https://zhuanlan.zhihu.com/p/452147151)
* [iOS 14 AdNetwork归因流程](https://www.jianshu.com/p/fc42a3b7c695)