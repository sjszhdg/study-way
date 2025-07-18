# xss # 
跨站脚本攻击XSS(Cross Site Scripting)，为了不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS。恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。XSS攻击针对的是用户层面的攻击！

# xss分类 #
1.反射性xss
反射型XSS 是非持久性、参数型的跨站脚本。反射型XSS 的JS 代码在Web 应用的参数（变量）中，如搜索框的反射型XSS。在搜索框中，提交PoC[scriptalert(/xss/)/script]，点击搜索，即可触发反射型XSS。 注意到，我们提交的poc 会出现在search.php 页面的keywords 参数中。

<img width="953" height="692" alt="image" src="https://github.com/user-attachments/assets/ea04785f-c40e-45f1-80bc-344df4d13fb9" />


2.存储型xss 

存储型XSS 是持久性跨站脚本。持久性体现在XSS 代码不是在某个参数（变量）中，而是写进数据库或文件等可以永久保存数据的介质中。存储型XSS 通常发生在留言板等地方。我们在留言板位置留言，将恶意代码写进数据库中。此时，我们只完成了第一步，将恶意代码写入数据库。因为XSS 使用的JS 代码，JS 代码的运行环境是浏览器，所以需要浏览器从服务器载入恶意的XSS 代码，才能真正触发XSS。此时，需要我们模拟网站后台管理员的身份，查看留言。


<img width="966" height="700" alt="image" src="https://github.com/user-attachments/assets/0b117bbf-584e-4747-a475-4428f71da953" />

3.DOM的xss 

DOM XSS 比较特殊。owasp 关于DOM 型号XSS 的定义是基于DOM 的XSS 是一种XSS 攻击，其中攻击的payload由于修改受害者浏览器页面的DOM 树而执行的。其特殊的地方就是payload 在浏览器本地修改DOM 树而执行， 并不会传到服务器上，这也就使得DOM XSS 比较难以检测。



# xss-labs #
下载xss-labs

将他转移到xp的www目录下
<img width="1692" height="853" alt="image" src="https://github.com/user-attachments/assets/dd90be7f-0eab-4a28-8ccb-b249888747f2" />

然后

在xp上创建网站
<img width="727" height="704" alt="image" src="https://github.com/user-attachments/assets/e06ced40-72af-41e5-82bb-d527af2aec8e" />

需要访问时打开apchi和mysql，访问
```
http://127.0.0.1/xss-labs
```
# level-1 #

<img width="941" height="674" alt="image" src="https://github.com/user-attachments/assets/db280fb2-d98a-425d-aa1c-aabf9a51f543" />

网页源代码

<img width="949" height="559" alt="image" src="https://github.com/user-attachments/assets/7a0a4908-55da-4879-a95e-3d09eced1f87" />


这里能看到网页源码里是有个alert函数的，只要触发这个函数就可以到下一关

这里输入
```
< svg onclick="alert(1)" >
SVG（可缩放矢量图形）的根标签，用于定义矢量图形。
onclick="alert(1)"事件属性，意思是当用户点击这个 SVG 元素时，会执行alert(1)这个 JavaScript 代码，弹出一个显示数字 “1” 的提示框。
```

<img width="934" height="624" alt="image" src="https://github.com/user-attachments/assets/351fba5f-81d1-45a5-b6a4-9100b1e7cc99" />

# level-2 #

<img width="942" height="539" alt="image" src="https://github.com/user-attachments/assets/9787c289-f9d3-408e-b5e7-f3be00e1ec96" />

<img width="939" height="619" alt="image" src="https://github.com/user-attachments/assets/6933d13f-5c29-479d-a039-3c26a74498eb" />

第二关是有一个input输入框的输入内容就是test，我们要做的就是payload逃出这个引号内容，然后调用这个alert函数
```
" onclick="alert(1)
是将图中test替换为以上内容，后再input里用onclick
onclick="alert(1)" 是 HTML 中的一个内联事件处理属性，用于在元素被点击时触发 JavaScript 代码
```
我这里用onclick

<img width="949" height="711" alt="image" src="https://github.com/user-attachments/assets/217b02ab-1e28-4775-9ebf-f5f6246a463a" />

所以我们这里点击input的输入框就会调用alert函数进入下一关

<img width="939" height="302" alt="image" src="https://github.com/user-attachments/assets/9b73f6d6-80dd-4aeb-9347-dc1be281418a" />


# level-3 #
<img width="950" height="683" alt="image" src="https://github.com/user-attachments/assets/61e43fc5-10e3-4a1a-a990-4dfc24c31a48" />

<img width="940" height="882" alt="image" src="https://github.com/user-attachments/assets/7c03545b-100a-4b8d-82d1-a754d14087ea" />

这个和上一关差不多，先用上一关方法试试
```
' onfocus='alert(1)' autofocus='
onfocus 是 HTML 中的事件属性，当元素获得焦点时会触发后面的脚本；
autofocus 则是让元素自动获得焦点的属性。
两者结合后，页面加载时元素会因 autofocus 自动聚焦，进而触发 onfocus 中的脚本执行，这是典型的利用 HTML 属性注入脚本的 XSS 攻击模式，可能被用于窃取用户信息、篡改页面内容等恶意行为。
```
成功了，而且不需要点击输入框。

<img width="958" height="289" alt="image" src="https://github.com/user-attachments/assets/2b5cf093-2adf-4a8a-9ebd-509e3dfb22a1" />

# level-4 #
<img width="939" height="614" alt="image" src="https://github.com/user-attachments/assets/df8befc7-1855-41a5-969a-795f6183f448" />

<img width="944" height="643" alt="image" src="https://github.com/user-attachments/assets/7784f8eb-8066-4fa0-9d85-e9c271d1dc13" />
这一关也是有个输入框，继续上一关方法试试，感觉这次不太行了
```
" onfocus="alert(1)" autofocus="
```
<img width="952" height="725" alt="image" src="https://github.com/user-attachments/assets/001f8cc6-123c-422b-8279-3bc260dc4acb" />

# level-5 #

<img width="954" height="719" alt="image" src="https://github.com/user-attachments/assets/15e70b9c-195f-48aa-84c8-7d9de0be19b3" />

<img width="950" height="775" alt="image" src="https://github.com/user-attachments/assets/7e7cef4d-e7ea-4544-aef8-29e36d328568" />

这个关也是差不多

还用那个方法试试
<img width="958" height="102" alt="image" src="https://github.com/user-attachments/assets/d4691dfd-2c58-4e00-963a-4f6810da94a4" />

这次不行了

他在我们o后面加了个_,把我们的方法破坏了

我们换个方法
```
"><a href=javascript：alert(1)>下一关</a>
 <a href=javascript：alert(1)> 是创建了一个超链接，其href属性使用了javascript:伪协议，当用户点击该链接时，会执行alert(1)这个 JavaScript 代码，弹出内容为1的对话框。
```
<img width="970" height="783" alt="image" src="https://github.com/user-attachments/assets/00447793-a288-455c-9ae0-3581dc1cdbf0" />


# level-6 #

<img width="960" height="608" alt="image" src="https://github.com/user-attachments/assets/b64909e2-fe26-467d-a23b-c347964f5bc6" />

<img width="888" height="179" alt="image" src="https://github.com/user-attachments/assets/3572659c-590d-45d5-98b0-47705091fe5f" />

依旧输入框试试上一关方法href被过滤了

<img width="945" height="95" alt="image" src="https://github.com/user-attachments/assets/a7d086a8-6aba-4270-8b14-049639e2700e" />

不行了在我们r后面加_了

onfocus+autofocus也不行也被过滤
<img width="946" height="775" alt="image" src="https://github.com/user-attachments/assets/d183aa4c-854f-4f7f-9e07-bc8dc7e1c683" />

我们试试将href中的re换成大写
```
"><a hREf=javascript:alert()>下一关</a>

```
<img width="943" height="770" alt="image" src="https://github.com/user-attachments/assets/f39a9605-4159-4e9d-910a-0a763f156a78" />

# level-7 #
<img width="820" height="750" alt="image" src="https://github.com/user-attachments/assets/85357c2c-d04d-4506-9d66-6ab2f58ec4b2" />

<img width="954" height="863" alt="image" src="https://github.com/user-attachments/assets/43c95124-4085-42cc-8922-2cd3d419ea5b" />

先试试上一关方法
```
"><a href=javascript:alert()>下一关</a>
```
它吧我们的href和script给删掉了
<img width="935" height="207" alt="image" src="https://github.com/user-attachments/assets/ac4956f4-77c6-4c40-b555-f9c7936b35e5" />

```
"><a hrhrefef=javasscriptcript:alert()>下一关</a>
```
把被删了的字段放进原本的字段里，它让刚开始检查不出来，把第一个href和script删掉时，我们直接形成了第二个href和script。成功过关
<img width="954" height="773" alt="image" src="https://github.com/user-attachments/assets/33b8ee53-e0ea-45c3-9c59-77a04362f01a" />

# level-8 #


<img width="947" height="741" alt="image" src="https://github.com/user-attachments/assets/312ceeb3-1d9c-4dca-bfc0-f672fb9c4c07" />
<img width="957" height="613" alt="image" src="https://github.com/user-attachments/assets/559166a5-3ecc-4fd3-b4d3-48cb54779970" />

我们看到这里是有一个链接的我们就把它变成我们的链接就行了，接下来试试
<img width="957" height="204" alt="image" src="https://github.com/user-attachments/assets/bc36abf2-4cb8-471f-b6cb-3abda16814be" />

它在我们script的中间加了个_ ,那我们就把r和i换成编码
```
javasc&#114;&#105;&#10;pt:alert()
```
<img width="954" height="637" alt="image" src="https://github.com/user-attachments/assets/351b4abd-6917-4f53-9721-1459c8968699" />

# level-9 # 
<img width="950" height="857" alt="image" src="https://github.com/user-attachments/assets/3957ea7a-8081-4d61-9b26-7082aa3e7470" />

<img width="971" height="452" alt="image" src="https://github.com/user-attachments/assets/395dec1b-e209-40ac-891e-e6d6c9f38934" />
这关和上一关类似，应该也过滤了，先试试看吧

我们这里添加了JavaScrip:alert(),他显示我们不合法，就是链接不规范。

<img width="964" height="219" alt="image" src="https://github.com/user-attachments/assets/dbe65ff4-32af-442c-871c-d542db9c5608" />

果然是没加http://所以不规范
<img width="945" height="165" alt="image" src="https://github.com/user-attachments/assets/23296948-b70e-43de-bae2-db6bf2c13a30" />

既然还是加了_，还是上面的方法，换成编码，因为要有http://，只有它失效才能执行JavaScript：alert（）的弹窗效果，但是不能删了他，要利用它过掉过滤。

我们这里用/*  */把他注释掉
```
javasc&#114;&#105;&#10;pt:alert()  /* http:// */
```

<img width="898" height="906" alt="image" src="https://github.com/user-attachments/assets/21e4996e-ac3e-411e-9a8b-9cbe7eee42cf" />

# level_10 #

<img width="929" height="620" alt="image" src="https://github.com/user-attachments/assets/4865c7bc-c5b3-45d4-8a23-e1ef72c2af31" />
<img width="942" height="688" alt="image" src="https://github.com/user-attachments/assets/96593925-08f5-40bc-9cf1-3b7dad318e08" />

我们直接传参上面将well done换成
```
<a  href=javascript:alert()>下一关</a>
```
<img width="891" height="177" alt="image" src="https://github.com/user-attachments/assets/b84b22c7-ef5f-4046-a6f8-8f3421b1c957" />

它把我们的<>都实体化了

注意到源码里有三行隐藏input一个个试试看，把隐藏属性换掉。

用type=“text”隐藏的框就显示了

<img width="945" height="564" alt="image" src="https://github.com/user-attachments/assets/0db25e0a-1502-41b8-b82c-ef30b0c0caac" />
这里只有t_sort显示了所以就利用这个操作

<img width="924" height="587" alt="image" src="https://github.com/user-attachments/assets/7243a741-0073-43db-8a81-09e54fb8d1c5" />

```
?t_sort=" onfocus="alert()" autofocus="" type="text
```
<img width="1012" height="450" alt="image" src="https://github.com/user-attachments/assets/63a0efc3-17fe-47b3-88ee-1a03f15afd69" />

# level-11 #

<img width="960" height="693" alt="image" src="https://github.com/user-attachments/assets/790d0c3e-6645-4539-9897-5f1b46d5abe9" />


<img width="942" height="875" alt="image" src="https://github.com/user-attachments/assets/f968fb5e-a01f-4990-9712-2a28b950cefe" />

这里个第10关一样还是隐藏的input用上一关代码试试

没有变化

<img width="919" height="899" alt="image" src="https://github.com/user-attachments/assets/72dfcba9-5075-48c8-a263-326648a48951" />

但是我们可以看到sort是收到参数的，只是被编码了


<img width="951" height="527" alt="image" src="https://github.com/user-attachments/assets/96723f4a-fc72-4b39-90ae-a1ce46c59cee" />

这里实在想不出有什么绕过的方法，所以查看源文件代码看看是怎么回事


<img width="939" height="716" alt="image" src="https://github.com/user-attachments/assets/8ba60386-8673-46c2-9362-f1bed8079154" />


```
ini_set("display_errors", 0);
关闭 PHP 错误显示功能

$str = $_GET["keyword"];
从 URL 参数中获取名为 "keyword" 的值

$str00 = $_GET["t_sort"];
从 URL 参数中获取名为 "t_sort" 的值

$str11=$_SERVER['HTTP_REFERER'];
获取 HTTP 请求的来源 URL（即用户从哪个页面链接过来的）

$str22=str_replace(">","",$str11);
$str33=str_replace("<","",$str11);
从来源 URL 中移除<和>

echo "<h2 align=center>没有找到和".htmlspecialchars($str)."相关的结果.</h2>".'<center>

输出 HTML 标题，显示 "没有找到和 [关键词] 相关的结果"
使用htmlspecialchars对关键词进行编码，防止 XSS 攻击

htmlspecialchars($str00);
通过 htmlspecialchars 函数对变量 $str00 进行处理，把特殊字符（如 <、>、& 等 ）转换为 HTML 实体，

 $str33 
 值作为隐藏输入框的 value ，这里没有做转义处理（对比第一行 ）
```

原来在服务器端还将请求头中的referer头的值赋给了str11这个变量，在将变量值中的<、>删除之后就 会插入到t_ref这个标签的value属性值中。

因为$str11是获取当前请求的来源页面 URL，然后给$str22,它把>换成空,在给$str33，它把<换成空，然后变量 $str33 的值作为隐藏输入框的 value

然后$str11写了http_referer,所以应该是传给ref的我们先试试


<img width="962" height="401" alt="image" src="https://github.com/user-attachments/assets/1b43d192-3fb3-4e8c-bbb2-eefb8235c47b" />

这里可以看到referer的头部被传给了ref，到这里就和简单了直接在这里写payload

在referer头部写上

```
referer: " onfocuse="alert()" autofocuse="" type="text
```

<img width="969" height="515" alt="image" src="https://github.com/user-attachments/assets/bc628a6d-1e4d-4470-b0ce-0f2ee4c37ab8" />

<img width="900" height="853" alt="image" src="https://github.com/user-attachments/assets/d9e42500-f753-4ce4-9ac6-4a9eea6e0aaa" />


# level-12 #
<img width="933" height="901" alt="image" src="https://github.com/user-attachments/assets/a4945cfc-150e-47cb-859a-835b1539e69c" />

查看网页源码

<img width="1209" height="662" alt="image" src="https://github.com/user-attachments/assets/24c228b3-878f-491d-8e21-4e97bf792bcb" />

这里和11关差不多我们来用命令测试一下
```
t_link="%20type="text%20&t_history="%20type="text%20&t_sort="%20type="text%20&t_ua="%20type="text
```
<img width="1221" height="303" alt="image" src="https://github.com/user-attachments/assets/8691d808-c54b-44f3-a874-3f3ff6f3de99" />

t_sort的命令是进去了的，只不过和11关一样被实体化了，我们来看一下源代码。

<img width="1378" height="1084" alt="image" src="https://github.com/user-attachments/assets/b34573b7-0a91-4c91-90c8-1c8be9a5d956" />

这里和11关一样的只不过是第四个的元素名字变量一下和http变了一下，我们直接用burpsuite抓包，修改一下试试。

<img width="1476" height="725" alt="image" src="https://github.com/user-attachments/assets/8f954206-761f-47f6-af60-ba5c756327bd" />

这里也是成功了
<img width="1014" height="894" alt="image" src="https://github.com/user-attachments/assets/66a12cbe-d598-4c2a-979e-ae0a8b01304c" />






# xss.PwnFunction #

准备访问xss.pwnfunction.com时发现访问不了，开vpn也不行
这是原作者的项目地址
https://github.com/PwnFunction/xss.pwnfunction.com?tab=readme-ov-file

然后可以将他部署在ubantu上

我的是ubantu24.04


直接用proxy代理把项目拉下来
```
proxychains git clone https://github.com/PwnFunction/xss.pwnfunction.com.git
```

进入吗命令行，先下载hugo服务
```
apt install hugo
```
<img width="1260" height="643" alt="image" src="https://github.com/user-attachments/assets/1f80cbac-57ed-4772-af16-2c078f733e06" />

进入hugo目录
```
cd xss.pwnfunction.com-main/hugo··
```

将可以访问的ip改掉，这里0.0.0.0 指的是所有ip都可以访问
```
hugo server --bind 0.0.0.0 --baseURL http://192.168.137.135/
```

<img width="1185" height="536" alt="image" src="https://github.com/user-attachments/assets/e65a593c-a145-4240-92d7-6086c0fed951" />


然后就可以在我们的浏览器上访问了
```
http://<ubantu的ip>:1313
```

<img width="2247" height="1309" alt="image" src="https://github.com/user-attachments/assets/aa185bfc-c9da-45c8-af73-ae2ba0494775" />


# 原型链污染 #

原型和原型链都是来源于对象而服务于对象的概念，所以我们要先明确一点：
JavaScript中一切引用类型都是对象，对象就是属性的集合。
Array类型、Function类型、Object类型、Date类型、RegExp类型等都是引用类型。
也就是说 数组是对象、函数是对象、正则是对象、对象还是对象。

原型和原型链到底是什么呢，我学的时候，也懵逼了好长时间。
定义：每个 JavaScript 函数（构造函数）都有一个 prototype 属性，指向一个对象（原型对象）。
作用就是为了共享属性和方法，所有通过该构造函数创建的实例都能访问原型上的属性和方法。

这里的原型对象是什么，实例对象有是什么？


我们看一个例子
```
function Person(name) {
  this.name = name; // 实例属性（每个实例独立）
}

const person1 = new Person("Alice"); // person1 是实例对象
const person2 = new Person("Bob");   // person2 是另一个实例对象
```
实例对象就是这么简单，那原型对象呢
```
function Animal(name) {
  this.name = name;
}
Animal.prototype.color = 'white';

var cat1 = new Animal('大毛');
var cat2 = new Animal('二毛');

cat1.color // 'white'
cat2.color // 'white'
```
这个例子里cat1和cat2是实例对象，图中的animal的prototype就是cat1和cat2的原型对象
JavaScript 规定，每个函数都有一个prototype属性，指向一个对象。就是object

```
cat1.color--->Animal.prototype---->aaaaaaa.prototype------>xxxxx.------->Object.prototype---->null
```
如果一层层地上溯，所有对象的原型最终都可以上溯到`Object.prototype`，即`Object`构造函数的`prototype`属性。也就是说，所有对象都继承了`Object.prototype`的属性。这就是所有对象都有`valueOf`和`toString`方法的原因，因为这是从`Object.prototype`继承的。
那么，`Object.prototype`对象有没有它的原型呢？回答是`Object.prototype`的原型是`null`。`null`没有任何属性和方法，也没有自己的原型。因此，原型链的尽头就是`null`。

# prototype和__proto__和constructor 有什么区别呢 #

# 1，prototype #
每个函数（Function）默认拥有的属性，指向一个对象（原型对象）。

当函数被用作构造函数（通过 new 调用）时，新创建的对象会继承原型对象的属性和方法。

后面将他和其他两个结合起来说

# 2.__proto__ #
每个对象（包括数组、函数）默认拥有的属性，指向创建它的构造函数的 prototype 对象。

JavaScript 通过 __proto__ 实现原型链查找，当访问一个对象的属性时，引擎会先查找对象本身，再通过 __proto__ 逐级向上查找。

```
const person = new Person("Alice");
console.log(person.__proto__ === Person.prototype); // true
```

# 3.constructor #
每个原型对象默认拥有的属性，指向创建该原型的构造函数
用于标识对象的 “类型”，或通过它创建新实例。

`prototype`对象有一个`constructor`属性，默认指向`prototype`对象所在的构造函数。
```
function P() {}
P.prototype.constructor === P // true
```
由于`constructor`属性定义在`prototype`对象上面，意味着可以被所有实例对象继承。
```
function P大() {}
var p小 = new P大();

//应该有这个属性吗
p小.constructor === P大 // true

p小.constructor === P大.prototype.constructor // true
```
这个还是非常清晰的

上面代码中，`p`是构造函数`P`的实例对象，但是`p`自身没有`constructor`属性，该属性其实是读取原型链上面的`P.prototype.constructor`属性。

`constructor`属性的作用是，可以得知某个实例对象，到底是哪一个构造函数产生的。


`constructor`属性表示原型对象与构造函数之间的关联关系，如果修改了原型对象，一般会同时修改`constructor`属性，防止引用的时候出错。

```
function Person(name) {
  this.name = name;
}

Person.prototype.constructor === Person // true

Person.prototype = {
  method: function () {}
};

Person.prototype.constructor === Person // false
Person.prototype.constructor === Object // true
```
上面代码中，构造函数`Person`的原型对象改掉了，但是没有修改`constructor`属性，导致这个属性不再指向`Person`。由于`Person`的新原型是一个普通对象，而普通对象的`constructor`属性指向`Object`构造函数，导致`Person.prototype.constructor`变成了`Object`。
应该能看拿出来哪里修改了原型对象吧，Person.prototype = { method 这里修改了（我就不写玩了OVO），这个看懂了后面都清晰的。

所以，修改原型对象时，一般要同时修改`constructor`属性的指向。
```
C.prototype.method1 = function (...) { ... };
```
这个方法比较好，建议用这个，这个比较清晰，易于理解

```
//定义基类animal
function Animal(type) {
  this.type = type;
}
Animal.prototype.move = function() {
  console.log(`${this.type} is moving`);
};
//定义子类dog
function Dog(name) {
  this.name = name;
  Animal.call(this, "Dog"); // 继承属性
}
// 设置 Dog 的原型为 Animal 的实例
Dog.prototype = Object.create(Animal.prototype);
// 修复 constructor
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() {
  console.log("Woof!");
};

const dog = new Dog("Buddy");
dog.move(); // 输出: Dog is moving（通过原型链访问 Animal.prototype）
dog.bark(); // 输出: Woof!
console.log(dog.constructor === Dog); // true
```

Animal 构造函数：创建对象时设置 type 属性（如 Dog、Cat）。

Animal.prototype.move：所有 Animal 实例共享的方法，通过原型链访问。

Dog 构造函数：
    设置 name 属性（如 "Buddy"）。
    通过 Animal.call(this, "Dog") 调用父类构造函数，将 type 设置为 "Dog"。
    效果：每个 Dog 实例都有 name 和 type 属性。

Object.create(Animal.prototype):
    创建一个新对象，其 __proto__ 指向 Animal.prototype。
    将该对象赋值给 Dog.prototype，使 Dog 实例可以访问 Animal.prototype 的方法（如 move()）。
    原型链结构：
    dog.__proto__ ---> Dog.prototype ---> Animal.prototype ---> Object.prototype。
