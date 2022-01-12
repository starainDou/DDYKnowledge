* 由于使用markdown可能造成字符被转义等不确定错误。。。

> ###### 元字符

| 元字符 | 描述 |
| ------ | ------ |
| \ | 将下个字符标记符或一个向后引用或一个八进制转义符。例，“\\n”匹配\n。“\n”匹配换行符。“\\”匹配\ | 
| ^ | 匹配输入字符串的开始位置。如果设置了RegExp对象的Multiline属性，^也匹配“\n”或“\r”之后的位置 | 
| $ | 匹配输入字符串的结束位置。如果设置了RegExp对象的Multiline属性，$也匹配“\n”或“\r”之前的位置 | 
| * | 匹配前面的子表达式任意次。例如，zo*能匹配“z”，“zo”以及“zoo”，但是不匹配“bo”。*等价于{0,} | 
| + | 匹配前面的子表达式一次或多次(大于等于1次）。例，“zo+”能匹配“zo”以及“zoo”，但不能匹配“z”。+等价于{1,} | 
| ? | 匹配前面的子表达式零次或一次。例如，“do(es)?”可以匹配“do”或“does”中的“do”。?等价于{0,1} | 
| {n} | n是一个非负整数。匹配确定的n次。例如，“o{2}”不能匹配“Bob”中的“o”，但是能匹配“food”中的两个o | 
| {n,} | n是一个非负整数。至少匹配n次。例如，“o{2,}”不能匹配“Bob”中的“o”，但能匹配“foooood”中的所有o。“o{1,}”等价于“o+”。“o{0,}”则等价于“o*” | 
| {n,m} | m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次。例如，“o{1,3}”将匹配“fooooood”中的前三个o。“o{0,1}”等价于“o?”。请注意在逗号和两个数之间不能有空格 | 
| ? | 当该字符紧跟在任何一个其他限制符（*,+,?，{n}，{n,}，{n,m}）后面时，匹配模式是非贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串“oooo”，“o+?”将匹配单个“o”，而“o+”将匹配所有“o” | 
| .点 | 匹配除“\r\n”之外的任何单个字符。要匹配包括“\r\n”在内的任何字符，请使用像“[\s\S]”的模式 | 
| (pattern) | 匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“\(”或“\)” | 
| (?:pattern) | 非获取匹配，匹配pattern但不获取匹配结果，不进行存储供以后使用。这在使用或字符“(\|)”来组合一个模式的各个部分是很有用。例如“industr(?:y\|ies)”就是一个比“industry\|industries”更简略的表达式 | 
| (?=pattern) | 非获取匹配，正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如，“Windows(?=95\|98\|NT\|2000)”能匹配“Windows2000”中的“Windows”，但不能匹配“Windows3.1”中的“Windows”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始 | 
| (?!pattern) | 非获取匹配，正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如“Windows(?!95\|98\|NT\|2000)”能匹配“Windows3.1”中的“Windows”，但不能匹配“Windows2000”中的“Windows” | 
| (?<=pattern) | 非获取匹配，反向肯定预查，与正向肯定预查类似，只是方向相反。例如，“(?<=95\|98\|NT\|2000)Windows”能匹配“2000Windows”中的“Windows”，但不能匹配“3.1Windows”中的“Windows”。
| (?<!pattern) | 非获取匹配，反向否定预查，与正向否定预查类似，只是方向相反。例如“(?<!95\|98\|NT\|2000)Windows”能匹配“3.1Windows”中的“Windows”，但不能匹配“2000Windows”中的“Windows”。这个地方不正确，有问题 | 
| x\|y | 匹配x或y。例如，“z\|food”能匹配“z”或“food”(此处请谨慎)。“(z\|f)ood”则匹配“zood”或“food” | 
| [xyz] | 字符集合。匹配所包含的任意一个字符。例如，“[abc]”可以匹配“plain”中的“a” | 
| [^xyz] | 负值字符集合。匹配未包含的任意字符。例如，“[^abc]”可以匹配“plain”中的“plin” | 
| [a-z] | 字符范围。匹配指定范围内的任意字符。例如，“[a-z]”可以匹配“a”到“z”范围内的任意小写字母字符<br>注意:只有连字符在字符组内部时,并且出现在两个字符之间时,才能表示字符的范围; 如果出字符组的开头,则只能表示连字符本身 | 
| [^a-z] | 负值字符范围。匹配任何不在指定范围内的任意字符。例如，“[^a-z]”可以匹配任何不在“a”到“z”范围内的任意字符 | 
| \b | 匹配一个单词边界，也就是指单词和空格间的位置（即正则表达式的“匹配”有两种概念，一种是匹配字符，一种是匹配位置，这里的\b就是匹配位置的）。例如，“er\b”可以匹配“never”中的“er”，但不能匹配“verb”中的“er” | 
| \B | 匹配非单词边界。“er\B”能匹配“verb”中的“er”，但不能匹配“never”中的“er” | 
| \cx | 匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“c”字符 | 
| \d | 匹配一个数字字符。等价于[0-9] | 
| \D | 匹配一个非数字字符。等价于[^0-9] | 
| \f | 匹配一个换页符。等价于\x0c和\cL | 
| \n | 匹配一个换行符。等价于\x0a和\cJ | 
| \r | 匹配一个回车符。等价于\x0d和\cM | 
| \s | 匹配任何不可见字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v] | 
| \S | 匹配任何可见字符。等价于[^ \f\n\r\t\v] | 
| \t | 匹配一个制表符。等价于\x09和\cI | 
| \v | 匹配一个垂直制表符。等价于\x0b和\cK | 
| \w | 匹配包括下划线的任何单词字符。类似但不等价于“[A-Za-z0-9_]”，这里的"单词"字符使用Unicode字符集 | 
| \W | 匹配任何非单词字符。等价于“[^A-Za-z0-9_]” | 
| \xn | 匹配n，其中n为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，“\x41”匹配“A”。“\x041”则等价于“\x04&1”。正则表达式中可以使用ASCII编码 | 
| \num | 匹配num，其中num是一个正整数。对所获取的匹配的引用。例如，“(.)\1”匹配两个连续的相同字符 | 
| \n | 标识一个八进制转义值或一个向后引用。如果\n之前至少n个获取的子表达式，则n为向后引用。否则，如果n为八进制数字（0-7），则n为一个八进制转义值 | 
| \nm | 标识一个八进制转义值或一个向后引用。如果\nm之前至少有nm个获得子表达式，则nm为向后引用。如果\nm之前至少有n个获取，则n为一个后跟文字m的向后引用。如果前面的条件都不满足，若n和m均为八进制数字（0-7），则\nm将匹配八进制转义值nm | 
| \nml | 如果n为八进制数字（0-7），且m和l均为八进制数字（0-7），则匹配八进制转义值nml | 
| \un | 匹配n，其中n是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配版权符号（&copy;） | 
| \p{P} | 小写 p 是 property 的意思，表示 Unicode 属性，用于 Unicode 正表达式的前缀。中括号内的“P”表示Unicode 字符集七个字符属性之一：标点字符 <br>其他六个属性:<br> L：字母；<br>M：标记符号（一般不会单独出现）；<br>Z：分隔符（比如空格、换行等）；<br>S：符号（比如数学符号、货币符号等）；<br>N：数字（比如阿拉伯数字、罗马数字等）；<br>C：其他字符。<br>*注：此语法部分语言不支持，例：javascript。| 
| < > | 匹配词（word）的开始（<）和结束（>）。例如正则表达式<the>能够匹配字符串"for the wise"中的"the"，但是不能匹配字符串"otherwise"中的"the"。注意：这个元字符不是所有的软件都支持的 | 
| ( ) |  将( 和 ) 之间的表达式定义为“组”（group），并且将匹配这个表达式的字符保存到一个临时区域（一个正则表达式中最多可以保存9个），它们可以用 \1 到\9 的符号来引用 |
|  \| | 将两个匹配条件进行逻辑“或”（Or）运算。例如正则表达式(him\|her) 匹配"it belongs to him"和"it belongs to her"，但是不能匹配"it belongs to them."。注意：这个元字符不是所有的软件都支持的 | 
| + | 匹配1或多个正好在它之前的那个字符。例如正则表达式9+匹配9、99、999等。注意：这个元字符不是所有的软件都支持的 | 
| ? | 匹配0或1个正好在它之前的那个字符。注意：这个元字符不是所有的软件都支持的 | 
| {i} {i,j} | 匹配指定数目的字符，这些字符是在它之前的表达式定义的。例如正则表达式A[0-9]{3} 能够匹配字符"A"后面跟着正好3个数字字符的串，例如A123、A348等，但是不匹配A1234。而正则表达式[0-9]{4,6} 匹配连续的任意4个、5个或者6个数字 |

> ###### 常用正则

| 作用 | 正则 | 评注 |
| ------ | ------ | ------ |
| 匹配中文字符 | [u4e00-u9fa5] | 匹配中文还真是个头疼的事，有了这个表达式就好办了 | 
| 匹配双字节字符(包括汉字在内) | [^x00-xff] | 可以用来计算字符串的长度（一个双字节字符长度计2，ASCII字符计1） | 
| 匹配空白行 | ns*r | 可以用来删除空白行 |  
| 匹配HTML标记 | <(S*?)[^>]*>.*?\|<.*? /> | 网上流传的版本太糟糕，上面这个也仅仅能匹配部分，对于复杂的嵌套标记依旧无能为力 |  
| 匹配首尾空白字符 | ^s*\|s*$  | 可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式 
| 匹配Email地址 | w+([-+.]w+)*@w+([-.]w+)*.w+([-.]w+)* | 表单验证时很实用 | 
| 匹配网址URL | [a-zA-z]+://[^s]* | 网上流传的版本功能很有限，上面这个基本可以满足需求 |  
| 匹配帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线) | ^[a-zA-Z][a-zA-Z0-9_]{4,15}$ | 表单验证时很实用 |  
| 匹配国内电话号码 | d{3}-d{8}\|d{4}-d{7} | 匹配形式如 0511-4405222 或 021-87888822 | 
| 匹配腾讯QQ号 | [1-9][0-9]{4,} | 腾讯QQ号从10000开始 |  
| 匹配中国邮政编码 | [1-9]d{5}(?!d) | 中国邮政编码为6位数字 |  
| 匹配身份证 | d{15}\|d{18} | 中国的身份证为15位或18位 |  
| 匹配ip地址 | d+.d+.d+.d+ | 评注：提取ip地址时有用 |  

| 作用 | 正则 |
| ------ | ------ |
|正整数 |  ^[1-9]d*$ | 　 　
|负整数 | ^-[1-9]d*$ | 
|整数 | ^-?[1-9]d*$ | 
|非负整数 | ^[1-9]d*\|0$ | 
|非正整数 | ^-[1-9]d*\|0$ |
|正浮点数 | ^[1-9]d*.d*\|0.d*[1-9]d*$ | 
|负浮点数 | ^-([1-9]d*.d*\|0.d*[1-9]d*)$ | 
|浮点数 | ^-?([1-9]d*.d*\|0.d*[1-9]d*\|0?.0+\|0)$ | 
|非负浮点数 | ^[1-9]d*.d*\|0.d*[1-9]d*\|0?.0+\|0$ | 
|非正浮点数 | ^(-([1-9]d*.d*\|0.d*[1-9]d*))\|0?.0+\|0$ | 
|26个英文字母组成字符串 | ^[A-Za-z]+$ | 　　 
|26个英文大写字母组成字符串 | ^[A-Z]+$ | 
|26个英文小写字母组成字符串 | ^[a-z]+$ | 　　
|数字和英文字母组成字符串 | ^[A-Za-z0-9]+$ | 　　
|数字、字母或下划线组成字符串 | ^w+$ | 　　 
|只能输入数字 | “^[0-9]*$” |  
|只能输入n位的数字 | “^d{n}$” |  
|只能输入至少n位数字 | “^d{n,}$”  | 
|只能输入m-n位的数字 | “^d{m,n}$”  | 
|只能输入零和非零开头的数字 | “^(0\|[1-9][0-9]*)$”  | 
|只能输入有两位小数的正实数 | “^[0-9]+(.[0-9]{2})?$” |  
|只能输入有1-3位小数的正实数 | “^[0-9]+(.[0-9]{1,3})?$”  | 
|只能输入非零的正整数 | “^+?[1-9][0-9]*$”  | 
|只能输入非零的负整数 | “^-[1-9][0-9]*$” |  
|只能输入长度为3的字符 | “^.{3}$” |  
|只能输入由26个英文字母组成的字符串 | “^[A-Za-z]+$”  | 
|只能输入由26个大写英文字母组成的字符串 | “^[A-Z]+$”  | 
|只能输入由26个小写英文字母组成的字符串 | “^[a-z]+$”  | 
|只能输入由数字和26个英文字母组成的字符串 | “^[A-Za-z0-9]+$”  | 
|只能输入由数字、26个英文字母或者下划线组成的字符串 | “^w+$”  | 
|验证用户密码<br>格式为：以字母开头，长度在6-18之间，只能包含字符、数字和下划线 | “^[a-zA-Z]w{5,17}$” |
|验证是否含有^%&'',;=?$"等字符 | “[^%&'',;=?$x22]+” | 
|只能输入汉字 | “^[u4e00-u9fa5],{0,}$”  |
|Email地址 | “^w+[-+.]w+)*@w+([-.]w+)*.w+([-.]w+)*$”  |
|InternetURL | “^http://([w-]+.)+[w-]+(/[w-./?%&=]*)?$”  |
|电话号码<br>格式为:<br>“XXXX-XXXXXXX”，<br>“XXXX-XXXXXXXX”，<br>“XXX-XXXXXXX”，<br>“XXX-XXXXXXXX”，<br>“XXXXXXX”，<br>“XXXXXXXX” | “^((d{3,4})\|d{3,4}-)?d{7,8}$” | 
|验证身份证号（15位或18位数字） | “^d{15}\|d{}18$”  |
|验证一年的12个月<br>正确格式为：“01”-“09”和“1”“12”  | “^(0?[1-9]\|1[0-2])$” |
|验证一个月的31天 | “^((0?[1-9])\|((1\|2)[0-9])\|30\|31)$”  |
|正确格式为 | “01”“09”和“1”“31” |
|匹配中文字符的正则表达式 |  [u4e00-u9fa5] |
|匹配双字节字符(包括汉字在内) | [^x00-xff] |
|匹配空行的正则表达式 | n[s\| ]*r |
|匹配HTML标记的正则表达式 | /<(.*)>.*\|<(.*) />/ |
|匹配首尾空格的正则表达式 | (^s*)\|(s*$) |
|匹配Email地址的正则表达式 | w+([-+.]w+)*@w+([-.]w+)*.w+([-.]w+)* |
|匹配网址URL | http://([w-]+.)+[w-]+(/[w- ./?%&=]*)?| 


例如：判断是否是合法的整数或浮点数
NSString *regex = @"^-?([1-9]d*|[1-9]d*.d*|0.d*[1-9]d*|0?.0+|0)$";
NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",regex];
BOOL result = [pred evaluateWithObject:@"要判断的string"];


> ###### NSPredicate的用法


* 数组筛选
读入了一个array1，然后想把array2中符合array1中内容的元素过滤出来。
傻瓜式做法就是两个for循环，一个一个进行比较，效率极低。
其实一个循环甚至无循环就可以搞定了，那就需要用NSPredicate

一个循环

```
NSMutableArray *filterArray = [NSMutableArray arrayWithObjects:@"pict", @"blackrain", @"ip", nil];
NSMutableArray *contentArray = [NSMutableArray arrayWithObjects:@"I am a picture.", @"I am a guy", @"I am gagaga", @"ipad", @"iphone", nil];
for (NSString *tempFilter in filterArray) {
  NSPredicate *filterPredicate = [NSPredicate predicateWithFormat:@"SELF CONTAINS %@", tempFilter];
  NSArray *tempResultArray = [contentArray filteredArrayUsingPredicate:filterPredicate];
  NSLog(@"result:%@",tempResultArray);
}
```

无需循环 过滤出来不包含filterArray中的所有item的元素。
```
NSMutableArray *filterArray = [NSMutableArray arrayWithObjects:@"abc1", @"abc2", nil];
NSMutableArray *contentArray = [NSMutableArray arrayWithObjects:@"a1", @"abc1", @"abc4", @"abc2", nil];
NSPredicate *thePredicate = [NSPredicate predicateWithFormat:@"NOT (SELF in %@)", filterArray];
[contentArray filterUsingPredicate:thePredicate];
```

1） 当使用聚合类的操作符(SELF in 集合操作符)时是可以不需要循环的 
2）当使用单个比较类(CONTAINS 字符串比较操作符)的操作符时可以一个循环来搞定 CONTAINS


* 文件筛选
```
// 生成文件路径下文件集合列表
NSString *defaultPath = [[NSBundle mainBundle] resourcePath];
NSArray *contentsDict = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:defaultPath error:nil];

// 简单比较
NSString *match = @"ddyImg-999.png";
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF == %@", match];
NSArray *results1 = [contentsDict filteredArrayUsingPredicate:predicate];﻿
// match里like的用法（类似Sql中的用法）
NSString *match = @"ddyImg-*.png";
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF like %@", match];
NSArray *results2 = [directoryContents filteredArrayUsingPredicate:predicate];﻿
// 大小写比较 ［c］表示忽略大小写，［d］表示忽略重音，可以在一起使用，如下：
NSString *match = @"ddyImg-*.png";
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF like[cd] %@", match];
NSArray *results3 = [directoryContents filteredArrayUsingPredicate:predicate];﻿
// 使用正则
NSString *match = @"ddyImg-\\d{3}\\.png";  //ddyImg-123.png
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF matches %@", match];
NSArray *results4 = [directoryContents filteredArrayUsingPredicate:predicate];﻿
```

* Format：
  * (1)比较运算符>,<,==,>=,<=,!= 
可用于数值及字符串
例：@"number > 100"
  * (2)范围运算符：IN、BETWEEN
例：@"number BETWEEN {1,5}"
      @"address IN {'shanghai','beijing'}"
  * (3)字符串本身:SELF 
例：@“SELF == ‘APPLE’"
  * (4)字符串相关：BEGINSWITH、ENDSWITH、CONTAINS
例：@"name CONTAIN[cd] 'ang'"   //包含某个字符串
       @"name BEGINSWITH[c] 'sh'"     //以某个字符串开头
       @"name ENDSWITH[d] 'ang'"      //以某个字符串结束
        注:[c]不区分大小写[d]不区分发音符号即没有重音符号[cd]既不区分大小写，也不区分发音符号。
  * (5)通配符：LIKE
例：@"name LIKE[cd] '*er*'"    //*代表通配符,Like也接受[cd].
       @"name LIKE[cd] '???er*'"
  * (6)正则表达式：MATCHES
例：NSString *regex = @"^A.+e$";   //以A开头，e结尾
      @"name MATCHES %@",regex

> ###### 实际应用

* 对NSArray进行过滤 
```
NSArray *array = [[NSArray alloc]initWithObjects:@"beijing",@"shanghai",@"guangzou",@"wuhan", nil];    
NSString *string = @"ang";    
NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF CONTAINS %@",string];    
NSLog(@"%@",[array filteredArrayUsingPredicate:pred]);
```

* 判断字符串首字母是否为字母
```
NSString *regex = @"[A-Za-z]+";    
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex];    
    
if ([predicate evaluateWithObject:aString]) {    
} 
```
* 字符串替换
```
NSError* error = NULL;    
NSRegularExpression* regex = [NSRegularExpression regularExpressionWithPattern:@"(encoding=\")[^\"]+(\")"    
                                                                            options:0    
                                                                            error:&error];    
NSString* sample = @"<xml encoding=\"abc\"></xml><xml encoding=\"def\"></xml><xml encoding=\"ttt\"></xml>";    
NSLog(@"Start:%@",sample);    
NSString* result = [regex stringByReplacingMatchesInString:sample    
                                                      options:0    
                                                       range:NSMakeRange(0, sample.length)    
                                                      withTemplate:@"$1utf-8$2"];    
NSLog(@"Result:%@", result); 
```

* 截取字符串
```
//组装一个字符串，需要把里面的网址解析出来    
NSString *urlString=@"<meta/><link/><title>1Q84 BOOK1</title></head><body>";    
    
//NSRegularExpression类里面调用表达的方法需要传递一个NSError的参数。下面定义一个      
NSError *error;    
    
//http+:[^\\s]* 这个表达式是检测一个网址的。(?<=title\>).*(?=</title)截取html文章中的<title></title>中内文字的正则表达式    
NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"(?<=title\\>).*(?=</title)" options:0 error:&error];    
    
if (regex != nil) {    
    NSTextCheckingResult *firstMatch=[regex firstMatchInString:urlString options:0 range:NSMakeRange(0, [urlString length])];    
        
    if (firstMatch) {    
        NSRange resultRange = [firstMatch rangeAtIndex:0];    
            
        //从urlString当中截取数据    
        NSString *result=[urlString substringWithRange:resultRange];    
        //输出结果    
        NSLog(@"->%@<-",result);    
    }    
        
}
```

* NSDate进行筛选
```
 
//日期在十天之内:  
NSDate *endDate = [[NSDate date] retain];  
NSTimeInterval timeInterval= [endDate timeIntervalSinceReferenceDate];  
timeInterval -=3600*24*10;  
NSDate *beginDate = [[NSDate dateWithTimeIntervalSinceReferenceDate:timeInterval] retain];  
//对coredata进行筛选(假设有fetchRequest)  
NSPredicate *predicate_date =  
[NSPredicate predicateWithFormat:@"date >= %@ AND date <= %@", beginDate,endDate];  
      
[fetchRequest setPredicate:predicate_date];  
//释放retained的对象  
[endDate release];  
[beginDate release]; 
```

> ###### 额外补充

* 创建谓词

> predicateWithFormat:
NSPredicate *predicate;
predicate = [NSPredicate predicateWithFormat:@"name == 'Herbie'"];
注意:如果谓词串中的文本块未被引用,则被看做是键路径,即需要用引号表明是字符串,单引号,双引号均可.键路径可以在后台包含许多强大的功能
计算谓词:
BOOL match = [predicate evaluateWithObject:car];
让谓词通过某个对象来计算自己的值,给出BOOL值

* 燃料过滤器
> filteredArrayUsingPredicate:是NSArray数组的一种类别方法,循环过滤数组中的内容,将值为YES的对象累积到结果数组中返回
iphone编程应该密切注意谓词使用带来的性能问题

* 格式说明符

>%d和%@等插入数值和字符串,%K表示key
还可以引入变量名,用$,类似环境变量,如:@"name == $NAME",再用predicateWithSubstitutionVariables调用来构造新的谓词(键/值字典),其中键是变量名,值是要插入的内容,注意这种情况下不能把变量当成键路径,只能用作值

* 比较和逻辑运算符

> ==等于
\>:大于
\>=和=>:大于或等于
<:小于
<=和=<:小于或等于
!=和<>:不等于
括号和逻辑运算AND、OR、NOT或者C样式的等效表达式&&、||、!
注意：不等号适用于数字和字符串

* 数组运算符

> BETWEEN和IN后加某个数组，可以用{50,200}，也可以用%@格式说明符插入自己的对象，也可以用变量

* SELF
> self就表示对象本身

* 字符串运算符
> BEGINSWITH
ENDSWITH
CONTAINS
[c],[d],[cd],后缀表示不区分大小写，不区分发音符号，两这个都不区分

* LIKE运算符
> 类似SQL的LIKES
LIKE，与通配符“*”表示任意多和“?”表示一个结合使用
LIKE也接受[cd]符号
MATCHES可以使用正则表达式
```
Car *makeCar (NSString *name, NSString *make, NSString *model,  
              int modelYear, int numberOfDoors, float mileage,  
              int horsepower) {  
    Car *car = [[[Car alloc] init] autorelease];  
      
    car.name = name;  
    car.make = make;  
    car.model = model;  
    car.modelYear = modelYear;  
    car.numberOfDoors = numberOfDoors;  
    car.mileage = mileage;  
      
    Slant6 *engine = [[[Slant6 alloc] init] autorelease];  
    [engine setValue: [NSNumber numberWithInt: horsepower]  
              forKey: @"horsepower"];  
    car.engine = engine;  
      
      
    // Make some tires.  
    // int i;  
    for (int i = 0; i < 4; i++) {  
        Tire * tire= [[[Tire alloc] init] autorelease];  
        [car setTire: tire  atIndex: i];  
    }  
      
    return (car);  
      
} // makeCar  
  
  
int main (int argc, const char * argv[])  
{  
    NSAutoreleasePool *pool;  
    pool = [[NSAutoreleasePool alloc] init];  
      
    Garage *garage = [[Garage alloc] init];  
    garage.name = @"Joe's Garage";  
      
    Car *car;  
    car = makeCar (@"Herbie", @"Honda", @"CRX", 1984, 2, 34000, 58);  
    [garage addCar: car];  
  
    NSPredicate *predicate;  
    predicate = [NSPredicate predicateWithFormat: @"name == 'Herbie'"];  
    BOOL match = [predicate evaluateWithObject: car];  
    NSLog (@"%s", (match) ? "YES" : "NO");  
      
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower > 150"];  
    match = [predicate evaluateWithObject: car];  
    NSLog (@"%s", (match) ? "YES" : "NO");  
      
    predicate = [NSPredicate predicateWithFormat: @"name == %@", @"Herbie"];  
    match = [predicate evaluateWithObject: car];  
    NSLog (@"%s", (match) ? "YES" : "NO");  
      
    predicate = [NSPredicate predicateWithFormat: @"%K == %@", @"name", @"Herbie"];  
    match = [predicate evaluateWithObject: car];  
    NSLog (@"%s", (match) ? "YES" : "NO");  
      
    NSPredicate *predicateTemplate = [NSPredicate predicateWithFormat:@"name == $NAME"];  
    NSDictionary *varDict;  
    varDict = [NSDictionary dictionaryWithObjectsAndKeys:  
               @"Herbie", @"NAME", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables: varDict];  
    NSLog(@"SNORGLE: %@", predicate);  
    match = [predicate evaluateWithObject: car];  
    NSLog (@"%s", (match) ? "YES" : "NO");  
      
      
    car = makeCar (@"Badger", @"Acura", @"Integra", 1987, 5, 217036.7, 130);  
    [garage addCar: car];  
      
    car = makeCar (@"Elvis", @"Acura", @"Legend", 1989, 4, 28123.4, 151);  
    [garage addCar: car];  
      
    car = makeCar (@"Phoenix", @"Pontiac", @"Firebird", 1969, 2, 85128.3, 345);  
    [garage addCar: car];  
      
    car = makeCar (@"Streaker", @"Pontiac", @"Silver Streak", 1950, 2, 39100.0, 36);  
    [garage addCar: car];  
      
    car = makeCar (@"Judge", @"Pontiac", @"GTO", 1969, 2, 45132.2, 370);  
    [garage addCar: car];  
      
    car = makeCar (@"Paper Car", @"Plymouth", @"Valiant", 1965, 2, 76800, 105);  
    [garage addCar: car];  
      
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower > 150"];  
    NSArray *cars = [garage cars];  
    for (Car *car in [garage cars]) {  
        if ([predicate evaluateWithObject: car]) {  
            NSLog (@"%@", car.name);  
        }  
    }  
  
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower > 150"];  
    NSArray *results;  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    NSArray *names;  
    names = [results valueForKey:@"name"];  
    NSLog (@"%@", names);  
      
    NSMutableArray *carsCopy = [cars mutableCopy];  
    [carsCopy filterUsingPredicate: predicate];  
    NSLog (@"%@", carsCopy);  
      
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower > %d", 50];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicateTemplate = [NSPredicate predicateWithFormat: @"engine.horsepower > $POWER"];  
    varDict = [NSDictionary dictionaryWithObjectsAndKeys:  
               [NSNumber numberWithInt: 150], @"POWER", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables: varDict];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat:  
                 @"(engine.horsepower > 50) AND (engine.horsepower < 200)"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"oop %@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name < 'Newton'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", [results valueForKey: @"name"]);  
      
    predicate = [NSPredicate predicateWithFormat:  
                 @"engine.horsepower BETWEEN { 50, 200 }"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    NSArray *betweens = [NSArray arrayWithObjects:  
                         [NSNumber numberWithInt: 50], [NSNumber numberWithInt: 200], nil];  
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower BETWEEN %@", betweens];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
  
    predicateTemplate = [NSPredicate predicateWithFormat: @"engine.horsepower BETWEEN $POWERS"];  
    varDict = [NSDictionary dictionaryWithObjectsAndKeys: betweens, @"POWERS", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables: varDict];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name IN { 'Herbie', 'Snugs', 'Badger', 'Flap' }"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", [results valueForKey: @"name"]);  
  
    predicate = [NSPredicate predicateWithFormat: @"SELF.name IN { 'Herbie', 'Snugs', 'Badger', 'Flap' }"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", [results valueForKey: @"name"]);  
      
    names = [cars valueForKey: @"name"];  
    predicate = [NSPredicate predicateWithFormat: @"SELF IN { 'Herbie', 'Snugs', 'Badger', 'Flap' }"];  
    results = [names filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name BEGINSWITH 'Bad'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name BEGINSWITH 'HERB'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name BEGINSWITH[cd] 'HERB'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name LIKE[cd] '*er*'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name LIKE[cd] '???er*'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    NSArray *names1 = [NSArray arrayWithObjects: @"Herbie", @"Badger", @"Judge", @"Elvis", nil];  
    NSArray *names2 = [NSArray arrayWithObjects: @"Judge", @"Paper Car", @"Badger", @"Phoenix", nil];  
  
    predicate = [NSPredicate predicateWithFormat: @"SELF IN %@", names1];  
    results = [names2 filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", predicate);  
    NSLog (@"%@", results);  
      
          
    return 0;  
  
      
      
    predicate = [NSPredicate predicateWithFormat: @"modelYear > 1970"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"name contains[cd] 'er'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
  
    predicate = [NSPredicate predicateWithFormat: @"name beginswith 'B'"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"%K beginswith %@",  
                 @"name", @"B"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"with args : %@", results);  
      
    predicateTemplate = [NSPredicate predicateWithFormat: @"name beginswith $NAME"];  
    NSDictionary *dict = [NSDictionary   
                          dictionaryWithObjectsAndKeys: @"Bad", @"NAME", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables:dict];  
    NSLog (@"SNORGLE: %@", predicate);  
      
    predicate = [NSPredicate predicateWithFormat: @"name in { 'Badger', 'Judge', 'Elvis' }"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicateTemplate = [NSPredicate predicateWithFormat: @"name in $NAME_LIST"];  
    names = [NSArray arrayWithObjects:@"Badger", @"Judge", @"Elvis", nil];  
    dict = [NSDictionary   
                          dictionaryWithObjectsAndKeys: names, @"NAME_LIST", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables:dict];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicateTemplate = [NSPredicate predicateWithFormat: @"%K in $NAME_LIST", @"name"];  
    dict = [NSDictionary   
    dictionaryWithObjectsAndKeys: names, @"NAME_LIST", nil];  
    predicate = [predicateTemplate predicateWithSubstitutionVariables:dict];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
    NSLog (@"xSNORGLE: %@", predicate);  
      
    // SELF is optional here.  
    predicate = [NSPredicate predicateWithFormat:@"SELF.name in { 'Badger', 'Judge', 'Elvis' }"];  
      
    for (Car *car in cars) {  
        if ([predicate evaluateWithObject: car]) {  
            NSLog (@"SNORK : %@ matches", car.name);  
        }  
    }  
      
      
#if 0  
    predicate = [NSPredicate predicateWithFormat: @"ANY engine.horsepower > 200"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"SNORGLE: %@", predicate);  
#endif  
      
    predicate = [NSPredicate predicateWithFormat: @"engine.horsepower > 200"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
      
    predicate = [NSPredicate predicateWithFormat: @"tires.@sum.pressure > 10"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
  
#if 0  
    predicate = [NSPredicate predicateWithFormat: @"ALL engine.horsepower > 30"];  
    results = [cars filteredArrayUsingPredicate: predicate];  
    NSLog (@"%@", results);  
#endif  
      
      
    [garage release];  
      
    [pool release];  
      
    return (0);  
      
}
 ```
> 判断字符串首字母是否为字母。 
Objective-c代码 
NSString *regex = @"[A-Za-z]+"; 
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", regex]; 
if ([predicate evaluateWithObject:aString]) { 
} 


