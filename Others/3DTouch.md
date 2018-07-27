使用3Dtouch请注意机型和系统的判断

> ### 快捷启动

* #### 姿势一: 配置plist文件

1.在inof.plist文件中添加数组类型的 UIApplicationShortcutitems 

2.在这个数组(UIApplicationShortcutitems)里添加字典类型，表示每个选项 

3.在字典中配置每个选项的属性值，如 

UIApplicationShortcutItemType：该QuickAction的标示符，全局唯一；

UIApplicationShortcutItemTitle: 大字号主title

UIApplicationShortcutItemSubtitle：中字号子title；

UIApplicationShortcutItemIconType：该QuickAction的图标。

<br><br>

系统提供的UIApplicationShortcutItemIconType有如下类型：

UIApplicationShortcutIconTypeCompose,

UIApplicationShortcutIconTypePlay,

UIApplicationShortcutIconTypePause,

UIApplicationShortcutIconTypeAdd,

UIApplicationShortcutIconTypeLocation,

UIApplicationShortcutIconTypeSearch,

UIApplicationShortcutIconTypeShare,

UIApplicationShortcutIconTypeProhibit       NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeContact        NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeHome           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeMarkLocation   NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeFavorite       NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeLove           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeCloud          NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeInvitation     NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeConfirmation   NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeMail           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeMessage        NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeDate           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeTime           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeCapturePhoto   NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeCaptureVideo   NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeTask           NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeTaskCompleted  NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeAlarm          NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeBookmark       NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeShuffle        NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeAudio          NS_ENUM_AVAILABLE_IOS(9_1),

UIApplicationShortcutIconTypeUpdate         NS_ENUM_AVAILABLE_IOS(9_1),

4.在AppDelegate 中实现方法

```
-(void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL))completionHandler {

if ([shortcutItem.type isEqualToString:@"ShortCutOpen"]) {
NSLog(@"ShortCutOpen");
}

if ([shortcutItem.type isEqualToString:@"ShortCutShare"]) {
NSLog(@"ShortCutShare");
SecondViewController *vc = [[SecondViewController alloc] init];
[self.window.rootViewController presentViewController:vc animated:YES completion:nil];
}
}
```

* #### 姿势二: 纯代码

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    
    [self.window makeKeyAndVisible];

    self.window.rootViewController = [[ViewController alloc] init];    
    
    [self creatShortcutItem];

    UIApplicationShortcutItem *shortcutItem = [launchOptions valueForKey:UIApplicationLaunchOptionsShortcutItemKey];
    /** app不在后台(已杀死),处理逻辑,返回NO,不调用 -application:performActionForShortcutItem:completionHandler: */
    if (shortcutItem) {
        if([shortcutItem.type isEqualToString:@"open_search"]){
            DDYInfoLog(@"open_search");
        } else if ([shortcutItem.type isEqualToString:@"open_qrcode"]) {
            DDYInfoLog(@"open_qrcode");
        }
        return NO;
    }
    return YES;
}

#pragma mark - 3D Touch
#pragma mark 创建3DTouch列表
- (void)creatShortcutItem {
    // 创建系统风格的icon
    UIApplicationShortcutIcon *icon1 = [UIApplicationShortcutIcon iconWithType:UIApplicationShortcutIconTypeShare];
    UIApplicationShortcutItem *item1 = [[UIApplicationShortcutItem alloc] initWithType:@"open_search" localizedTitle:@"分享" localizedSubtitle:@"分享副标题" icon:icon1 userInfo:nil];
    
    // 创建自定义图标的icon
    UIApplicationShortcutIcon *icon2 = [UIApplicationShortcutIcon iconWithTemplateImageName:@"qrcode"];
    UIApplicationShortcutItem *item2 = [[UIApplicationShortcutItem alloc] initWithType:@"open_qrcode" localizedTitle:@"扫描" localizedSubtitle:@"扫描副标题" icon:icon2 userInfo:nil];
    
    // 添加到快捷选项数组
    [UIApplication sharedApplication].shortcutItems = @[item1, item2];
}

#pragma mark 修改3DTouch列表标签
- (void)editShortcutItem {
    // 获取第0个shortcutItem
    UIApplicationShortcutItem *shortcutItem = [[UIApplication sharedApplication].shortcutItems objectAtIndex:0];
    // 将shortcutItem0的类型由UIApplicationShortcutItem改为可修改类型UIMutableApplicationShortcutItem
    UIMutableApplicationShortcutItem *item = [shortcutItem mutableCopy];
    // 修改shortcutItem的标题
    [item setLocalizedTitle:@"修改"];
    [item setLocalizedSubtitle:@"修改副标题"];
    [item setIcon:[UIApplicationShortcutIcon iconWithType:UIApplicationShortcutIconTypeAlarm]];
    // 将shortcutItems数组改为可变数组
    NSMutableArray *itemArray = [[UIApplication sharedApplication].shortcutItems mutableCopy];
    // 替换原ShortcutItem
    [itemArray replaceObjectAtIndex:0 withObject:item];
    [UIApplication sharedApplication].shortcutItems = itemArray;
}

#pragma mark 后台状态点击3DTouch选项进入APP
/** app不在后台(已杀死)，则处理逻辑在 -application:didFinishLaunchingWithOptions:中 */
- (void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL))completionHandler {
    
    if (shortcutItem) {
        if([shortcutItem.type isEqualToString:@"open_search"]){
            DDYInfoLog(@"open_search");
        } else if ([shortcutItem.type isEqualToString:@"open_qrcode"]) {
            DDYInfoLog(@"open_qrcode");
            [self editShortcutItem];
        }
    }
    
    if (completionHandler) {
        completionHandler(YES);
    }
}
```


> ### Peak和Pop

1.在父级控制器中遵循 UIViewControllerPreviewingDelegate

2.在父级控制器中注册view，例如[self registerForPreviewingWithDelegate:self sourceView:cell]; 

3. 实现代理方法

```
#pragma mark Peek预览
- (nullable UIViewController *)previewingContext:(id <UIViewControllerPreviewing>)previewingContext viewControllerForLocation:(CGPoint)location
{
    // 获取按压的cell所在行，[previewingContext sourceView]就是按压的那个视图
    NSIndexPath *indexPath = [_tableView indexPathForCell:(UITableViewCell* )[previewingContext sourceView]];
    if (indexPath.section==0 && indexPath.row==0) {
        // 设定预览的界面
        UIViewController *childVC = [NSClassFromString(self.dataArray[indexPath.row]) vc];
        childVC.preferredContentSize = CGSizeMake(0.0f, 500.0f);
        // 调整不被虚化的范围，按压的那个cell不被虚化（轻轻按压时周边会被虚化，再少用力展示预览，再加力跳页至设定界面）
        CGRect rect = CGRectMake(0, 0, DDYSCREENW,40);
        previewingContext.sourceRect = rect;
        // 返回预览界面
        return childVC;
    }
    return nil;
}

#pragma mark Pop用力按则进入
- (void)previewingContext:(id <UIViewControllerPreviewing>)previewingContext commitViewController:(UIViewController *)viewControllerToCommit {
    [self showViewController:viewControllerToCommit sender:self];
}
```

4.子级控制器中实现

```
- (NSArray<id<UIPreviewActionItem>> *)previewActionItems {
    // 1
    UIPreviewAction *action1 = [UIPreviewAction actionWithTitle:@"Aciton1" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
        NSLog(@"Aciton1");
    }];
    // 2
    UIPreviewAction *action2 = [UIPreviewAction actionWithTitle:@"Aciton2" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
        NSLog(@"Aciton2");
    }];
    // 3
    UIPreviewAction *action3 = [UIPreviewAction actionWithTitle:@"Aciton3" style:UIPreviewActionStyleDefault handler:^(UIPreviewAction * _Nonnull action, UIViewController * _Nonnull previewViewController) {
        NSLog(@"Aciton3");
    }];
    // 先展示分组，点击分组进入分组内的actions
    UIPreviewActionGroup *group1 = [UIPreviewActionGroup actionGroupWithTitle:@"1" style:UIPreviewActionStyleDefault actions:@[action1, action2]];
    UIPreviewActionGroup *group2 = [UIPreviewActionGroup actionGroupWithTitle:@"2" style:UIPreviewActionStyleDestructive actions:@[action1, action3]];
    UIPreviewActionGroup *group3 = [UIPreviewActionGroup actionGroupWithTitle:@"3" style:UIPreviewActionStyleSelected actions:@[action2, action3]];
    
    NSArray *actions = @[group1, group2, group3];
    
    return actions;
}
```

