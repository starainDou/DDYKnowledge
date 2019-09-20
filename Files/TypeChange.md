常用类型转换

> # NSData 与 NSString

```
// NSData－> NSString
NSString *aString = [[NSString alloc] initWithData:adataencoding:NSUTF8StringEncoding];

// NSString－>NSData
NSString *aString = @"1234abcd";
NSData *aData = [aString dataUsingEncoding: NSUTF8StringEncoding];
```


> # NSData 与 Byte

```
// NSData－> Byte数组
NSString *testString = @"1234567890";
NSData *testData = [testString dataUsingEncoding: NSUTF8StringEncoding];

Byte *testByte = (Byte *)[testData bytes];
for(int i = 0;i<[testData length];i++) {
  printf("testByte = %d\n",testByte[i]);
}

// Byte数组－> NSData
Byte byte[] = {0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23};
NSData *adata = [[NSData alloc] initWithBytes:byte length:24];
```

> # Byte数组和16进制数


```
// Byte数组－>16进制数

Byte *bytes = (Byte *)[aData bytes];
NSString *hexStr= @"";

for(int i = 0;i<[encryData length];i++) {
  NSString *newHexStr = [NSString stringWithFormat:@"%x",bytes[i]&0xff];// 16进制数
  if([newHexStr length]==1) {
    hexStr = [NSString stringWithFormat:@"%@0%@",hexStr,newHexStr];
  } else {
    hexStr = [NSString stringWithFormat:@"%@%@",hexStr,newHexStr];
  }
}
NSLog(@"bytes 的16进制数为:%@",hexStr);

// 16进制数－>Byte数组
NSString *hexString = @"3e435fab9c34891f"; //16进制字符串
int j=0;
Byte bytes[128];  // 3ds key的Byte 数组， 128位

for(int i=0;i<[hexString length];i++) {
  // 两位16进制数转化后的10进制数
  int int_ch;  
  // 两位16进制数中的第一位(高位*16)
  unichar hex_char1 = [hexString characterAtIndex:i]; 
  int int_ch1;
  if(hex_char1 >= '0' && hex_char1 <='9') {
    // 0 的Ascll - 48
    int_ch1 = (hex_char1-48)*16;   
  } else if(hex_char1 >= 'A' && hex_char1 <='F') {
    // A 的Ascll - 65
    int_ch1 = (hex_char1-55)*16; 
  } else {
    // a 的Ascll - 97
    int_ch1 = (hex_char1-87)*16; 
  }
  i++;
// 两位16进制数中的第二位(低位)
  unichar hex_char2 = [hexString characterAtIndex:i]; 
  int int_ch2;
  if(hex_char2 >= '0' && hex_char2 <='9') {
    // 0 的Ascll - 48
    int_ch2 = (hex_char2-48); 
  } else if(hex_char1 >= 'A' && hex_char1 <='F') {
    // A 的Ascll - 65
    int_ch2 = hex_char2-55;
  } else {
    // a 的Ascll - 97
    int_ch2 = hex_char2-87; 
  }
  int_ch = int_ch1+int_ch2;
  NSLog(@"int_ch=%d",int_ch);
  // 将转化后的数放入Byte数组里
  bytes[j] = int_ch;  
  j++;
}
NSData *newData = [[NSData alloc] initWithBytes:bytes length:128];
NSLog(@"newData=%@",newData);
```


> # NSData 与 UIImage

```
// NSData－>UIImage
UIImage *aimage = [UIImage imageWithData:imageData]; 

//例：从本地文件沙盒中取图片并转换为NSData
NSString *path = [[NSBundle mainBundle] bundlePath];
NSString *name = [NSString stringWithFormat:@"ceshi.png"];
NSString *finalPath = [path stringByAppendingPathComponent:name];
NSData *imageData = [NSData dataWithContentsOfFile: finalPath];
UIImage *aimage = [UIImage imageWithData: imageData];

// UIImage－> NSData
NSData *imageData = UIImagePNGRepresentation(aimae);
```

> # int 和 byte[]

```
// 将int数值转换为占四个字节的byte数组，本方法适用于(高位在前，低位在后)的顺序。 和bytesToInt2（）配套使用  
public static byte[] intToBytes2(int value) { 
  byte[] src = new byte[4]; 
  src[0] = (byte) ((value>>24) & 0xFF); 
  src[1] = (byte) ((value>>16)& 0xFF); 
  src[2] = (byte) ((value>>8)&0xFF); 
  src[3] = (byte) (value & 0xFF); 
  return src; 
}

// byte数组中取int数值，本方法适用于(低位在后，高位在前)的顺序。和intToBytes2（）配套使用  
public static int bytesToInt2(byte[] src, int offset) { 
  int value; 
  value = (int) (((src[offset] & 0xFF)<<24) | ((src[offset+1] & 0xFF)<<16) | ((src[offset+2] & 0xFF)<<8) | (src[offset+3] & 0xFF)); 
  return value; 
}
```