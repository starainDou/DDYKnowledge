##### Mac OSX 校验文件 MD5/SHA1

```
# MD5
md5 filePath
# SHA1
shasum filePath 
# SHA1 
shasum -a 1 filePath
# SHA 256
shasum -a 256 filePath
```

##### 终端 sqlite3

```
// 创建或打开
sqlite3 SongListCache.sqlite

// 数据库文件所包含的所有自定义数据表
.tables

// 查看某个数据表所有内容
select * from MusicListTable_1000;

// https://www.cnblogs.com/Camier-myNiuer/p/5716826.html
```

##### 字符

```
半角空格（0×20）
制表符(t)
全角空格 (u3000)
u0009 CHARACTER TABULATION u000A LINE FEED u000D CARRIAGE RETURN u0020 SPACE u0085 NEXT LINE u00A0 NBSP u1680 OGHAM SPACE MARK u180E MONGOLIAN VOWEL SEPARATOR u2000 EN QUAD u200A HAIR SPACE u200B ZERO WIDTH SPACE u2028 LINE SEPARATOR u2029 PARAGRAPH SEPARATOR u202F NARROW NO-BREAK SPACE u205F MEDIUM MATHEMATICAL SPACE u3000 IDEOGRAPHIC SPACE
```

##### 文件已损坏解决

```
sudo xattr -d com.apple.quarantine /Applications/xxxx.app
```