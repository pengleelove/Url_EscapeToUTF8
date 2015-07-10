# 关于URL中中文编码的问题

=============================

```Objective-c
/**
*  关于中文URL编码问题无法使用浏览器打开
*
*  @param void 
*
*  @return <#return value description#>
*/
NSString * urlString = @"http://search.google.com?keywords=($# it's {a*123})00!*'();:@&=+$,/?%#[]中文";
NSString * urlString1 = @"http://search.google.com?keywords=($# it's {a*123})00!*'();:@&=+$,/?%#[]";

NSLog(@"original :%@",urlString);

/**
*  可以使用系统的浏览器打开一下，如果能跳转则证明编码成功
*
*  @param void <#void description#>
*
*  @return <#return value description#>
*/
//******************************************
// 第一种测试方法，经过未编码中文的地址
//    [[UIApplication sharedApplication]openURL:[NSURL URLWithString:urlString]];

//第二种测试方法，经过UTF8中文编码的URL地址
urlString = [urlString encodeUrlWithUTF8_CN:urlString];

NSLog(@"encode1 = %@",urlString);

//    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
//第三中方法，URL包含特殊字符的地址使用下面这个方法

urlString1 = [urlString1 encodeToPercentEscapeString:urlString1];

NSLog(@"encode2 = %@",urlString1);

//解码
NSString * decodeStr = [urlString1 decodeFromPercentEscapeString:urlString1];
NSLog(@"decode =%@",decodeStr);
```
```//打印结果
original :http://search.google.com?keywords=($# it's {a*123})00!*'();:@&=+$,/?%#[]中文
这里打印结果可以看出，中文正常编码了，特殊符号并没有改变
encode1 = http://search.google.com?keywords=($%23%20it's%20%7Ba*123%7D)00!*'();:@&=+$,/?%25%23%5B%5D%E4%B8%AD%E6%96%87
encode2 = http%3A%2F%2Fsearch.google.com%3Fkeywords%3D%28%24%23%20it%27s%20%7Ba%2A123%7D%2900%21%2A%27%28%29%3B%3A%40%26%3D%2B%24%2C%2F%3F%25%23%5B%5D
decode =http://search.google.com?keywords=($# it's {a*123})00!*'();:@&=+$,/?%#[]
****
```