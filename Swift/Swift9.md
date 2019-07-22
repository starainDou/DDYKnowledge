# Swift9-加密

* ### MD5

	MD5(Message-Digest Algorithm 5)以512位分组来处理输入文本，每组又划分为16个32位子分组。算法的输出由四个32位分组组成，将它们级联形成一个128位散列值。
	
	```
	/// MD5 编码
    ///
    /// - Parameters:
    ///   - string: 需要编码的字符串
    ///   - lower: true:字母小写 false:字母大写
    /// - Returns: 编码结果
    public static func ddyMD5String(_ string: String?, lower: Bool) -> String? {
        guard let cStr = string?.cString(using: .utf8) else {
            return nil
        }
        let buffer = UnsafeMutablePointer<UInt8>.allocate(capacity: 16)
        CC_MD5(cStr,(CC_LONG)(strlen(cStr)), buffer)
        let md5String = NSMutableString();
        for i in 0 ..< 16 {
            if lower {
                md5String.appendFormat("%02x", buffer[i])
            } else {
                md5String.appendFormat("%02X", buffer[i])
            }
        }
        free(buffer)
        return md5String as String
    }	
	```
	
* ### Base64

	Base64是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64个可打印字符来表示二进制数据的方法。作用是Base64编码是从二进制到字符的过程，可用于在HTTP环境下传递较长的标识信息。
	
	```
	/// Base64 编解码
    ///
    /// - Parameters:
    ///   - string: 需要编解码的字符串
    ///   - encode: true:编码 false:解码
    /// - Returns: 编码结果
    public static func ddyBase64String(_ string: String?, encode: Bool) -> String? {
        if encode { // 编码
            guard let codingData = string?.data(using: .utf8) else {
                return nil
            }
            return codingData.base64EncodedString()
        } else { // 解码
            guard let newStr = string else {
                return nil
            }
            guard let decryptionData = Data(base64Encoded: newStr, options: .ignoreUnknownCharacters) else {
                return nil
            }
            return String.init(data: decryptionData, encoding: .utf8)
        }
    }
	```