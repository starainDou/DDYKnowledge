> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.cnblogs.com/yangyxd/p/10044094.html

在 Flutter 中，我们可以用下面的代码从文件中加载图像：

```
Image.file(File(_fileName));

```

这个时候，当_fileName 这个文件名称和路径不变，文件内容变化时，Flutter 并不会更新显示。问题产生的原因是 Flutter 自动使用了缓存。

那么怎么办呢？

我查看到，Image.file 实际上会将 image 设置为 FileImage 这个 ImageProvider。FileImage 的代码中，在进行 operator 时，只判断了文件路径和缩放比例。正是因为如此，我们文件路径不变，缩放比例不变时，Flutter 会认为我们还是用的原图，并不会重新进行加载。

于是，我想到了办法是扩展一个 FileImage，将这个 operator 的方式改一下。

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
class FileImageEx extends FileImage {
  int fileSize;
  FileImageEx(File file, { double scale = 1.0 })
      : assert(file != null),
        assert(scale != null),
        super(file, scale: scale) {
    fileSize = file.lengthSync();
  }

  @override
  bool operator ==(dynamic other) {
    if (other.runtimeType != runtimeType)
      return false;
    final FileImageEx typedOther = other;
    return file?.path == typedOther.file?.path
        && scale == typedOther.scale && fileSize == typedOther.fileSize;
  }

}

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

接下来，直接使用下面的代码来加载图像：

```
Image(image: FileImageEx(File(_fileName)));

```

经测试，问题得到解决。

如果您有更好的方式，欢迎留言。