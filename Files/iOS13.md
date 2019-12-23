本文对应github地址[iOS 13 问题解决以及苹果登录](https://github.com/starainDou/DDYKnowledge/blob/master/Files/iOS13.md),如果由于github调整导致资源找不到或细节更改，请访问[github](https://github.com/starainDou/DDYKnowledge)     

[本文掘金地址](https://juejin.im/post/5d85c3fde51d453b7779d604) 

本文直接搬砖，随便看看就行       

iOS 13 (Xcode11编译时)问题解决以及苹果登录    

* #### KVC修改私有属性可能Crash(不是所有，不是所有，不是所有)，需要用别的姿势替代。
    
    
    
    * ##### UITextField的私有属性_placeholderLabel的字体颜色，
    
    如 ``` [textField setValue:color forKeyPath:@"_placeholderLabel.textColor"]; ``` 会crash。
    
    那么该怎么办呢？下面提供几种姿势
    
    ###### 姿势一：采用富文本形式 
    
    ``` 
    _textField.attributedPlaceholder = [[NSMutableAttributedString alloc] initWithString:placeholder attributes:@{NSForegroundColorAttributeName : color}];
    ```
    
    ###### 姿势二：new方式创建一个新label（太low不建议用）
    
    ```
    // 必须new创建，如果alloc-init创建还是crash(两种方式差别自行google，不是BD)
    UILabel * placeholderLabel = [UILabel new];
    placeholderLabel.text = @"666";
    placeholderLabel.textColor = [UIColor blueColor];
    [_textField setValue: placeholderLabel forKey:@"_placeholderLabel"];//new创建这里并没有crash
    ```
    
    ###### 姿势三：Runtime
    
    ```
    Ivar ivar = class_getInstanceVariable([UITextField class], "_placeholderLabel");
    UILabel *placeholderLabel = object_getIvar(_textField, ivar);
    placeholderLabel.textColor = color;
    ```
    
    * ##### [searchBar valueForKey:@"_searchField"]; 取值崩溃

	```
	- (UITextField *)ddy_SearchField {
		#ifdef __IPHONE_13_0
		if (@available(iOS 13.0, *)) {
	   		return self.searchTextField;
		}
		#endif
		return [self valueForKey:@"_searchField"];
	}
	```
    
    所以修改UISearchBar占位字符可以把上面的结合使用
    

* #### 模态弹出时 modalPresentationStyle 改变了默认值
 
     * 在iOS13之前的版本中, UIViewController的UIModalPresentationStyle属性默认是UIModalPresentationFullScreen，而在iOS13中变成了UIModalPresentationPageSheet。     
    * 我们需要在presentViewController时，设置一下UIModalPresentationStyle，就可以达到旧的效果。
    * 如果PageSheet想控制下拉dismiss，modalInPresentation可以控制

    [该分类所在github工程](https://github.com/starainDou/DDYCategory)

    UIViewController+DDYPresent.h[下载该文件](https://raw.githubusercontent.com/starainDou/DDYCategory/master/DDYCategory/DDYCategory/DDYCategory/UIKit/UIViewController%2BDDYPresent.h)
    
	```
	/// 一个一个改浪费时间，适合版本迭代中逐步替换；
	/// 直接重写-modalPresentationStyle 侵入性太大，造成系统弹出也被重置，或者某个控制器想改变样式都不能，不太友好
	/// 所以用一个类方法控制全局，一个实例方法控制具体某个控制器实例样式。
	    
	#import <UIKit/UIKit.h>
	
	NS_ASSUME_NONNULL_BEGIN
	
	@interface UIViewController (DDYPresent)
	
	/// 自动调整模态弹出样式时要排除的控制器(如果未设置则使用内置)
	/// @param controllerNameArray 模态弹出的控制器名称数组
	+ (void)ddy_ExcludeControllerNameArray:(NSArray<NSString *> *)controllerNameArray;
	
	/// 是否自动调整模态弹出全屏样式
	/// NO:表示不自动调整，保持默认，可能全屏样式也可能其他样式
	/// YES:表示调整为全屏样式
	/// 如果是排除的控制器数组包含的控制器则默认NO
	/// 如果不在排除的控制器数组内包含则默认YES
	@property (nonatomic, assign) BOOL ddy_AutoSetModalPresentationStyleFullScreen;
	
	@end
	
	NS_ASSUME_NONNULL_END
	    
	```
    
    UIViewController+DDYPresent.m[下载该文件](https://raw.githubusercontent.com/starainDou/DDYCategory/master/DDYCategory/DDYCategory/DDYCategory/UIKit/UIViewController%2BDDYPresent.m)
    
    
	```
	#import "UIViewController+DDYPresent.h"
	#import <objc/runtime.h>
	
	@implementation UIViewController (DDYPresent)
	
	static NSArray *excludeControllerNameArray;
	
	+ (void)changeOriginalSEL:(SEL)orignalSEL swizzledSEL:(SEL)swizzledSEL {
	    Method originalMethod = class_getInstanceMethod([self class], orignalSEL);
	    Method swizzledMethod = class_getInstanceMethod([self class], swizzledSEL);
	    if (class_addMethod([self class], orignalSEL, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod))) {
	        class_replaceMethod([self class], swizzledSEL, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
	    } else {
	        method_exchangeImplementations(originalMethod, swizzledMethod);
	    }
	}
	
	+ (void)load {
	    static dispatch_once_t onceToken;
	    dispatch_once(&onceToken, ^{
	        SEL originalSEL = @selector(presentViewController:animated:completion:);
	        SEL swizzledSEL = @selector(ddy_PresentViewController:animated:completion:);
	        [self changeOriginalSEL:originalSEL swizzledSEL:swizzledSEL];
	    });
	}
	
	- (void)ddy_PresentViewController:(UIViewController *)viewControllerToPresent animated:(BOOL)flag completion:(void (^)(void))completion {
	    if (@available(iOS 13.0, *)) {
	        if (viewControllerToPresent.ddy_AutoSetModalPresentationStyleFullScreen) {
	            viewControllerToPresent.modalPresentationStyle = UIModalPresentationFullScreen;
	        }
	    }
	    [self ddy_PresentViewController:viewControllerToPresent animated:flag completion:completion];
	}
	
	- (void)setDdy_AutoSetModalPresentationStyleFullScreen:(BOOL)ddy_AutoSetModalPresentationStyleFullScreen {
	    objc_setAssociatedObject(self, @selector(ddy_AutoSetModalPresentationStyleFullScreen), @(ddy_AutoSetModalPresentationStyleFullScreen), OBJC_ASSOCIATION_ASSIGN);
	}
	
	- (BOOL)ddy_AutoSetModalPresentationStyleFullScreen {
	    NSNumber *obj = objc_getAssociatedObject(self, @selector(ddy_AutoSetModalPresentationStyleFullScreen));
	    return obj ? [obj boolValue] : ![UIViewController ddy_IsExcludeSetModalPresentationStyleFullScreen:NSStringFromClass(self.class)];
	}
	
	// MARK: - 类方法
	// MARK: 全局设置排除的控制器
	+ (void)ddy_ExcludeControllerNameArray:(NSArray<NSString *> *)controllerNameArray {
	    excludeControllerNameArray = controllerNameArray;
	}
	
	// MARK: 如果没有外部设置则使用内置的排除数组
	+ (NSArray<NSString *> *)ddy_InnerExcludeControllerNameArray {
	    NSMutableArray *nameArray = [NSMutableArray array];
	    [nameArray addObject:@"UIImagePickerController"];
	    [nameArray addObject:@"UIAlertController"];
	    [nameArray addObject:@"UIActivityViewController"];
	    [nameArray addObject:@"UIDocumentInteractionController"];
	    [nameArray addObject:@"SLComposeViewController"]; //  #import <Social/Social.h>
	    [nameArray addObject:@"SLComposeServiceViewController"]; // #import <Social/Social.h>
	    [nameArray addObject:@"UIMenuController"];
	    [nameArray addObject:@"SFSafariViewController"]; // API_AVAILABLE(ios(9.0)) #import <SafariServices/SafariServices.h>
	    [nameArray addObject:@"SKStoreReviewController"]; // API_AVAILABLE(ios(10.3), macos(10.14)) #import <StoreKit/StoreKit.h>
	    [nameArray addObject:@"SKStoreProductViewController"]; // API_AVAILABLE(ios(6.0)) #import <StoreKit/StoreKit.h>
	    return nameArray;
	}
	
	// MARK: 是否是要排除自动设置的控制器
	+ (BOOL)ddy_IsExcludeSetModalPresentationStyleFullScreen:(NSString *)className {
	    NSArray *nameArray = excludeControllerNameArray ?: [UIViewController ddy_InnerExcludeControllerNameArray];
	    return [nameArray containsObject:className];
	}
	
	@end    
	```


* #### 获取DeviceToken姿势改变
 
    ##### iOS13之前
 
    ```
    NSString *myToken = [deviceToken description];
    myToken = [myToken stringByReplacingOccurrencesOfString: @"<" withString: @""];
    myToken = [myToken stringByReplacingOccurrencesOfString: @">" withString: @""];
    myToken = [myToken stringByReplacingOccurrencesOfString: @" " withString: @""];
    ```
    ##### iOS13之后(不建议这样写)

    [为什么不建议这样写](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622958-application?language=objc)APNs device tokens are of variable length. Do not hard-code their size

    ```
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
        if (![deviceToken isKindOfClass:[NSData class]]) return;
        const unsigned *tokenBytes = [deviceToken bytes];
        NSString *hexToken = [NSString stringWithFormat:@"%08x%08x%08x%08x%08x%08x%08x%08x",
                              ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                              ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                              ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
        NSLog(@"deviceToken:%@",hexToken);
    }
    ```
    
 
    ##### 推荐的写法
    
    
	```
	- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
	    if (!deviceToken || ![deviceToken isKindOfClass:[NSData class]] || deviceToken.length==0) {
	        return;
	    }
	    NSString *(^getDeviceToken)(void) = ^() {
	        if (@available(iOS 13.0, *)) {
	            const unsigned char *dataBuffer = (const unsigned char *)deviceToken.bytes;
	            NSMutableString *myToken  = [NSMutableString stringWithCapacity:(deviceToken.length * 2)];
	            for (int i = 0; i < deviceToken.length; i++) {
	                [myToken appendFormat:@"%02x", dataBuffer[i]];
	            }
	            return (NSString *)[myToken copy];
	        } else {
	            NSCharacterSet *characterSet = [NSCharacterSet characterSetWithCharactersInString:@"<>"];
	            NSString *myToken = [[deviceToken description] stringByTrimmingCharactersInSet:characterSet];
	            return [myToken stringByReplacingOccurrencesOfString:@" " withString:@""];
	        }
	    };
	    NSString *myToken = getDeviceToken();
	    NSLog(@"%@", myToken);
	}
	```
 
     
    
* #### UIWebView

    * 苹果已经从iOS13禁止UIWebView方式了，需要更换WKWebView(过渡期仍可用，只是邮件警告，目前不影响审核)
    * UIWebView 无缝转WKWebView 正在开发中，请添加关注随时发布!!!
    
* #### 即将废弃的 LaunchImage    

    * 从iOS 8 苹果引入了 LaunchScreen，我们可以设置LaunchScreen来作为启动页。当然，现在你还可以使用LaunchImage来设置启动图。不过使用LaunchImage的话，要求我们必须提供各种屏幕尺寸的启动图，来适配各种设备，随着苹果设备尺寸越来越多，这种方式显然不够 Flexible。而使用 LaunchScreen的话，情况会变的很简单， LaunchScreen是支持AutoLayout+SizeClass的，所以适配各种屏幕都不在话下。
    * 从2020年4月开始，所有使⽤ iOS13 SDK的 App将必须提供 LaunchScreen，LaunchImage即将退出历史舞台。


* #### MPMoviePlayerController 被禁止

    * 这个用的人应该不多了，如果是则更换姿势，如用AVPlayer

* #### UITableViewCell中cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;方框问题

    * 低版本Xcode(如Xcode10)编译运行在iOS13上则会出现方框，如果用Xcode11编译则不会出现
        
* #### 增加苹果登录(可选)    

1. 去[开发者网站](https://developer.apple.com) 在 Sign in with Apple 开启功能
2. Xcode 里面 Signing & Capabilities 开启 Sign in with Apple 功能
3. 利用 ASAuthorizationAppleIDButton
    
    ```
    ASAuthorizationAppleIDButton *button = [ASAuthorizationAppleIDButton buttonWithType:ASAuthorizationAppleIDButtonTypeSignIn style:ASAuthorizationAppleIDButtonStyleWhite];
    [button addTarget:self action:@selector(signInWithApple) forControlEvents:UIControlEventTouchUpInside];
    button.center = self.view.center;
    button.bounds = CGRectMake(0, 0, 40, 40); // 宽度过小就没有文字了，只剩图标
    [self.view addSubview:button];
    ```
    
4. ASAuthorizationControllerPresentationContextProviding

    * ASAuthorizationControllerPresentationContextProviding 就一个方法，主要是告诉 ASAuthorizationController 展示在哪个 window 上  

    ```
    #pragma mark - ASAuthorizationControllerPresentationContextProviding
    
    - (ASPresentationAnchor)presentationAnchorForAuthorizationController:(ASAuthorizationController *)controller API_AVAILABLE(ios(13.0))
    {
        return self.view.window;
    }
    ```  

5. Authorization 发起授权登录请求

    ```
    - (void)signInWithApple API_AVAILABLE(ios(13.0)) {
        ASAuthorizationAppleIDProvider *provider = [[ASAuthorizationAppleIDProvider alloc] init];
        ASAuthorizationAppleIDRequest *request = [provider createRequest];
        request.requestedScopes = @[ASAuthorizationScopeFullName, ASAuthorizationScopeEmail];
        
        ASAuthorizationController *vc = [[ASAuthorizationController alloc] initWithAuthorizationRequests:@[request]];
        vc.delegate = self;
        vc.presentationContextProvider = self;
        [vc performRequests];
    }
    ```
    
    * ASAuthorizationAppleIDProvider 这个类比较简单，头文件中可以看出，主要用于创建一个 ASAuthorizationAppleIDRequest 以及获取对应 userID 的用户授权状态。在上面的方法中我们主要是用于创建一个 ASAuthorizationAppleIDRequest ，用户授权状态的获取后面会提到。
    * 给创建的 request 设置 requestedScopes ，这是个 ASAuthorizationScope 数组，目前只有两个值，ASAuthorizationScopeFullName 和 ASAuthorizationScopeEmail ，根据需求去设置即可。
    * 然后，创建 ASAuthorizationController ，它是管理授权请求的控制器，给其设置 delegate 和 presentationContextProvider ，最后启动授权 performRequests

6. 授权回调处理

    ```
    #pragma mark - ASAuthorizationControllerDelegate
    
    - (void)authorizationController:(ASAuthorizationController *)controller didCompleteWithAuthorization:(ASAuthorization *)authorization API_AVAILABLE(ios(13.0))
    {
        if ([authorization.credential isKindOfClass:[ASAuthorizationAppleIDCredential class]])       {
            ASAuthorizationAppleIDCredential *credential = authorization.credential;
            
            NSString *state = credential.state;
            NSString *userID = credential.user;
            NSPersonNameComponents *fullName = credential.fullName;
            NSString *email = credential.email;
            NSString *authorizationCode = [[NSString alloc] initWithData:credential.authorizationCode encoding:NSUTF8StringEncoding]; // refresh token
            NSString *identityToken = [[NSString alloc] initWithData:credential.identityToken encoding:NSUTF8StringEncoding]; // access token
            ASUserDetectionStatus realUserStatus = credential.realUserStatus;
            
            NSLog(@"state: %@", state);
            NSLog(@"userID: %@", userID);
            NSLog(@"fullName: %@", fullName);
            NSLog(@"email: %@", email);
            NSLog(@"authorizationCode: %@", authorizationCode);
            NSLog(@"identityToken: %@", identityToken);
            NSLog(@"realUserStatus: %@", @(realUserStatus));
        }
    }
    
    - (void)authorizationController:(ASAuthorizationController *)controller didCompleteWithError:(NSError *)error API_AVAILABLE(ios(13.0))
    {
        NSString *errorMsg = nil;
        switch (error.code) {
            case ASAuthorizationErrorCanceled:
                errorMsg = @"用户取消了授权请求";
                break;
            case ASAuthorizationErrorFailed:
                errorMsg = @"授权请求失败";
                break;
            case ASAuthorizationErrorInvalidResponse:
                errorMsg = @"授权请求响应无效";
                break;
            case ASAuthorizationErrorNotHandled:
                errorMsg = @"未能处理授权请求";
                break;
            case ASAuthorizationErrorUnknown:
                errorMsg = @"授权请求失败未知原因";
                break;
        }
        NSLog(@"%@", errorMsg);
    }
    ```
    
    * User ID: Unique, stable, team-scoped user ID，苹果用户唯一标识符，该值在同一个开发者账号下的所有 App 下是一样的，开发者可以用该唯一标识符与自己后台系统的账号体系绑定起来。

    * Verification data: Identity token, code，验证数据，用于传给开发者后台服务器，然后开发者服务器再向苹果的身份验证服务端验证本次授权登录请求数据的有效性和真实性，详见 Sign In with Apple REST API。如果验证成功，可以根据 userIdentifier 判断账号是否已存在，若存在，则返回自己账号系统的登录态，若不存在，则创建一个新的账号，并返回对应的登录态给 App。

    * Account information: Name, verified email，苹果用户信息，包括全名、邮箱等。

    * Real user indicator: High confidence indicator that likely real user，用于判断当前登录的苹果账号是否是一个真实用户，取值有：unsupported、unknown、likelyReal。

    * 失败情况会走 authorizationController:didCompleteWithError: 这个方法，具体看代码吧

7. 其他情况的处理

    * 用户终止 App 中使用 Sign in with Apple 功能
    * 用户在设置里注销了 AppleId
    
    这些情况下，App 需要获取到这些状态，然后做退出登录操作，或者重新登录。     
    我们需要在 App 启动的时候，通过 getCredentialState:completion: 来获取当前用户的授权状态

    ```
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        
        if (@available(iOS 13.0, *)) {
            NSString *userIdentifier = 钥匙串中取出的 userIdentifier;
            if (userIdentifier) {
                ASAuthorizationAppleIDProvider *appleIDProvider = [ASAuthorizationAppleIDProvider new];
                [appleIDProvider getCredentialStateForUserID:userIdentifier
                                                  completion:^(ASAuthorizationAppleIDProviderCredentialState credentialState,
                                                               NSError * _Nullable error)
                {
                    switch (credentialState) {
                        case ASAuthorizationAppleIDProviderCredentialAuthorized:
                            // The Apple ID credential is valid
                            break;
                        case ASAuthorizationAppleIDProviderCredentialRevoked:
                            // Apple ID Credential revoked, handle unlink
                            break;
                        case ASAuthorizationAppleIDProviderCredentialNotFound:
                            // Credential not found, show login UI
                            break;
                    }
                }];
            }
        }
        
        return YES;
    }
    ```
    
    ASAuthorizationAppleIDProviderCredentialState 解析如下：

    * ASAuthorizationAppleIDProviderCredentialAuthorized 授权状态有效；
    * ASAuthorizationAppleIDProviderCredentialRevoked 上次使用苹果账号登录的凭据已被移除，需解除绑定并重新引导用户使用苹果登录；
    * ASAuthorizationAppleIDProviderCredentialNotFound 未登录授权，直接弹出登录页面，引导用户登录

8. 还可以通过通知方法来监听 revoked 状态，可以添加 ASAuthorizationAppleIDProviderCredentialRevokedNotification 这个通知

    ```
    - (void)observeAppleSignInState {
        if (@available(iOS 13.0, *)) {
            [[NSNotificationCenter defaultCenter] addObserver:self
                                                     selector:@selector(handleSignInWithAppleStateChanged:)
                                                         name:ASAuthorizationAppleIDProviderCredentialRevokedNotification
                                                       object:nil];
        }
    }
    
    - (void)handleSignInWithAppleStateChanged:(NSNotification *)notification {
        // Sign the user out, optionally guide them to sign in again
        NSLog(@"%@", notification.userInfo);
    }
    ```
    
 9. One more thing

    苹果还把 iCloud KeyChain password 集成到了这套 API 里，我们在使用的时候，只需要在创建 request 的时候，多创建一个 ASAuthorizationPasswordRequest ，这样如果 KeyChain 里面也有登录信息的话，可以直接使用里面保存的用户名和密码进行登录。代码如下  
    
    ```
    - (void)perfomExistingAccountSetupFlows API_AVAILABLE(ios(13.0))
    {
        ASAuthorizationAppleIDProvider *appleIDProvider = [ASAuthorizationAppleIDProvider new];
        ASAuthorizationAppleIDRequest *authAppleIDRequest = [appleIDProvider createRequest];
        ASAuthorizationPasswordRequest *passwordRequest = [[ASAuthorizationPasswordProvider new] createRequest];
        
        NSMutableArray <ASAuthorizationRequest *>* array = [NSMutableArray arrayWithCapacity:2];
        if (authAppleIDRequest) {
            [array addObject:authAppleIDRequest];
        }
        if (passwordRequest) {
            [array addObject:passwordRequest];
        }
        NSArray <ASAuthorizationRequest *>* requests = [array copy];
        
        ASAuthorizationController *authorizationController = [[ASAuthorizationController alloc] initWithAuthorizationRequests:requests];
        authorizationController.delegate = self;
        authorizationController.presentationContextProvider = self;
        [authorizationController performRequests];
    }
    
    #pragma mark - ASAuthorizationControllerDelegate
    
    - (void)authorizationController:(ASAuthorizationController *)controller didCompleteWithAuthorization:(ASAuthorization *)authorization API_AVAILABLE(ios(13.0))
    {
        if ([authorization.credential isKindOfClass:[ASPasswordCredential class]]) {
            ASPasswordCredential *passwordCredential = authorization.credential;
            NSString *userIdentifier = passwordCredential.user;
            NSString *password = passwordCredential.password;
            
            NSLog(@"userIdentifier: %@", userIdentifier);
            NSLog(@"password: %@", password);
        }
    }
    ``` 
 10. [官方Demo](https://docs-assets.developer.apple.com/published/80ac09c84c/AddingTheSignInWithAppleFlowToYourApp.zip)       

* #### Flutter1.9.1+hotfix2 Dart2.5 在iOS13真机上启动不了 

    错误信息 Device doesn't support wireless sync. AMDeviceStartService(device, CFSTR("com.apple.debugserver"), &gdbfd, NULL)
    
    解决方案 
    
    * 姿势一:
    
    更新[Flutter](https://flutter.cn/docs/get-started/install/macos)
    
    ```
    flutter upgrade
    ```
    
    * 姿势二:
    
    暂时切换到dev或master
    
    ```
    flutter channel dev
    // 或下面
    // flutter channel master
    // 然后执行
    flutter doctor
    // dart2.6 flutter1.10
    ```
    
    
* #### 获取不到wifiSSID(wifi名)   

    ```
    Dear Developer,
    
    As we announced at WWDC19, we're making changes to further protect user privacy and prevent unauthorized location tracking. Starting with iOS 13, the CNCopyCurrentNetworkInfo API will no longer return valid Wi-Fi SSID and BSSID information. Instead, the information returned by default will be: 
    
    SSID: “Wi-Fi” or “WLAN” (“WLAN" will be returned for the China SKU)
    BSSID: "00:00:00:00:00:00" 
    
    If your app is using this API, we encourage you to adopt alternative approaches that don’t require Wi-Fi or network information. Valid SSID and BSSID information from CNCopyCurrentNetworkInfo will still be provided to VPN apps, apps that have used NEHotspotConfiguration to configure the current Wi-Fi network, and apps that have obtained permission to access user location through Location Services. 
    
    Test your app on the latest iOS 13 beta to make sure it works properly. If your app requires valid Wi-Fi SSID and BSSID information to function, you can do the following:
    For accessory setup apps, use the NEHotSpotConfiguration API, which now has the option to pass a prefix of the SSID hotspot your app expects to connect to.
    For other types of apps, use the CoreLocation API to request the user’s consent to access location information.
    
    Learn more by reading the updated documentation or viewing the the Advances in Networking session video from WWDC19. You can also submit a TSI for code-level support. 
    
    Best regards,
    Apple Developer Relations
    ```

    苹果为了所谓隐私安全不让直接获取到wifiSSID了，然后还告知,如果是使用 NEHotspotConfiguration 的app可以获取,另外其他类型app需要用CoreLocation请求位置权限，用户同意后才可以获取。
    
    * ##### 使用NEHotspotConfiguration的app 
    
    ```
    // 连接WiFi
    NEHotspotConfiguration *config = [[NEHotspotConfiguration alloc] initWithSSID:@"wifi名" passphrase:@"密码" isWEP:NO];
    NEHotspotConfigurationManager *manager = [NEHotspotConfigurationManager sharedManager];
    [manager applyConfiguration: config completionHandler:^(NSError * _Nullable error) {
        NSLog(@"error :%@",error);
    }];
    
    // 获取wifiSSID 
    - (NSString *)wifiSSID {
        NSString *wifiSSID = nil;
        
        CFArrayRef wifiInterfaces = CNCopySupportedInterfaces();
        
        if (!wifiInterfaces) {
            return nil;
        }
        
        NSArray *interfaces = (__bridge NSArray *)wifiInterfaces;
        
        for (NSString *interfaceName in interfaces) {
            CFDictionaryRef dictRef = CNCopyCurrentNetworkInfo((__bridge CFStringRef)(interfaceName));
            if (dictRef) {
                NSDictionary *networkInfo = (__bridge NSDictionary *)dictRef;
                wifiSSID = [networkInfo objectForKey:(__bridge NSString *)kCNNetworkInfoKeySSID];
                CFRelease(dictRef);
            }
        }
        CFRelease(wifiInterfaces);
        return wifiName;
    }
    ```
    
    * ##### 请求位置权限征求用户同意后获取wifiSSID
    
    推荐使用封装好的请求权限方式[https://github.com/RainOpen/DDYAuthManager](https://github.com/RainOpen/DDYAuthManager)
    
    ```
    #import <SystemConfiguration/CaptiveNetwork.h>
    
    - (void)ddy_wifiSSID:(void (^)(NSString *wifiSSID))wifiSSID {
        
        void (^callBack)(NSString *) = ^(NSString *wifiName) {
            if (wifiSSID) {
                wifiSSID(nil);
            }
        };
        
        void (^getWifiName)(void) = ^(){
            CFArrayRef wifiInterfaces = CNCopySupportedInterfaces();
            if (!wifiInterfaces) {
                callBack(nil);
                return;
            }
        
            NSString *wifiName = nil;
            for (NSString *interfaceName in (__bridge_transfer NSArray *)wifiInterfaces) {
                CFDictionaryRef dictRef = CNCopyCurrentNetworkInfo((__bridge CFStringRef)(interfaceName));
                if (dictRef) {
                    NSDictionary *networkInfo = (__bridge NSDictionary *)dictRef;
                    wifiName = [networkInfo objectForKey:(__bridge NSString *)kCNNetworkInfoKeySSID];
                    CFRelease(dictRef);
                }
            }
            CFRelease(wifiInterfaces);
            callBack(wifiName);
        };
        
        if ([CLLocationManager locationServicesEnabled]) {
            [DDYAuthManager ddy_LocationAuthType:DDYCLLocationTypeAuthorized alertShow:YES success:^{
                getWifiName();
            } fail:^(CLAuthorizationStatus authStatus) {
                NSLog(@"定位服务被拒绝，弹窗告诉无法获取wifiSSID，请到设置开启定位权限");
                callBack(nil);
            }];
        } else {
            NSLog(@"定位服务不可用");
            callBack(nil);
        }
    }
    ```
    * ##### VPN旧版应用
    
* #### Xcode真机调试包 [Could not find Developer Disk Image](https://github.com/starainDou/DDYKnowledge/blob/master/Files/Tip001.md)

  1. 下载[开发包](https://www.lanzous.com/b923289)
  2. 强制退出Xcode（必须退出干净）
  3. 前往"/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport"粘贴解压缩文件(以自己实际路径实际名称)
  4. 如果找不到相应真机调试包可以尝试修改文件名

* #### iOS 13 UITabBar上分割线呢操作

    原来设置分割线的方式失效了
    
    ```
    [[UITabBar appearance] setBackgroundImage:[UIImage new]];
    [[UITabBar appearance] setShadowImage:[UIImage new]];
    ```

    最新更改TabBar上细线方式实例，利用苹果提供的新API，为所欲为(改图片，改颜色)
    
	```
	// OC
	if (@available(iOS 13, *)) {
		#ifdef __IPHONE_13_0
		UITabBarAppearance *appearance = [self.tabBar.standardAppearance copy];
		appearance.backgroundImage = [UIImage new];
		appearance.shadowImage = [UIImage imageNamed:@"Dotted_Line"];
		appearance.shadowColor = [UIColor clearColor];
		self.tabBar.standardAppearance = appearance;
		#endif
	} else {
		self.tabBar.backgroundImage = [UIImage new];
		self.tabBar.shadowImage = [UIImage imageNamed:@"Dotted_Line"];
	}
	
	// Swift
	if #available(iOS 13, *) {
		let appearance = self.tabBar.standardAppearance.copy()
		appearance.backgroundImage = UIImage()
		appearance.shadowImage = UIImage()
		appearance.shadowColor = .clear
		self.tabBar.standardAppearance = appearance
	} else {
		self.tabBar.shadowImage = UIImage()
		self.tabBar.backgroundImage = UIImage()
	}
	```
* #### iOS 13 Push后Pop回来tabbar选中文字颜色变系统蓝色(OC代码，swift一个样)
	
	* ##### 姿势一
	
	```
	self.tabBar.tinColor = color;
	```
	
	* ##### 姿势二
	
	```
	if (@available(iOS 10.0, *)) {
		self.tabBar.unselectedItemTintColor = color;
	}
	```
	
* #### 暗黑模式 

    * 关于暗黑模式也是开发者可选择性适配的内容，这里不赘述了，提供个文章参考
    * 不想适配暗黑则在Info.plist中添加Key:User Interface Style，值String的Light
    * [QiShare iOS13 DarkMode适配](https://juejin.im/post/5d889661e51d453b1e478b94)
    * [iOS13 暗黑模式(Dark Mode)适配之OC版](https://www.jianshu.com/p/0da3b107f06c)

* ##### library not found for -l stdc++.6.0.9

	* Xcode10开始去除了C++6.0.9

	* 如果非用不可，[下载文件](https://www.lanzous.com/i6e9c6h)
	
	```
	// 文件夹 1、2、3、4 中的文件分别对应复制到Xcode10中的以下4个目录中即可(Xcode11目录可能有变更)
	// 假设默认安装目录且Xcode.app命名
	
	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/

	/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/lib/

	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/lib/

	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/usr/lib/
	```
	
	更新Xcode11目录变更
	
	```
	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/
	↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓ 变更为 ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓
	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/
	```
	
	* 换个姿势试试
	
	```
	1. TARGETS–Build Phases–Link Binary With Libraries，删除6.0.9依赖，
	2. 需要的话对应添加libc++.tdb、libstdc++.tdb
	3. TARGETS–Build Settings–Other Linker Flags，删除 -l”stdc++.6.0.9”
	4. 如果是第三库引用了C++6.0.9库，那就只能联系服务方修改了
	```
* #### Multiple commands produce 'xxx/Info.plist'

	* Xcode10开始变更编译系统，如果项目所在空间存在多个Info.plist则报错

	```
	xcworkspace项目： Xcode左上角菜单栏 File –> Workspace Settings –> Build System – >Legacy Build System
	xcodeproj项目：Xcode左上角菜单栏 –> File –> Project Settings –> Build System –> Legacy Build System
	```
	
* #### 升级Xcode后xib报错 Failed to find or create execution context for description ...

	* 可以万能重启，还可以。。。

	```
	sudo killall -9 com.apple.CoreSimulator.CoreSimulatorService
	
	# 将你xcode中Developer文件夹位置放进来
	sudo xcode-select -s  /Applications/Xcode.app/Contents/Developer
	
	xcrun simctl erase all
	```
	
* #### 友盟导致崩溃 +[_LSDefaults sharedInstance]

	* 更新版本
	* 如果暂时没法更新(理由，我擦，屎山),临时方案[宇宙中转站→Saylor_lone博客:iOS 13 适配 ING...](https://juejin.im/post/5d89dc766fb9a06b0e54d251)

	```
	// 本工地大工没实际验证。。。
	@implementation NSObject (DDYExtension)
	
	+ (void)changeOriginalSEL:(SEL)orignalSEL swizzledSEL:(SEL)swizzledSEL {
	    Method originalMethod = class_getInstanceMethod([self class], orignalSEL);
	    Method swizzledMethod = class_getInstanceMethod([self class], swizzledSEL);
	    if (class_addMethod([self class], orignalSEL, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod))) {
	        class_replaceMethod([self class], swizzledSEL, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
	    } else {
	        method_exchangeImplementations(originalMethod, swizzledMethod);
	    }
	}
	
	+ (void)load {
	    static dispatch_once_t onceToken;
	    dispatch_once(&onceToken, ^{
	        SEL originalSEL = @selector(doesNotRecognizeSelector:);
	        SEL swizzledSEL = @selector(ddy_doesNotRecognizeSelector:);
	        [self changeOriginalSEL:originalSEL swizzledSEL:swizzledSEL];
	    });
	}
	
	+ (void)ddy_doesNotRecognizeSelector:(SEL)aSelector{
		// 处理 _LSDefaults 崩溃问题
		if([[self description] isEqualToString:@"_LSDefaults"] && (aSelector == @selector(sharedInstance))){
	    	//冷处理...
	    	return;
		}
		[self ddy_doesNotRecognizeSelector:aSelector];
	}
	```
	
* #### UITextField的leftView和rightView设置UIImageView或UIButton等被系统强(变)奸(窄)了。。。

	* ##### 姿势一：临时解决方案
		* 交换leftView/rightView的getter、setter，
		* 然后包装一个containerView父视图，并将containerView给了相应左右视图
		* 取视图则先取出containerView，从containerView中取出真正想要的视图
		* 注意处理在containerView上的位置。。。
	
	* ##### 姿势二：通用型方案
		
		* 想全局更改，交换 -leftViewRectForBounds: (最好留出控制属性以在外部随意更改玩转)
		* 如果采用子类，重写 -leftViewRectForBounds:
		
		```
		- (CGRect)leftViewRectForBounds:(CGRect)bounds {
		    CGRect iconRect = [super leftViewRectForBounds:bounds];
		    iconRect.origin.x = 3; // 可用属性控制 
		    iconRect.size.width = 6; // 可用属性控制 
		    return iconRect;
		}	
		```
	
			
* UIScrollView滚动条指示器偏移

	```
	// 屏幕旋转可能会触发系统对滚动条的自动修正，如果没有修改需求，关闭该特性即可
	#ifdef __IPHONE_13_0
   if (@available(iOS 13.0, *)) {
       self.automaticallyAdjustsScrollIndicatorInsets = NO;
   }
	#endif
	```
*  WKWebView 中测量页面内容高度的方式变更

    iOS 13以前 document.body.scrollHeight
    iOS 13开始 document.documentElement.scrollHeight

* CBCenterManager 蓝牙使用权限
    
    iOS13之前不用申请权限，iOS13开始需要申请权限
    
    ```
    <key>NSBluetoothAlwaysUsageDescription</key> 
    <string>App想使用蓝牙，是否同意</string>`
    ```
	
* #### Xcode10的过时问题，[虫洞传送门](https://www.jianshu.com/p/af0f8400ff09)

* #### Xcode11 创建新工程在AppDelegate.m中设置window不生效

    * Xcode11把入口交给了scene(为SwiftUI多场景打基础的)
    * 想要换回AppDelegate入口
    * 打开Info.plist点击减号删除Application Scene Mainfest
    * 再删除SceneDelegate两个文件
    * 删除AppDelegate.m中UISceneSession lifecycle两个方法
    * 最后AppDelegate.h中添加 @property (strong, nonatomic) UIWindow *window;

    
> ##### 参考

* [iOS 13 适配 ING...](https://juejin.im/post/5d89dc766fb9a06b0e54d251)
* [iOS13 [UIViewController presentViewController]样式适配](https://juejin.im/post/5d5f96866fb9a06b0517f78c)