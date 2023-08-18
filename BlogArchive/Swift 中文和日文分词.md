Swift 内置的 CFStringTokenizer 和 NaturalLanguage 都可以实现各种常见的自然语言分词，包括不是用空格分割单词的日语和中文。

CFStringTokenizer 实现分词

```
func tokenize(text: String) -> [String] {
    let tokenize = CFStringTokenizerCreate(kCFAllocatorDefault, text as CFString?, CFRangeMake(0, text.count), kCFStringTokenizerUnitWord, CFLocaleCopyCurrent())
    CFStringTokenizerAdvanceToNextToken(tokenize)
    var range = CFStringTokenizerGetCurrentTokenRange(tokenize)
    var keyWords : [String] = []
    while range.length > 0 {
        let wRange = text.index(text.startIndex, offsetBy: range.location)..<text.index(text.startIndex, offsetBy: range.location + range.length)
        let keyWord = String(text[wRange])
        keyWords.append(keyWord)
        CFStringTokenizerAdvanceToNextToken(tokenize)
        range = CFStringTokenizerGetCurrentTokenRange(tokenize)
    }
    return keyWords
}

let words = tokenize(text: "越是隔离，我就越是怀念旅行。")
print(words)
// ["越是", "隔离", "我", "就", "越是", "怀念", "旅行"]
```

NaturalLanguage 实现
NaturalLanguage 是一个自然语言处理框架，获取自然语言的元数据，实现分词，分段落，分句子等。

```
import NaturalLanguage


func tokenize (text: String) -> [String] {
    // 设置分割粒度，.word分词，.paragraph分段落，.sentence分句，document
    let tokenizer = NLTokenizer(unit: .word)
    tokenizer.string = text
    var keyWords : [String] = []
    tokenizer.enumerateTokens(in: text.startIndex..<text.endIndex) { tokenRange, _ in
        print(text[tokenRange])
        keyWords.append(String(text[tokenRange]))
        return true
    }
    return keyWords
}

// 这里我使用日语测试，中文英文也一样的。
let text = """
その日、人類は思い出した。 奴らに支配されていた恐怖を。鳥籠に囚われていた屈辱を。
"""
let words = tokenize(text: text)
print(words)
// ["その", "日", "人類", "は", "思い出し", "た", "奴", "ら", "に", "支配", "さ", "れ", "て", "い", "た", "恐怖", "を", "鳥籠", "に", "囚われ", "て", "い", "た", "屈辱", "を"]
```


NSLinguisticTagger

```
NSString *string = self.text.text;
NSLinguisticTaggerOptions options = NSLinguisticTaggerOmitWhitespace | NSLinguisticTaggerJoinNames |NSLinguisticTaggerOmitPunctuation;
NSArray *arr = [NSLinguisticTagger availableTagSchemesForUnit:NSLinguisticTaggerUnitWord|NSLinguisticTaggerUnitDocument language:@"zh-Hans"];
NSLinguisticTagger * tagger = [[NSLinguisticTagger alloc]initWithTagSchemes:arr options:options];
tagger.string = string;


NSMutableArray *array = [[NSMutableArray alloc]init];
NSString *printStr = @"";

[tagger enumerateTagsInRange:NSMakeRange(0, string.length) scheme:NSLinguisticTagSchemeScript  options:options usingBlock:^(NSString * _Nonnull tag, NSRange tokenRange, NSRange sentenceRange, BOOL * _Nonnull stop) {
    NSString *token = [string substringWithRange:tokenRange];
    
    [array addObject:token];
    
    //[str stringByAppendingFormat:token];
    //NSLog(@"%@",array);

}];

for(int i = 0; i < [array count]; i++){
    printStr = [printStr stringByAppendingFormat:@"%@\n", [array objectAtIndex:i]];
}
```