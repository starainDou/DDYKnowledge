NS_ENUM_AVAILABLE(10_0, 6_0);

```
// NSParagraphStyle
typedef NS_ENUM(NSInteger, NSLineBreakMode) {
    NSLineBreakByWordWrapping = 0,     	// Wrap at word boundaries, default
    NSLineBreakByCharWrapping,		// Wrap at character boundaries
    NSLineBreakByClipping,		// Simply clip
    NSLineBreakByTruncatingHead,	// Truncate at head of line: "...wxyz"
    NSLineBreakByTruncatingTail,	// Truncate at tail of line: "abcd..."
    NSLineBreakByTruncatingMiddle	// Truncate middle of line:  "ab...yz"
} NS_ENUM_AVAILABLE(10_0, 6_0);
```


NS_DEPRECATED_IOS(2_0,6_0) __TVOS_PROHIBITED; Deprecated: use NSLineBreakMode instead

```
typedef NS_ENUM(NSInteger, UILineBreakMode) {
    UILineBreakModeWordWrap = 0,            // Wrap at word boundaries
    UILineBreakModeCharacterWrap,           // Wrap at character boundaries
    UILineBreakModeClip,                    // Simply clip when it hits the end of the rect
    UILineBreakModeHeadTruncation,          // Truncate at head of line: "...wxyz". Will truncate multiline text on first line
    UILineBreakModeTailTruncation,          // Truncate at tail of line: "abcd...". Will truncate multiline text on last line
    UILineBreakModeMiddleTruncation,        // Truncate middle of line:  "ab...yz". Will truncate multiline text in the middle
} NS_DEPRECATED_IOS(2_0,6_0) __TVOS_PROHIBITED;
```

因为映射的枚举值一样所以即使 NS* 用了 UI* 也不会出错

* NSLineBreakByWordWrapping;以单词为显示单位显示，后面部分省略不显示。
* NSLineBreakByCharWrapping;以字符为显示单位显示，后面部分省略不显示。
* NSLineBreakByClipping;剪切与文本宽度相同的内容长度，后半部分被删除。
* NSLineBreakByTruncatingHead;前面部分文字以……方式省略，显示尾部文字内容。
* NSLineBreakByTruncatingTail;结尾部分的内容以……方式省略，显示头的文字内容。
* NSLineBreakByTruncatingMiddle;中间的内容以……方式省略，显示头尾的文字内容。
