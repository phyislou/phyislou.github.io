# HTML+CSS+JS的内容


无论何时，官方文档永远是最准确无误的参考资料，一切请务必以之为准：
* [W3C制定的网页设计标准](https://www.w3.org/standards/webdesign/)
* [MDN火狐的开发者文档（中文）](https://developer.mozilla.org/zh-CN/)


## 网页的显示

我们先用一道非常经典的题目开始：从输入URL开始，直到页面显示在浏览器中，这其中经过了哪些流程呢？

实际上的流程是这样的：

1. **域名解析**：一个完整的URL的格式是这样的
   
   `protocol :// hostname[:port] / path / [;parameters][?query]#fragment`

   以百度为例，其中：

   传输协议：`https://`

   主机地址：`www.baidu.com`

   而主机地址中的`baidu.com`即为域名，域名解析即将域名转换为对应的服务器（主机）IP的过程，依次从：浏览器缓存->本地hosts->路由器缓存->根服务器查找。

2. **数据传输**：建立TCP连接，客户端发起http请求，服务器响应请求。
   
3. **渲染网页**：构建DOM树与render树，以及获取其他资源