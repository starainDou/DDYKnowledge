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
	
* ### Sha1           

	SHA1和MD5本质都是摘要函数，长度不同(MD5是128位,SHA1是160位,SHA256是256位)   
	
	```
	public static func ddySha1String(_ string: String?) -> String? {
        guard let codingData = string?.data(using: .utf8) else {
            return nil
        }
        var digest = [UInt8](repeating: 0, count:Int(CC_SHA1_DIGEST_LENGTH))
        let newData = NSData.init(data: codingData)
        CC_SHA1(newData.bytes, CC_LONG(codingData.count), &digest)
        let output = NSMutableString(capacity: Int(CC_SHA1_DIGEST_LENGTH))
        for byte in digest {
            output.appendFormat("%02x", byte)
        }
        return output as String
    }
	```
	
* ### 综合封装

	MD5、SHA1、SHA224、SHA256、SHA384、SHA512   
	
	
	```
	// Secure Hash Algorithm
	enum DDYSHAType {
	   case MD5, SHA1, SHA224, SHA256, SHA384, SHA512
    	var infoTuple: (algorithm: CCHmacAlgorithm, length: Int) {
        	switch self {
        	case .MD5:      return (algorithm: CCHmacAlgorithm(kCCHmacAlgMD5),    length: Int(CC_MD5_DIGEST_LENGTH))
        	case .SHA1:     return (algorithm: CCHmacAlgorithm(kCCHmacAlgSHA1),   length: Int(CC_SHA1_DIGEST_LENGTH))
        	case .SHA224:   return (algorithm: CCHmacAlgorithm(kCCHmacAlgSHA224), length: Int(CC_SHA224_DIGEST_LENGTH))
        	case .SHA256:   return (algorithm: CCHmacAlgorithm(kCCHmacAlgSHA256), length: Int(CC_SHA256_DIGEST_LENGTH))
        	case .SHA384:   return (algorithm: CCHmacAlgorithm(kCCHmacAlgSHA384), length: Int(CC_SHA384_DIGEST_LENGTH))
        	case .SHA512:   return (algorithm: CCHmacAlgorithm(kCCHmacAlgSHA512), length: Int(CC_SHA512_DIGEST_LENGTH))
        	}
    	}
	}
	
	// MD5 SHA1 SHA256 SHA512 这4种本质都是摘要函数，不通在于长度  MD5 是 128 位，SHA1  是 160 位 ，SHA256  是 256 位
 	public static func shaCrypt(string: String?, cryptType: DDYSHAType, key: String?, lower: Bool) -> String? {
    	guard let cStr = string?.cString(using: String.Encoding.utf8) else {
    		return nil
      	}
    	// let strLen = Int(string!.lengthOfBytes(using: String.Encoding.utf8))
    	let strLen  = strlen(cStr)
    	let digLen = cryptType.infoTuple.length
    	let buffer = UnsafeMutablePointer<CUnsignedChar>.allocate(capacity: digLen)
    	let hash = NSMutableString()

    	if let cKey = key?.cString(using: String.Encoding.utf8), key != "" {
    	    let keyLen = Int(key!.lengthOfBytes(using: String.Encoding.utf8))
    	    CCHmac(cryptType.infoTuple.algorithm, cKey, keyLen, cStr, strLen, buffer)
    	} else {
    	    switch cryptType {
    	    case .MD5:      CC_MD5(cStr,    (CC_LONG)(strlen(cStr)), buffer)
    	    case .SHA1:     CC_SHA1(cStr,   (CC_LONG)(strlen(cStr)), buffer)
    	    case .SHA224:   CC_SHA224(cStr, (CC_LONG)(strlen(cStr)), buffer)
    	    case .SHA256:   CC_SHA256(cStr, (CC_LONG)(strlen(cStr)), buffer)
    	    case .SHA384:   CC_SHA384(cStr, (CC_LONG)(strlen(cStr)), buffer)
    	    case .SHA512:   CC_SHA512(cStr, (CC_LONG)(strlen(cStr)), buffer)
    	    }
    	}
    	for i in 0..<digLen {
    	    if lower {
    	    	hash.appendFormat("%02x", buffer[i])
    	    } else {
    	    	hash.appendFormat("%02X", buffer[i])
    	    }
    	}
    	free(buffer)
    	return hash as String
    }
	```
	
	对称加密:AES、AES128、DES、DES3、CAST、RC2RC4、Blowfish     
	iOS中填充规则PKCS7,加解密模式ECB(无补码,CCCrypt函数中对应的nil),字符集UTF8,输出base64(可以自己改hex)
	

	```
	// Symmetric Cryptographic Algorithm
	enum DDYSCAType {
    	case AES, AES128, DES, DES3, CAST, RC2, RC4, Blowfish
    	var infoTuple: (algorithm: CCAlgorithm, digLength: Int, keyLength: Int) {
       	switch self {
        	case .AES:     return (CCAlgorithm(kCCAlgorithmAES),       Int(kCCKeySizeAES128),      Int(kCCKeySizeAES128))
        	case .AES128:  return (CCAlgorithm(kCCAlgorithmAES128),    Int(kCCBlockSizeAES128),    Int(kCCKeySizeAES256))
        	case .DES:     return (CCAlgorithm(kCCAlgorithmDES),       Int(kCCBlockSizeDES),       Int(kCCKeySizeDES))
        	case .DES3:    return (CCAlgorithm(kCCAlgorithm3DES),      Int(kCCBlockSize3DES),      Int(kCCKeySize3DES))
        	case .CAST:    return (CCAlgorithm(kCCAlgorithmCAST),      Int(kCCBlockSizeCAST),      Int(kCCKeySizeMaxCAST))
        	case .RC2:     return (CCAlgorithm(kCCAlgorithmRC2),       Int(kCCBlockSizeRC2),       Int(kCCKeySizeMaxRC2))
        	case .RC4:     return (CCAlgorithm(kCCAlgorithmRC4),       Int(kCCBlockSizeRC2),       Int(kCCKeySizeMaxRC4))
        	case .Blowfish:return (CCAlgorithm(kCCAlgorithmBlowfish),  Int(kCCBlockSizeBlowfish),  Int(kCCKeySizeMaxBlowfish))
        	}
    	}
	}

    public static func scaCrypt(string: String?, cryptType: DDYSCAType, key: String?, encode: Bool) -> String? {

    	if string == nil {
    	    return nil
    	}
    	let strData = encode ? string!.data(using: .utf8) : Data(base64Encoded: string!)
    	// 创建数据编码后的指针
    	let dataPointer = UnsafeRawPointer((strData! as NSData).bytes)
    	// 获取转码后数据的长度
    	let dataLength = size_t(strData!.count)
    	// 将加密或解密的密钥转化为Data数据
    	guard let keyData = key?.data(using: .utf8) else {
    	    return nil
    	}
    	// 创建密钥的指针
    	let keyPointer = UnsafeRawPointer((keyData as NSData).bytes)
    	// 设置密钥的长度
    	let keyLength = cryptType.infoTuple.keyLength

    	// 创建加密或解密后的数据对象
    	let cryptData = NSMutableData(length: Int(dataLength) + cryptType.infoTuple.digLength)
    	// 获取返回数据(cryptData)的指针
    	let cryptPointer = UnsafeMutableRawPointer(mutating: cryptData!.mutableBytes)
    	// 获取接收数据的长度
    	let cryptDataLength = size_t(cryptData!.length)
    	// 加密或则解密后的数据长度
    	var cryptBytesLength:size_t = 0
    	// 是解密或者加密操作(CCOperation 是32位的)
    	let operation = encode ? CCOperation(kCCEncrypt) : CCOperation(kCCDecrypt)
    	// 算法类型
    	let algoritm: CCAlgorithm = CCAlgorithm(cryptType.infoTuple.algorithm)
    	// 设置密码的填充规则（ PKCS7 & ECB 两种填充规则）
    	let options:CCOptions = UInt32(kCCOptionPKCS7Padding) | UInt32(kCCOptionECBMode)
    	// 执行算法处理
    	let cryptStatus = CCCrypt(operation, algoritm, options, keyPointer, keyLength, nil, dataPointer, dataLength, cryptPointer, cryptDataLength, &cryptBytesLength)
    	// 结果字符串初始化
    	var resultString: String?
    	// 通过返回状态判断加密或者解密是否成功
    	if CCStatus(cryptStatus) == CCStatus(kCCSuccess) {
    	    cryptData!.length = cryptBytesLength
    	    if encode {
    	    	resultString = cryptData!.base64EncodedString(options: .lineLength64Characters)
    	    } else {
    	    	resultString = NSString(data:cryptData! as Data ,encoding:String.Encoding.utf8.rawValue) as String?
    	    }
    	}
    	return resultString
    }
	```
	
	
	CCCrypt 函数的介绍

	1、参数1： 是指定加密还是解密的枚举类型（kCCEncrypt 、kCCDecrypt）     
	2、参数2： 是指加密算法的类型。在CommonCryptor.h中提供了kCCAlgorithmAES128、kCCAlgorithmAES、kCCAlgorithmDES、kCCAlgorithm3DES、kCCAlgorithmCAST、kCCAlgorithmRC4、kCCAlgorithmRC2、kCCAlgorithmBlowfish等多种类型的加密算法    
	3、 参数3：用来设置密码的填充规则（表示在使用密钥和算法对文本进行加密时的方法）的选项，该选项可以是kCCOptionPKCS7Padding或kCCOptionECBMode两者中的任一个    
	4、参数4：密钥的数据指针    
	5、参数5： 是密钥的长度 ，必须是 24 位    
	6、参数6： 加密或者解密的偏移对象    
	7、参数7： 要解密或者解密的数据指针对象   
	8、参数8： 要解密或者解密的数据字符长度    
	9、参数9： 加密或者解密的数据指针   
	10、参数10： 接受加密或者解密的数据长度   
	11、参数11： 这是加密或者解密的数据长度   
	
	
	注意    
	在加密或者解密的结果输出的时候，要重新设置接受解密或者加密的数据对象的长度为真实长度。

	CCCrypt 的返回结果    
	1、kCCSuccess	加解密操作正常结束    
	2、kCCParamError 非法的参数值   
	3、kCCBufferTooSmall 选项设置的缓存不够大   
	4、kCCMemoryFailure 内存分配失败    
	5、kCCAlignmentError 输入大小匹配不正确    
	6、kCCDecodeError 输入数据没有正确解码或解密    
	7、kCCUnimplemented 函数没有正确执行当前的算法   

	
	
	[验证](http://tool.chacuo.net/cryptdes)     
	[参考](https://www.jianshu.com/p/cab8f6ffb082)     
	[参考](https://www.jianshu.com/p/940aaca1e3f6)    
	[RSA加密](https://www.jianshu.com/p/d2cb314d30ec)    