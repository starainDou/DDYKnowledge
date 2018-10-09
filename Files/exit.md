# 主动退出程序

```
typedef NS_ENUM(NSUInteger, AppExitTag) {
    AppExitTagExit = 1,
    AppExitTagAbort = 2,
    AppExitTagAssert = 3,
};

- (void)exitApplication {
    //直接退，看起来好像是 crash 所以做个动画
    [UIView beginAnimations:@"exitApplication" context:nil];
    [UIView setAnimationDuration:0.5];
    [UIView setAnimationDelegate:self];
    [UIView setAnimationTransition:UIViewAnimationTransitionFlipFromLeft forView:self.view.window cache:NO];
    [UIView setAnimationDidStopSelector:@selector(animationFinished:finished:context:)];
    self.view.window.bounds = CGRectMake(0, 0, 0, 0);
    [UIView commitAnimations];
}
 
- (void)animationFinished:(NSString *)animationID finished:(NSNumber *)finished context:(void *)context {
     if ([animationID compare:@"exitApplication"] == 0) {
        // 退出代码
        [self exitApp:AppExitTagExit];
    }
}

- (void)exitApp:(AppExitTag)tag {
	switch (tag) {
            case AppExitTestTag_Exit:
                exit(0);// exit(1)表示异常退出
                break;
            case AppExitTestTag_Abort:
                abort();
                break;
            case AppExitTestTag_Assert:
                assert(0);
                break;
            default:
                break;
	}
}

```