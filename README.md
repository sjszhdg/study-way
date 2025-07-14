
另一个支线说过了burpsuite pro 的破解，这次用的proxifier普通的也够用但是还是给大家提供一个key

proxifiter还是正常程序下载一下

我也在上面放了安装包

官网https://www.proxifier.com/

<img width="937" height="489" alt="image" src="https://github.com/user-attachments/assets/e81e3dae-c0e8-4c65-b760-1150b472d787" />

选下面的window的

然后就正常下载

到选择的这一步

<img width="570" height="893" alt="image" src="https://github.com/user-attachments/assets/69ab23d8-a6ac-43a8-a8f7-486852813fb7" />

```
NY8VX-Z2NH2-TFXWY-IL5YC-GARRM
```
把上面的key输入进去然后继续就可以了
还是非常简单的

现在还开始抓包

# 第一步 #
打开proxifier

点击

<img width="765" height="488" alt="2b16f46e67435a2236b467ce4f70178" src="https://github.com/user-attachments/assets/324d7c6d-2500-4b76-aacb-8a992c5df049" />

打开后是这样的，我这里已经添加了

<img width="872" height="512" alt="7423c53a975d2815dca85e36e136816" src="https://github.com/user-attachments/assets/30e9064c-a6f9-4abb-8298-ba7c9098af34" />

添加这个就行了

<img width="1747" height="1013" alt="image" src="https://github.com/user-attachments/assets/30edce10-44a2-497a-ae5b-d70d20e0f9b5" />

这个proxifier的ip是和burpsuite里的ip和端口一样的是为了联动

<img width="1895" height="1051" alt="image" src="https://github.com/user-attachments/assets/d2907079-01e2-486e-8510-733705efe8e3" />

然后点击

<img width="664" height="501" alt="6d3bfaf19d3f6feafb7570bb40f46df" src="https://github.com/user-attachments/assets/eede11c9-536f-4d1b-8a05-5198f8e9ad85" />

因为是为了抓微信小程序所以要添加wechat

<img width="1080" height="628" alt="image" src="https://github.com/user-attachments/assets/d27caf31-089a-4003-834d-917aa8b104ed" />

图中第二步要找到wechatappex的路径

<img width="676" height="505" alt="29580edfe61ba068a3a25583e9fec31" src="https://github.com/user-attachments/assets/a68ef5d7-0fe4-4719-8d8f-17ebd0fed389" />

打开任务管理器，点开wechatappex右击打开文件所在位置，然后复制路径，在上图第二步点开，搜索复制进去找到wechatappex所在位置然后直接打开就行了

<img width="581" height="438" alt="fe88cf288c5e7f4f16931713394cda8" src="https://github.com/user-attachments/assets/82a47063-aa33-41c2-b9ea-410115005ce1" />

<img width="627" height="351" alt="2979cb6384d6cdc83a9adf2c9af22aa" src="https://github.com/user-attachments/assets/feab3ead-bf07-407a-87bd-004112dc42d2" />

然后点击OK就行了

# 第二步 #
打开burpsuite，

点击微信小程序页面

这里查看抓包，prixifier界面说明有包

<img width="1737" height="1001" alt="image" src="https://github.com/user-attachments/assets/11de6a5d-5b26-468a-a971-621755f37c60" />

随机点开一个小程序，在查看burpsuite，出现get，客户端从服务端获取资源

<img width="1870" height="1042" alt="image" src="https://github.com/user-attachments/assets/722ae5ec-7c0a-4c08-81f7-1865614a1b5f" />

图中几个参数
post：http协议的请求方法之一，用于向服务器提交数据常用来传输表单信息，文件内容，让服务器根据这个执行操作，比如指定URL。

GET：主要用于从服务器获取指定资源，获取静态资源（html网页，css样式文件，JavaScript脚本，图片），文本文件等，也能获取动态数据，向服务器查询特定数据，比如电商网站按关键词搜索商品 https://example.com/search?keyword=手机  ，通过 URL 里的查询参数（ ?  后部分 ），让服务器返回匹配商品列表等结果，不过一般用于获取数据，不适合传敏感信息（参数暴露在 URL ）。还有API 接口数据拉取，很多开放 API 用 GET 提供数据，比如天气 API  https://api.example.com/weather?city=Beijing  ，获取对应城市天气信息返回给调用方 。




