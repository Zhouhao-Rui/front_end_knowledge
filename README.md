

# HTML和浏览器

## 1. Http和Https

https的ssl加密是在传输层进行实现的。

Http: hyper text transportation protocol, 是客户端和服务器端请求和应答的标准(TCP)，从WWW浏览器传输超文本到本地的浏览器。

Https: 在Http下加入了SSL层，Https的安全基础是SSL，加密的详细内容需要使用SSL

- https使用了SSL protocol对http协议传输的数据进行了加密，构建了可加密传输和身份认证(identification authentication) 的网络协议，安全系数更高

- Https协议需要用CA证书，费用较高

- Http传输端口是80，https传输端口是443

Https工作流程：

- http url访问服务器，要求web服务器建立SSL连接
- web服务器收到请求后，会将网站的证书（public key），传输给客户端
- client和server开始协商SSL的安全等级
- client建立会话密钥(session key)，通过公钥来加密，传送给server
- server通过private key解密

Https 降低了man-in-the-middle attack

Https的缺点：

- Handshaking is time-consuming 
- SSL CA costs more money
- one IP to one SSL CA, not flexible.

## 2. TCP 和 UDP

### 2.1 TCP handshaking

- client sends request to the server with Syn=1, and servers gets the request data. This shows that the server can receive data successfully.
- server sends response to the client with Syn=1 and acknowledgement, and the client receive the response. This shows that the server can receive and send data normally. At the same time, the clinet can also successfully send and receive data.
- Client again sends the data to the server. The server can confirm that the response has been received from the client successfully.

### 2.2 TCP和UDP

- Once TCP is established, the data can be transmitted in bi-directions. TCP has built-in system to check if the data is transmitted successfully. It's needs higher bandwidth, and it's used for web page, image...
- UDP is connectionless internet protocol, data is continuously sending from the sender to the receiver, no matter whether the receiver has got the data. (Broadcast, long-connection)

DIfference:

- TCP is connection-oriented and more reliable, UDP is connectionless.
- TCP make sure the low data loss and high data accuracy during transmission, UDP focuses on transmitting huge amount of data continuously.
- TCP is for data stream, but the UCP is for datagram, and won't slow down data sending frequency with network congestion. It is easy for UDP to get data loss.
- TCP is one-to-one, UDP is broadcasting.

## 3. WebSocket

Websocket 是HTML5中的协议，支持long connection，http不支持long connection。

html 1.1中使用了**keep-alive**，可以将多个request合并为一个request，并且接受response

Websocket比普通的http多了upgrade和connection两个属性

- upgrade: Websocket
- connection: upgrade

## 4. Http请求方式和请求头

### 4.1 HTTP Header

HTTP header carrys the case-sensitive addtional information with HTTP request and response. 

general header: used in the request and response, but its content is not related to the data transmission.

request header, response header

Entity header: contains the information about the body of the resource.

payload header: contains representation-independent information about payload data.

### 4.2 Http request method

Get: should only retrive data

Head: Like get method, but only request the header

Post:  submits an entity to the specified resource, often causing a change in state or side effects on the server.

Put: replaces all current representations of the target resource with the request payload.

**The difference between put and post is that put is idempotent, calling it once or several time it has the same effect.**

Put 用相同的payload多次更新一个资源，是不会

POST所对应的URI并非创建的资源本身，而是资源的接收者。比如：POST [http://www.forum.com/articles](https://link.jianshu.com?t=http%3A%2F%2Fwww.forum.com%2Farticles)的语义是在[http://www.forum.com/articles](https://link.jianshu.com?t=http%3A%2F%2Fwww.forum.com%2Farticles)下创建一篇帖子，HTTP响应中应包含帖子的创建状态以及帖子的URI。两次相同的POST请求会在服务器端创建两份资源，它们具有不同的URI；所以，POST方法不具备幂等性。而PUT所对应的URI是要创建或更新的资源本身。比如：PUT [http://www.forum/articles/4231](https://link.jianshu.com?t=http%3A%2F%2Fwww.forum%2Farticles%2F4231)的语义是创建或更新ID为4231的帖子。对同一URI进行多次PUT的副作用和一次PUT是相同的；因此，PUT方法具有幂等性。

Delete: Delete the corresponding resource.

Options: Ask for the communication options

Connect:  establishes a tunnel to the server identified by the target resource.

Patch: applies partial modifications to a resource.

### 4.3 HTTP status code

200: request successfully

201: POST or PUT, successfully created resource

204: No content, only contains the header

300: The request has more than one responses, user needs to choose one from them

301: The URL has been replaced permanently. 

304: The response has not been modified, user can use the cache version of the response.

400: Bad request, has syntax error

401: Unauthorized, user may need api key or token.

403: Forbidden, has no priority to access the resource.

404: Not found

405: Method not allowed, like POST or PUT

500: Internal Internet server Error

502: This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response. (Nginx proxy server)

503: service unaviable

504: gateway timeout

## 5. BOM

Bom is Browser Object Model, which is Window. Window is the global object in the browser.

- location
  - location.href
  - location.search: search one url
  - location.hash: return the string after #
  - location.pathname
  - location.hostname
  - location.reload
  - Location.replace
- history
  - history.go()
  - histroty.back()
  - history.forward()
- navigator
  - navigator.cookieEnabled



# 网页性能优化(optimization)

# CSS

## 1. head

head中一般存放metadata

- title
- meta charset 字符编码设置 utf-8 
- Style: 样式
- link: 链接css文件
- script

## 2. SEO(search engine optimization) and h element

SEO 在搜索引擎的时候会优先爬h1元素，所以一个网站最好只有一个h1元素

## 3. element

Code: 将其中的元素的font-family改变

br：换行符

hr：水平分割线

Pre: 对空行和空格进行控制(space and empty line)

&nbsp:空格 &gt:大于 &lt:小于

span：用于区分普通的文本和特殊的文本

Div: 块级元素，可以将同一类的元素包裹分类

img：src，alt，width，height（如果只设置了width或者height，会自动拉伸）

**a: href ｜ target｜ anchor（锚点）｜fake href #(是指向了页面的顶部) javascript:（可以写javascript代码）

- blank：打开一个新的标签页
- parent：在浏览器的标签页打开
- top：iframe嵌套时，会在顶层的iframe打开
- name：指定一个iframe打开

Iframe: 可以放入网页nested browser content（src）

**base（在header中）：可以给a元素指定一个base url和target方式

```html
<base href="www.google.com" target="_blank">
```

## 4. Css

指定编码 @charset “utf-8”

导入外联 @import(url)

选择器（selector）：

- universal selector
- type selector
- class seletor
- id selector
- attribute selector：[title] [title="hello"] [title*='hello'] (通配)
- combinations
- pseudo classes
- pseudo elements
- descendant combinator: 
  - div span 间接选择器
- child combinator
  - Div > span 必须是div的子类，直接选择
  - 但如果element的顺序反了，比如p里面包含div，则不能识别
- adjacent sibling combinator
  - div+p (只选择相邻的那个p)
- All sibling combinator
  - Div ~ p
- intersection combinator
  - Div.title
- union combinator
  - .title, #content, 

attributes:

- color: foreground color

- Background-color

- width

- Height

- Outline: 和border一样是轮廓，但是outline不占宽度

- Text-decoration: 给文本设置装饰线

  - underline
  - overline
  - line-through
  - none

- Letter-spacing: Add horizontal spacing behavior between letters

- Word-spacing: Add horizontal spacing behavior between words

- Text-transform: 字符转换，capitalize（首字母大写）,uppercase, lowercase

- Text-indent: 首字母缩进(2em)

- Text-align: left, right, center,justify左右两边间距等分到中间，最后一行没效果（图片，文字都可以排版）

- Font-size: em和百分比，根据父级的font-size进行计算，1em=原来的font-size

- Font-family: 设置一个或者多个字体

- Font-weight: 100-900,normal:400 bold:700

- Font-style: i和em标签都会使用到italic

  - italic：如果font family不支持italic，那么无效
  - oblique：文本倾斜显示

- font-variant：small-caps 小型大写字母

- Line-height: 设置文本的最小行高

  - 行高：两行文字baseline之间的距离，对齐x元素的底部，相当于文字高度+行距
  - 行距：文字空白部分的距离，上下等分

  **所以line-height如果等于div的height时，等于上下垂直居中**

- font：{style variant weight} font-size/line-height font-family(不可以省略)

### pseudo class

:link, :active, :hover, :target(锚点), :enabled :disabled :checked

dynamic pseudo class:

- A:link 未访问的链接 a:visited 访问过的链接
- a:hover a:active
- :focus

链接 选中效果不消失，因为已经在了缓存中

structual pseduo class

- :nth-child(1)
- :nth-child(2n)
- :nth-child(2n-1)
- Nth-of-type(n)
- first-child
- Last-child
- Only-child
- empty 没有子元素的元素
- not(element) element可以是任意的selector

```html
<div>
	<div>1</div>
	<p>2</p>
	<p>3</p>
</div>
```

P:nth-child(2) 会是2

P:nth-of-type(2) 会是3

因为nth会去找父元素，然后在里面找第n个

### pseduo elements

::first-line ::first-class ::before ::after

```
::before {
	content: url()
	font-size: ''
	clear: both 清除浮动
}
```

### Css 特性

- 继承

  - font-size
  - color
  - Font-weight
  - font-family
  - text-indent
  - text-style
  - text-align
  - line-height
  - word-spacing
  - Letter-spacing
  - text-transform
  - color
  - list-style
  - cursor
  - 继承的是计算值，比如1em会进行换算后，直接继承px值

- 层叠

  - 选择器的权重不同
    - !important: 10000
    - Line-style: 1000
    - id: 100
    - Class, pesudo-class, attractive: 10
    - Element: 1
    - *: 0
  - 优先比较priority的选择器的个数
  
  

### 列表

ol, li; ul, li;

Dl dt dd (dl：definition list；dt：define type；dd：define description)

List-style-type: 设置li前面标记的样式（decimal，upper-roman）

list-style-image: 设置li前面标记为图片

### 表格

Tr: table row

Td: table decription

cellpadding:单元格内部间距, cellspacing: 单元格之间的间距,border, width, align:对齐方式，center，left，right

tr -> valign align

Td -> valign align width height rowspan colspan(单元格合并)

Border-clloapse: 边框合并

caption，tbody，thead, th

Border-spacing: 边框的上下左右的margin

### 表单

input type: text, password, checkbox(name), button,radio(name),file, submit, reset

fieldset+ legend: 表单外边框+说明

select+options：下拉菜单 multiple selected

textArea rows cols resize：none禁止用户拖放textarea

placeholder, maxlength, readonly, disabled, checked（默认被选中), autofocus

Submit: action 类似 http://google.com target: 打开页面位置

label for=""，里面是input的id

enctype: application/x-www-form-urlencoded / form/multidata

### Block/line element/ box-sizings

块级：独占父元素的一行，div, p, h1-h6, pre, ul, li

行内级：a,ima,iframe,strong,span,code,input

替换级：内部的value会被一些内容替换， input，iframe，video等，可以设置宽高

非替换级：内部的value是由text或者一些其他的东西决定，a,span，label等，不可以设置宽高

display：block，inline-block（在同一行显示，同时可以设置宽度和高度），inline

Line-table: 在行内级显示的table

Display: none 元素不再占据空间，也不再显示

Visiablity：visable, hidden 元素会隐藏，但是依然会占据原来的空间

overflow: visiable, hidden,scroll,auto（如果溢出了显示滚动条，否则没有滚动条）

**行内级元素之间有空格解决方案**

父元素中font-size：0，让子元素去覆盖

加入float

margin auto：如果是inline元素，包裹内容；如果是块级元素，独占一行

Min-width, max-width

Max-width max-height

Word-break

```html
<div>
ssssssssssssssssssssssssss
</div>
```

这样子会把这些字母当成一个单词，所以需要使用word-break: break-all 来进行单词分割

**上下margin会进行传递，子元素如果和父元素的顶部重叠，给子元素设置margin-top的时候，相当于只给父元素设置了margin-top，同样作用于margin-bottom（父元素的大小是auto）**

可以设置padding-top，padding-bottom，border解决

overflow：hidden  触发BFC

**如果上下两个margin都设置了margin-top和margin-bottom，那样有可能会把两个margin合并成为一个margin，这个称之为collapse，max(margin-top, margin-bottom)**

Border-style:

- solid
- dashed
- doubled
- dotted
- Inset,outset
- ridge, groove

**实现三角形**

利用border比content大，border会进行等分，那么如果content无限小，左边border和上面的border会在对角线进行等分，出现三角形

```css
.tri {
	width: 0;
	height: 0;
	
	border-left: 50px solid black;
	border-top: 50px solid green;
	border-bottom: 50px solid red;
	border-right: 50px solid blue;
}
```

inline元素css不生效：

- margin-top
- margin-bottom
- height
- width

margin-left和margin-right是生效的

padding-top和padding-bottom会生效，但是不会占据空间，所以bottom或者top的元素不会变化

border-radius 的值相当于四个角的直径值，所以如果宽度一半是border-radius，就变成了圆形

**box-shadow**

Box-shadow: inset? Length{2, 4} color?

Length: 水平偏移 垂直偏移 模糊半径 延伸距离

**text-shadow**

Text-shadow：length{2,3} color?

length：水平偏移 垂直偏移 模糊半径

**box-sizing**

- Content-box 内容盒子：设置width和height时，只是指定了内容的宽高
- border-box 边距盒子：content+border+padding 如果设置了padding，相当于内减

### css居中

- 普通的文本: text-align:center
- 行内元素：text-align: center
- 图片 text-align：center（因为是inline-block）
- inline-block元素: text-align: center
- 块级元素：margin：0 auto（同时还得width《父元素）

margin：0 auto的原理：

Margin-right:auto + margin-left:auto，auto的意思是将剩余空间全部分配，相当于left和right均分

垂直不能设置auto，因为默认值是0

**margin-top百分比参考的是width，所以margin-top：50%会比预想中多很多**

background-position （right bottom）或者直接用px

background-attachment local背景图片会随着文字滚动而滚动 fixed 背景不会随box滚动

**img和background-image的选择：**

img有seo优化，因为有alt属性

img优先加载，background需要等相应的元素加载完，再根据css的background下载对应的图片

cursor: auto / text / pointer / default

**标准流 （normal flow）**

margin,padding和inline，block决定了定位的位置

不利于进行元素的层叠

定位，脱离标准流

position:

- relative：相对于标准流中的位置，改变位置
- absolute：如果父元素是relative，根据父元素的左上角进行定位，否则继续往上找。如果没有relative，那么根据浏览器的视口进行定位。
- fixed
- Static

脱离标准流元素的特点（fixed，absolute）：

- 可以设置高度和宽度
- 宽高默认情况下由内容决定的
- 不再受标准流的约束
- 和display的关系，脱离标准流之后会变成block,长宽变成了auto

absolute：

- 脱离标准流
- left, right, top, bottom 进行元素的定位
- 子绝父相
- 绝对定位 margin-left + left + content + margin-right + right  = parent width/ margin-top + top + content + margin-bottom + bottom  = parent height
- absolute 占据100%宽度和高度，除了100%设置，也可以left:0, bottom:0 或者 top:0, bottom:0 
- Absolute 居中 left: 0, right:0, margin: 0 auto;

### 居中的三种方式

| Type        | Horizontal                       | Vertical                                                     |
| ----------- | -------------------------------- | ------------------------------------------------------------ |
| normal flow | Margin:0 auto                    | margin-top: 2/height of parent, transform: translate(2/height of self) |
| flex box    | Justify-content: center          | align-items: center                                          |
| Absolute    | Left: 0, right:0, margin: 0 auto | top: 0, bottom: 0, margin: auto 0                            |

z-index：正整数，负整数，0，siblings，如果z越大，覆盖在上面，z如果相等，后写的覆盖在上面

### Float

一旦浮动，脱离标准流，朝着左边或者右边一直浮动，直到贴近父类元素或者其他浮动的元素的边界。

- 定位元素会在float元素的上面
- float不能和行内级元素重叠（inline-block，inline），但能和块级元素重叠
- 浮动元素之间不能重叠

文字环绕图片的效果：本来是因为vertical-align，导致文字和图片baseline对齐。但是一旦浮动，会脱离BFC，所以导致vertical-align失效，导致文字环绕的效果

**高度坍塌：**

因为一旦使用float，就会脱离标准流，那么就不会将高度汇报给父元素，最后div container认为没有高度，height：auto直接变成了0.

解决办法：

清除浮动，::after

```css
.container::after {
  content: "";
	clear: both;
  display: block;
}
```

### CSS3

- tranform
  - translate(x, y) 如果是百分比的话，是针对自身的width或者height
  - Scale(x, y)
  - rotate(deg) 正数是顺时针，负数是逆时针
  - skew(deg, deg)
  
- transform-origin(x, y) 设置x和y，作为新的变形的原点

- transition: transition-property, transition-duration, transition-timing-function, delay
  - transform or others with pseudo class
  - ms
  - ease, ease-in ease-out linear
  - ms
  
- keyframe
  
  - @keyframe
  
  ```css
  @keyframe test {
  	0% {
  		transform: translate(0, 0)
  	}
  	
  	25% {
  		transform: translate(100, 0)
  	}
  	
  	50% {
  		transform: translate(200, 200)
  	}
  	
  	75% {
  		transform: translate(100, 0)
  	}
  	
  	100% {
      transform: translate(0, 0)
  	}
  }
  
  animation test 1000ms ease-in-out
  ```
  
  

**vertical-align**

行盒：每一行都有一个行盒，将内容进行包裹，但是是baseline对齐的，所以会有padding，即便没有text

使用vertical-align：middle，top等，可以消除这个多出来的padding。

vertical-align是保证中心点和X的中心对齐的（但是文字下沉，导致中心点下沉），所以vertical-align的middle并不是真的居中

**高斯模糊**

fliter: blur(8px)

### HTML5

- 语义化标签

  - Header
  - nav
  - Section
  - article
  - Footer
  - aside
  - 让tag name更语义化

- 新增的标签

	- video src controls / autoplay / muted
  - audio src controls / autoplay / muted
  
- 表单扩展
  - type: tele, date, time, email
  
  
  
### Flex
- order
  - 可以决定排列的顺序，值越小的排在越前面
- align-self
  - 可以跳出align-items的规则，比如align-items是center，可以自己是flex-end
- flex-grow
  - 根据比重，分配剩余的空间
  
- flex-shrink
  - 如果宽度大于了容器宽度，那么根据shrink的比例进行缩放
  
- flex-basis
  - 决定flex-item的宽度
  
- flex
  - 是flex-grow ｜ flex-shrink | flex-basis的综合
  

### 字体

- @font-face：src, font-family ，可以引用网络的字体

-webkit-

-moz-

### 文字溢出

```css
text-overflow: ellipse;
overflow: hidden;
workspace: nowrap;
```

### 移动端适配

header: 

```html
<meta name="viewport" content="width: device-width">
```

设置大小的单位：

- px：像素
- em：
  - font-size:相对于父元素，em=1相当于100%
  - Width：相对于自己的/父元素的font-size
- %：
  - font-size: 相对于父元素的font-size
  - width：相对于父元素的width
  - margin-top：相对于父元素的width
- rem
  - 相对的是root的font-size

移动端适配：

- 统一使用rem单位，对不同的元素进行配置
- 针对不同的device，设置不同的font-size
- @media screen and (max-width < 414px)

# 数据结构算法

## 排序算法

简单排序：冒泡 - 选择 - 插入

高级排序：希尔 - 快速

### 冒泡排序

从第一个开始，依次往后面进行排序和交换, 依次挑选出最大的，第二大的，。。。

时间是 n + (n - 1) + (n - 2) + (n-3) + ... 1 = n(n - 1) / 2

交换次数：n(n-1) / 2

```javascript
ArrayList.prototype.swap = function (m, n) {
                var temp = this.arr[m]
                this.arr[m] = this.arr[n]
                this.arr[n] = temp
            }

            // 冒泡排序
            ArrayList.prototype.bubblesort = function () {
                for (let j = this.arr.length - 1; j > 0; j --) {
                    for (let i = 0; i < j; i ++) {
                        if (this.arr[i] > this.arr[i + 1]) {
                            // swap
                            this.swap(i, i + 1)
                        }
                    }
                }
            }
```

### 选择排序

挑选出最小的一次，然后依次放入到左端

时间是 n + (n - 1) + (n - 2) + (n-3) + ... 1 = n(n - 1) / 2

交换次数：n-1

```javascript
ArrayList.prototype.selectsort = function () {
                // start的位置动态决定
                for (let j = 0; j < length - 1; j ++) {
                    let min = j;
                    for (let i = min + 1; i < length; i ++) {
                        if (this.arr[min] > this.arr[i]) {
                            min = i
                        }
                    }
                    this.swap(min, j)
                }
            }
```

### 插入排序

核心思想是局部有序，那么只需要将不有序的item和有序的进行比较

```javascript
ArrayList.prototype.insertsort = function () {
                const length = this.arr.length

                for (let i = 1; i < length; i ++) {
                    // 获取i位置的元素，和前面的局部有序比较
                    var temp = this.arr[i]
                    var j = i
                    while (this.arr[j - 1] > temp && j > 0) {
                        this.arr[j] = this.arr[j - 1]
                        j --
                    }

                    this.arr[j] = temp
                }
            }
```

### 希尔排序

解决插入排序中可能出现的，最小项出现在最右边，导致全部遍历的问题

**分组**

间隔比较大，然后依次缩小间隔，最后间隔为1，就是插入排序

不同的增量会导致不同的效率

```javascript
ArrayList.prototype.shellsort = function () {
                const length = this.arr.length

                // 初始化的增量
                let gap = Math.floor(length / 2)

                // 增量减小
                while (gap >= 1) {
                    // 拿到对应的元素
                    for (let i = gap; i < length; i++) {
                        var temp = this.arr[i]
                        var j = i
                        while (this.arr[j - gap] > temp && (j > gap - 1)) {
                            this.arr[j] = this.arr[j - gap]
                            j -= gap
                        }
                          // 将j元素复制到temp
                        this.arr[j] = temp
                    }
                    gap = Math.floor(gap / 2)
                }

            }
```

### 快速排序

相比于冒泡，在找到最大或者最小的元素后，以最小的代价放入到对应的位置

使用归并的方式，分而治之

- 选择一个pivot，然后放到最右边
- 左右两个元素，设置pointer
- left寻找比右边大的，right寻找比右边小的，然后交换
- 重复以上步骤
- 如果left=right,说明全部交换完了，然后left和pivot交换
- 然后左边和右边重复上面的步骤

如何选择pivot？

- 随机数：本身就消耗性能，而且无法保证能选到中位数
- 选择**头，中，尾**的中位数

```javascript
ArrayList.prototype.getPivot = function (left, right) {
                const center = Math.floor((left + right) / 2)
                if (this.arr[left] > this.arr[center]) {
                    this.swap(left, center)
                }  
                if (this.arr[center] > this.arr[right]) {
                    this.swap(center, right)
                }
                if (this.arr[left] > this.arr[center]) {
                    this.swap(left, right)
                }

                this.swap(center, right - 1)

                return this.arr[right - 1]
            }

            ArrayList.prototype.quicksort = function () {
                this.quick(0, this.arr.length - 1)
            }

            ArrayList.prototype.quick = function () {
                // 结束递归
                if (left >= right) {
                    return 
                }
                // 获取枢纽
                let pivot = this.getPivot(left, right)
                // 定义指针
                let i = left
                let j = right - 1

                // 开始进行交换
                while (i < j) {
                    // 寻找合适的位置
                    while (this.arr[++i] < pivot) {

                    }
                    while (this.arr[--j] > pivot) {

                    }

                    if (i < j) {
                        this.swap(i, j)
                    } else {
                        break;
                    }
                }

                // 将枢纽放置正确位置
                this.swap(i, right - 1)

                this.quick(left, i - 1)
                this.quick(i + 1, right)
            }
```



# Javascript

## 1. 浏览器工作原理

Java, C, C++ 编译成可执行文件

javascript 源代码解释，再执行

-- 如何在浏览器中执行？

```
输入域名 -》ip地址 -〉index.html -》解析script标签-〉下载js文件-》浏览器运行文件
```

-- 浏览器内核？

Gecko (firefox, Netscape)

Trident (IE4 - IE11)

Webkit (chrome, safari)

Blink (Google, Chrome, Opera)

- layout engine
- rendering engine

-- 浏览器渲染过程

```
Html - > html parser -> html dom tree -> attachment -> render Tree (layout) -> painting -> display

Style sheet -> Css parser -> style rules
```

-- js引擎

将JavaScript解析成为machine code，最后被cpu执行

SpiderMonkey

Chakra (IE)

JavascriptCore(webkit)

V8 (Chrome)

-- webkit

Webcore (HTML 解析，布局，渲染)

JavascriptCore (解析，执行JavaScript代码)

-- v8

v8可以独立运行（Chrome），也可以嵌入到任何C++库中（Node）

```
Javascript代码 -》词法分析 [tokens: {type: 'keyword', value: 'const' ...}]，语法分析  
-〉AST (抽象语法树) [VariableDeclaration, Identifier...] => bytecode（TurboFan） =》machine code
```

Bytecode?

因为不同的cpu有不同的架构，machine code会有区别，所以需要先转成字节码，再转成不同的cpu指令

TurboFan？

如果一个函数多次执行，则会优化成为machine code，否则转为assemble language

Deoptimization？

如果函数类型不同，会反向转为bytecode，重新转化

所以Typescript的效率会高很多

PreParser?

LazyParser, 必要的函数进行预解析

全量解析(full parser)需要在运行时再解析

## 2. GO，VO，AO

代码解析过程？

* --代码解析，v8引擎内部创建一个对象

  var GlobalObject(GO) = {

  console, Math, String, Date, Object, window:this....

  }

 * --执行代码

   v8引擎为了执行函数，内部会有执行上下文栈（execution Context Stack）

   ECStack -> 函数的进栈和出栈

   为了执行全局代码，创建一个全局执行上下文 (Global execution Context)

   Global execution Context

   -- VO:GO(variable object)

   ```js
   var name = "rzh"
   
   /**
   * 作用域提升
   */
   console.log(num1) // undefined
   var num1 = 1
   
   console.log(window) // window == globalObject
   ```

   -- 全局函数

   - 开辟一块空间来存储函数

   ​	[[scope]]: 父级作用域 (全局作用域)

   ​	函数执行的代码块

   在GO中存储的是函数的引用地址

   - 函数执行上下文(functional execution context)

     -- VO:AO (active object)

     ```js
     function foo() {
     	var a='rz',
     	var b='b'
     }
     ```

     a和b会被提升到AO，编译阶段会被解析成为undefined

     执行完就会被销毁，之后重新再开辟空间

   --全局变量和函数变量冲突？

   ```js
   var name = "rzh"
   
   function foo() {
   	console.log(name)
   }
   ```

   AO 中的name应该是undefined，之后向上级作用域进行查找（作用域链）

   scope （AO + GO）

### 2.1. 变量环境和记录

每一个执行上下文会被关联到一个环境变量（variable object），源代码中的变量或者函数的声明，会被作为属性添加到variable object中

对于函数来说，参数也会被添加到variable object中

**variable environment** **variable record**

### 2.2 作用域提升（scope hoisting）面试题

```javascript
/**

 *  200
 *  先执行GO compiler， n = undefined
 *  再顺序执行foo()和console.log(n)
 *  GO n -> 200
    */
    var n = 100
    function foo() {
    n = 200
    }

foo()
console.log(n)

/**

 * 先执行Compiler, 函数自己AO有n，n为undefined
 * 再执行foo（）
 * undefined 200
   */

function foo() {
    console.log(n)
    var n = 200
    console.log(n)
}

var n = 100
foo()

/**

 * 首先foo2()创建FEC和AO对象n = 200，
 * 然后foo1()创建FEC和AO对象空
 * 所以n会查找到GO对象n = 100
 * 最后n查找global object = 100
   */
   var n = 100

function foo1() {
    console.log(n) //100
}

function foo2() {
    var n = 200
    console.log(n) // 200
    foo1()
}

foo2()
console.log(n) //100

/**

 * 编译阶段是不管return的，所以AO还是会有a，当执行阶段不赋值
 * return只有在执行阶段才会生效
 * a =》 100
 * */
   var a = 100

function foo() {
    console.log(a) // 100
    return 
    var a = 100
}

foo()

/**

 * 如果m没有类型，m会加入到GO
 * 如果m是var，加入到AO
   */

function foo() {
    m = 100
}

foo()
console.log(m)

/**

 * b = 10
 * var a = 10
   */

function foo() {
    var a = b = 10
}

foo()
console.log(b) // not defined
console.log(a) // 10
```

## 3. 内存管理	

代码执行分配内存（disk -》memory -〉CPU）

手动管理内存(C, C++, OC malloc, free)

自动管理内存

-- 分配申请内存空间 var obj = {}

-- 使用申请到的内存 var obj = {name: 'rz'}

-- 不需要使用的时候，进行释放 

自动管理内存(Java, javascript, python..)

### 3.1 js内存管理

- 在声明变量时自动创建内存
- 基本数据类型，直接在栈空间进行分配
- 复杂数据类型（function，object），用heap空间进行分配，然后保存地址，引用数据
- 垃圾回收机制（garbage collection GC）垃圾回收器

GC算法

引用计数 retain counter，当引用计数为0时，就开始free memory

- 循环引用 -》内存泄漏

标记清除 root object，从root开始查找，如果unreachable，则这个对象是不可用的

## 4. 闭包（clousure）

js中函数是一等公民

函数可以作为另外一个函数的参数，也可以作为另外一个函数的返回值来使用

高阶函数（high-order function）：如果一个函数能够接受另外一个函数作为参数

-- forEach, map, find, reduce, filter

当捕捉闭包的时候，它的自由变量会被确定，这样即使脱了捕捉的上下文，它也能照样运行

一个函数和对其周围状态的引用捆绑在一起，这样的组合就是闭包

一个内层函数可以访问到外层函数的作用域

```javascript
function foo() {
    var name = "rzh"
    function bar() {
        console.log("bar " + name)
    }
    return bar 
}

var fn = foo()
fn()
```

在foo执行完之后本来foo的AO本来应该销毁，但是内部bar function还能够访问foo的AO，这样就形成了闭包

闭包：函数+可访问的自由变量（bar + name）

上层作用域是在parser阶段就被确定了

- 如果函数之间的引用一直存在，但是只调用了一次或者有限次，那么会内存泄漏
- fn = null

```javascript
function foo() {
	var name = "rzh"
	var age = 12
	
	function info() {
		console.log(name)
	}
	return info
}

var fn = foo()
fn()
```

- 理论上这里虽然没用到age，但是依旧不会被销毁，因为foo的AO对象有引用，AO对象不会删除任何属性

- 但是js engine自己做了优化，不用的属性会被销毁

## 5. This

全局的this指向的是window对象，

Node环境下是一个空对象{}

```javascript
function foo() {
    console.log(this) // window
}

// 直接调用函数
foo()

// 创建一个对象，对象中函数指向foo
var obj = {
    name: 'rzh',
    foo: foo // obj对象
}

obj.foo()

// apply 调用
foo.apply('123') // string
```

this的指向和函数的位置无关，和调用的方式有关

### 5.1 默认绑定

函数调用没有绑定到对象上面，指向window

```javascript
function foo() {
    console.log(this) // window
}

function foo1() {
    foo()
    console.log(this) //window
}

function foo2() {
    foo1()
    console.log(this) // window
}

foo2()

/**
* 在被调用的时候才会分配this，所以this在被调用的时候没有绑定对象，window
*/
var obj = {
    foo: function () {
        console.log(this)
    }
}

var bar = obj.foo
bar()
```

### 5.2 隐式绑定

通过某个对象进行调用

**对象发起的调用**

对象会给绑定到function的context中

```javascript
function foo() {
	console.log(this)
}

var obj = {
	name: 'rzh',
	foo: foo
}

obj.foo() //obj
```

### 5.3 显式绑定

call和apply直接改变this的指向

call和apply只有传参数的区别，一个是剩余参数，一个是array

bind直接绑定this

```javascript
function sum(num1, num2) {
    console.log(num1 + num2, this)
}

sum.call("call", 20, 30)
sum.apply("apply", [20, 30])

var newFunc = sum.bind('aa', 20, 30)
newFunc()
// newFunc() 的默认绑定和显示绑定冲突，显示绑定的优先级更高
```

### 5.4 new 绑定

```javascript
/**
 * 
 * @param {*} name 
 * @param {*} age 
 */
function Person(name, age) {
    this.name = name
    this.age = age
}

var p = new Person("rzh", 18)
```

这样会先创建一个对象，然后绑定this

### 5.5 一些常见的内置函数的this指向

```javascript
说明是直接调用的fn()
setTimeout(function (){
    console.log(this) //window
}, 2000)

说明是boxDiv.click()
boxDiv.onclick = function () {
    console.log(this) // <div></div>
}

收集到依赖数组中，然后使用fn.call()
boxDiv.addEventListener('click', function () {
  console.log(this) // <div></div>
})

第二个参数是可以改变this的指向的，说明内部肯定是有call的
thisArgs?
forEach, map, filter..
var vals = [1, 2, 3]
vals.forEach(function (val) {
    console.log('forEach', this) // string abc
}, "abc")
```

### 5.6 函数调用的优先级

默认绑定的优先级最低

显示绑定高于隐式绑定（call，apply 》obj.fun()）

new绑定的优先级隐式绑定

new的优先级高于显示绑定

### 5.7 特殊的绑定

- Apply(null)

apply(undefined)

这时会自动将this绑定成window对象

- 间接函数引用

```javascript
var obj1 = {
    name: 'obj1',
    foo: function() {
        console.log(this)
    }
}

var obj2 = {
    name: 'obj2'
};

(obj2.bar = obj1.foo)() // window
```

### 5.8 箭头函数

箭头函数内部是没有this和arguments的

```javascript
const age = () => 18
const info = () => ({name: 'sd', age: 12})
```

箭头函数不适用四种规则，它的this决定于上层作用域

```javascript
// 模拟网络请求，此时setTimeout里面只能用箭头函数，因为他会搜索外层作用域getData，它的this是mockNetwork对象，因为mockNetwork对象调用了getData
// 否则需要在外层作用域保存this
var mockNetwork = {
    data: [],
    getData: new Promise(function (resolve, reject) {
        setTimeout(() => {
            this.data = [1, 2]
            resolve()
        }, 2000)
    })
}

mockNetwork.getData.then(res => {
    console.log(this.data)
})
```

### 5.9 面试题

```javascript
/**
* 关于上级作用域，只看是谁调用的，不要看compile之前的代码
*/
var name = "window"
var person = {
    name: 'person',
    sayName: function() {
        console.log(this.name)
    }
}

function sayName() {
    var sss = person.sayName
    sss() // window
    person.sayName() // person
    (person.sayName)() // person ==> person.sayName()
    (b = person.sayName)() // window
}
——————————————————————————————————————————————————————————
var name = "window"

var person1 = {
    name: 'person1',
    foo1: function () {
        console.log(this.name)
    },
    foo2: () => {
        console.log(this.name)
    },
    foo3: function() {
        return function () {
            console.log(this.name)
        }
    },
    foo4: function () {
        return () => {
            console.log(this.name)
        }
    }
}

var person2 = {name: 'person2'}

person1.foo1() // person1
person1.foo1.call(person2) // person2
person1.foo2() // window 上层作用域是global
person1.foo2.call(person2) // window

person1.foo3()() // window 等于是默认绑定
person1.foo3.call(person2)() // window 等于是默认绑定
person1.foo3().call(person2) // person2

person1.foo4()() // person1 上层作用域是foo4
person1.foo4.call(person2)() // person2
person1.foo4().call(person2) // person1 因为call不绑定箭头函数
——————————————————————————————————————————————————————————————————
var name = 'window'
function Person (name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  },
  this.foo2 = () => console.log(this.name),
  this.foo3 = function () {
    return function () {
      console.log(this.name)
    }
  },
  this.foo4 = function () {
    return () => {
      console.log(this.name)
    }
  }
}
// person1 = {name: 'person1', foo1(), ....}
var person1 = new Person('person1')
// person2 = {name: 'person2', foo1(), ....}
var person2 = new Person('person2')

person1.foo1() // person1
person1.foo1.call(person2) // person2

person1.foo2() // person1 上层作用域是Person
person1.foo2.call(person2) // person1 箭头函数没有call

person1.foo3()() // 独立函数调用 window
person1.foo3.call(person2)() // 独立函数调用 window
person1.foo3().call(person2) // person2

person1.foo4()() // person1 上层作用域是Person
person1.foo4.call(person2)() // person2
person1.foo4().call(person2) // 箭头函数没有call， 所以是person1
——————————————————————————————————————————————————————
var name = 'window'
function Person (name) {
  this.name = name
  this.obj = {
    name: 'obj',
    foo1: function () {
      return function () {
        console.log(this.name)
      }
    },
    foo2: function () {
      return () => {
        console.log(this.name)
      }
    }
  }
}
// Person {name: 'person1', obj: {name: 'obj', foo1(), foo2()}}
var person1 = new Person('person1')
// Person {name: 'person2', obj: {name: 'obj', foo1(), foo2()}}
var person2 = new Person('person2')

person1.obj.foo1()() // window 独立函数调用
person1.obj.foo1.call(person2)() // window 独立函数调用
person1.obj.foo1().call(person2) // person2

person1.obj.foo2()() // 箭头函数上级作用域 obj 因为是被obj调用的
person1.obj.foo2.call(person2)() // person2 上层作用域被绑定成了person2
person1.obj.foo2().call(person2) // obj 箭头函数不绑定this，所以是被obj调用的

```

### 5.10 手动实现call，apply和bind

```javascript
// 所有函数添加zhCall方法

Function.prototype.zhCall = function (thisArg, ...args) {
    var fn = this
    // type = arguments[0].constructor
    // type.prototype.fn = fn
    // arguments[0].fn()

    thisArg = thisArg ? Object(thisArg) : window
    thisArg.fn = fn
    thisArg.fn(...args)
    delete thisArg.fn
}

Function.prototype.zhApply = function (thisArg, args) {
    var fn = this
    thisArg = thisArg ? Object(thisArg) : window
    thisArg.fn = fn
    args = args || []
    thisArg.fn(...args)
    delete thisArg.fn
}

Function.prototype.zhBind = function(thisArg, ...arrayArgs) {
    var fn = this
    thisArg = (thisArg !== undefined && thisArg !== null) ? Object(thisArg) : window
    return function(...args) {
        thisArg.fn = fn
        finalArgs = [...arrayArgs, ...args]
        var result = thisArg.fn(...finalArgs)
        delete thisArg.fn
        return result
    }
}

function foo() {
    console.log(this)
}

function sum(num1, num2) {
    console.log(this, num1 + num2)
}

foo.zhCall(123)
sum.zhCall("123", 20, 30)

sum.zhApply("123", [20, 30])

var bar = sum.zhBind("123", 20)
bar(30)
```

## 6. Arguments

arguments是一个类数组，本质上是一个对象

- 获取参数的长度（arguments.length）
- 索引值获取具体的参数
- callee 获取到当前的函数

```javascript
三种方法将arguments转为array
/**
* slice默认便利一个iterator，转为数组
*/
var newArr2 = Array.prototype.slice.call(arguments)

/**
* Es6
*/
var newArr3 = Array.from(arguments)

/**
* 展开运算符
*/
var newArr4 = [...arguments]
return newArr4
```

箭头函数是没有arguments，回到上层作用域当中找

```javascript
function foo(num1, num2) {
    return () => {
        console.log(arguments)
    }
} 

foo(1, 2)() // arguments {'0': 1, '1': 2}
```

## 7. Javascript 函数式编程

### 7.1 纯函数

- 函数在相同的输入时，需要有相同的输出
- 输入和输出与外部的变量影响无关，只和输入值是有关的
- 不能产生副作用

slice和splice

slice是新建了一个空数组，将对应的sub array放入到数组中，所以这是一个纯函数

splice是直接对原数组进行修改，这个修改就是产生了副作用

- 只需要关心函数的实现逻辑，输入值和输出值都不需要改变
- 比如react的函数式组件里面的props，不能改变props的值，只能拿到props的值

### 7.2 柯里化

只传递给函数一部分参数来调用他，让他返回一个函数来处理另外的参数

```javascript
function foo() {
	return function (m) {
		return function (n) {
			return function (x) {
				....
			}
		}
	}
}
```

- **让每个函数的职责变得单一，而不是将一大堆的任务全部交给一个函数去处理** single responsibility principle
- **逻辑复用，定制一些方法** reuse some features with functions

```javascript
function makeAdder(count) {
    return function (number) {
        return count + number
    }
}

var adder5 = makeAdder(5)
adder5(10)
adder5(100)
```

```javascript
/**
 * 柯里化函数的实现
 */
var zhCurrying = function (fn) {
    function curried(...args) {
        // 判断已经接收的参数和需要接收的参数数量是否一致
        // fn.length 表示函数的参数个数
        if (args.length >= fn.length) {
            // 保证this和fn的this保持一致
            return fn.apply(this, args)
        } else {
            // 没有达到参数个数时，返回一个函数拼接参数
            // 递归用curreid判断是否达到了函数的参数值
            return function (...args2){
                return curried.apply(this, args.concat(args2))
            }
        }
    }
    return curried
}

var curryAdd = zhCurrying(sum)
console.log(curryAdd(10, 20)(30))
```

### 7.3. 组合函数

一个数据可以将函数依次调用的时候，就可以成为一个组合函数

```javascript
/**
*手动实现compose函数
*/
function zhCompose(...fns) {
    var length = fns.length
    for (var i = 0; i < length; i++) {
        if (typeof fns[i] !== 'function') {
            throw new TypeError('please input function')
        }
    }

    function compose(...args) {
        var index = 0
        var result = length ? fns[index].apply(this, args) : args
        while (++index < length) {
            result = fns[index].call(this, result)
        }
        return result
    }
    return compose
}

var new_fn = zhCompose(double, square)
console.log(new_fn(20))
```

## 8. javascript 知识补充

### 8.1 with语句

with是有自己的作用域的，有一个js对象的AO

```javascript
obj = {message: 'hello world'}
var message = '123'
function message() {
    function bar() {
        with(obj) {
            console.log(message) // 上层作用域是with
        }
    }
    bar()
}
```

可以在上下文中明确指定一个变量

### 8.2 Eval函数

可以将string转化成JavaScript代码

- 可读性非常差
- 因为是plain text，容易被篡改，造成安全隐患
- 字符串需要解释器进行解析，但是js代码会被js引擎进行词法分析，语法分析进行相关的优化

### 8.3 严格模式

strict mode，避免sloppy code，消除slient error

让js引擎执行代码时有更好的优化

避免使用保留字（class等）

- 意外创建全局变量

  ```javascript
  message = "13"
  ```

- 不允许函数有相同的参数名

- 静默错误

  ```javascript
  true.123 = "123"
  Object.defineProperty(obj, "name", {writeable: false})
  ```

- 不允许使用八进制的数字

- with函数不可以被使用

- eval函数不能向上引用变量了

- **自执行函数(默认绑定)会指向undefined**

  ```javascript
  "use strict"
  function foo() {
  	console.log(this) // undefined
  }
  
  foo()
  ```

- setTimeout还是指向window，实现是fn.apply(window)

## 9. javascript 面向对象

### 9.1 对象的两种定义形式：

```javascript
var obj = new Object()
obj.name = "rzh"
obj.rest = function () {
    console.log('resting...')
}
obj.working = function () {
    console.log('working...')
}

// 自变量形式
var obj2 = {
    name: 'rzh',
    age: 18,
    rest: function () {
        console.log('resting...')
    },
    working: function () {
        console.log('working...')
    }
}
```

### 9.2 对象的属性

- 数据类型描述符 (data properties)
- 存取类型描述符 (accessor properties)

|                     | Configurable | Enumerable | Value | Writable | Get  | Set  |
| :------------------ | ------------ | ---------- | ----- | -------- | ---- | ---- |
| data properties     | y            | y          | y     | y        | n    | n    |
| accessor properties | y            | y          | n     | n        | y    | y    |

- **Configurable: 可以通过delete删除属性**
- **Enumerable: 是否可以通过遍历得到这个属性**
- writable：是否可以修改这个属性
- value：默认是undefined

存储描述符

隐藏私有属性，不希望暴露给外部

```javascript
var obj3 = {
    name: 'rsz',
    _job: 'student',
  	/**
  	相当于getter和setter
  	set job(val) {
      this._job = val
    },
  	get job() {
      return this._job
    }
    */
}

Object.defineProperty(obj3, 'job', {
    enumerable: true,
    configurable: true,
    get() {
        return this._job
    },
    set(value) {
        this._job = value
    }
})

console.log(obj3.job)
```

数据劫持

```javascript
Object.defineProperty(obj3, 'job', {
    enumerable: true,
    configurable: true,
    get() {
        foo()
    },
    set(value) {
        this._job = value
        bar()
    }
})
function foo() {
    console.log('GET...')
}
function bar() {
    console.log('SET...')
}
obj3.job // GET...
obj3.job = 'army' // SET...
```

- 查看属性的描述符

```javascript
Object.getOwnPropertyDescriptor(obj, "name")
Object.getOwnPropertyDescriptors(obj)
```

- 阻止对象继续扩展属性

```
Object.preventExtensions(obj)
// seal 封条
Object.seal(obj) // 所有的属性的configurable 为false
Object.freeze(obj) // 所有属性的writable 为false
```

### 9.3 对象的创建

- 工厂模式，工厂方法，通过这个工厂生产不同的对象

抽离出相同的部分，其他部分通过传参数

```javascript
function createPerson(name, age) {
    return {
        name,
        age,
        eating() {
            console.log(this.name + 'eating')
        }
    }
}

var p1 = createPerson('zhangsan', 20)
var p2 = createPerson('lisi', 30)
```

**但是现在p1,p2都是object类型，不是具体的类**

- 构造函数（在对象创建的时候调用的方法）

和普通函数一样，但是如果new关键字调用的话，就是构造函数

- 在内存中先创建一个对象
- 对象内部prototype赋值为构造函数的prototype
- 构造函数的this绑定到了这个对象上
- 执行函数体内部的代码
- 返回这个对象

```javascript
/**
 * constructor
 * this = Person{}
 */
function Person(name, age) {
    this.name = name
    this.age = age

    this.eating = function () {
        console.log(this) // Person {name: 'zhangsan', age: 20, eating: [Function eating]}
        console.log(this.name + ' eating..')
    }
}

var p = new Person('zhangsan', 20)
p.eating()
```

构造函数的缺点：内部函数在不同对象创建时会被重复创建

### 9.4 对象的原型

- 对象的隐式原型

当一个对象获取某个属性时，会触发getter的操作

1. 在当前对象中查找对应的属性，如果找到直接使用
2. 如果没有找到，通过原型链进行查找

这样可以方便抽出相同的部分，进行组合和继承

```javascript
var obj5 = {name: 'rzh'}
obj5.__proto__.age = 18
console.log(obj5.age)
```

- 函数的显式原型

```javascript
fn.prototype
```

在new 操作的时候，创建出来的```__proto__``` 会引用构造函数的prototype属性

所以函数的prototype和``` __proto```是相同的

不管修改对象的原型还是函数的原型，其实修改的都是构造函数的原型对象

- contructor属性

原型上的contructor 属性指向了函数本身

fn.prototype.contructor.prototype.contructor...

- 利用原型抽离一些方法

```javascript
function Animal(name, age) {
    this.name = name
    this.age = age
}

Animal.prototype.eating = function () {
    console.log(this.name + 'eating')
}

var dog = new Animal('dog', 2)
var cat = new Animal('cat', 3)
dog.eating()
cat.eating()
```

### 9.5 面向对象的三大特性

- 封装
  - 将所有的属性和方法写到一个类中，被称为封装
- 继承
  - 将重复的代码逻辑放到父类当中，让子类进行代码复用
  - 继承是多态的前提
- 多态

原型链：原型的上面还可以有原型，直到找到Object的原型

```javascript
var obj = {}
obj.__proto__ = {}
obj.__proto__.__proto__ = {}
```

Object的默认的原型是[Object null prototype]，继续查找就是null

```javascript
/**
 * Object.prototype 所有的属性都被设置了enumerable false，所以不可见
 * 
 * constructor 指向Object
 */
console.log(Object.prototype.constructor)

Object.defineProperty(Object.prototype, 'constructor', {
    enumerable: true,
    configurable: true
})

console.log(Object.prototype)

console.log(Object.getOwnPropertyDescriptors(Object.prototype))
```

所有的构造函数都继承了Object

```javascript
function Person(name) {
	this.name = name
}

console.log(Person.prototype.__proto__) // Object
```

#### 9.5.1 继承

1. 原型链继承

```javascript
// 父类
function Person() {
    this.name = "lisi"
}

Person.prototype.eat = function () {
    console.log(this.name + " eating")
}

// 子类
function Student() {
    this.name = "zhangsan"
    this.sno = "123"
}

Student.prototype = new Person()

Student.prototype.study = function () {
    console.log(this.name + " studying")
}
console.log(Student.prototype)
var stu = new Student()
/**
 * student调用了eat，所以this是指向stu的
 */
stu.eat()
```

缺点：

- 继承的属性不可见

- 多个子类继承同一个父类之后，如果同时修改prototype，会出现问题

  ```javascript
  stu1.name = "123" // 不会影响stu2
  stu1.friends.push("123") // 会影响stu2
  ```

- 很难传递参数，比如student想new出来一个有name的对象，但是Student的构造函数里面没有name

2. 改进的原型链继承，使用了constructor stealing，将子类的this绑定到父类中

```JavaScript
function Person(name, age, friends) {
    this.name = name
    this.age = age
    this.friends = friends
}

Person.prototype.eat = function () {
    console.log(this.name + " eating")
}

// 子类
function Student(name, age, friends, sno) {
    // 改变this的指向，Person中的this变成了Student类
    Person.call(this, name, age, friends)
    this.sno = sno
}
Student.prototype.study = function () {
    console.log(this.name + " studying")
}
var stu = new Student('hah', 18, ['fd'], 11)
```

缺点：

- 原型对象会多出属性，不应该在原型上出现
- 原型会被多次调用

3. 原型式继承函数（prototypal inheritance in javascript）

```javascript
/**
* 创建一个对象，并且原型是另外一个对象
*/
var obj = {
    name: 'zhouhao',
    age: 18
}

var info = {

}

function createObject(obj) {
    var newObj = {}
    Object.setPrototypeOf(newObj, obj)
    return newObj
}

/**
 * 使用函数的prototype，new这个函数得到对象
 */
function createObject__(obj) {
    function Fn() {}
    Fn.prototype = obj
    return new Fn()
}

Object.create(obj)

var info = createObject(obj)
```

4. 寄生式继承

使用工厂函数，继承原型

```javascript
/**
 * 寄生式继承
 */
var obj = {
    running: function () {
        console.log("running")
    }
}

function factoryObj(person, name) {
    var stu = Object.create(person)
    stu.name = name

    return stu
}

var stu = factoryObj(obj, 'haha')
console.log(stu.name)
```

5. 寄生组合式继承

使用了一个中间人对象，避免重复使用Person

```javascript
function oPerson(name, age, friends) {
    this.name = name
    this.age = age
    this.friends = friends
}

oPerson.prototype.running = function () {
    console.log('running')
}

function oStudent(name, age, friends, sno, score) {
    oPerson.call(this, name, age, friends)
    this.sno = sno
    this.score = score
}

/**
 * 相比直接使用Person，这里利用一个中间人继承了Person的prototype，
 * student继承了这个中间人
 * 避免了重复使用Person
 * 因为要保证constructor一致，使用defineProperty定义一个属性
 */
var intermidater = Object.create(oPerson.prototype)
oStudent.prototype = intermidater
Object.defineProperty(oStudent, 'constructor', {
    enumerable: false,
    configurable: true,
    writable: true,
    value: oStudent
})

oStudent.prototype.study = function () {
    console.log('study')
}
var stu = new oStudent('zhouhao', 20, ['gg'], 208, 12)
stu.running()
console.log(stu.name)
stu.study()
```

```javascript
function inherit(subType, superType) {
    var intermidater = Object.create(superType.prototype)
    subType.prototype = intermidater
    Object.defineProperty(subType.prototype, 'constructor', {
        enumerable: false,
        configurable: true,
        writable: true,
        value: subType
    })
}
```

#### 9.5.2 Object方法补充

判断是否是自己的属性

```javascript
console.log(info.hasOwnProperty('address'))
console.log('address' in info)
```

instanceof 查看当前对象是否出现在原型链上

```javascript
var stu = new Student()
// stu.__proto__ = Student.prototype
console.log(stu instanceof Student) // true
console.log(stu instanceof Person) // true
console.log(stu instanceof Object) // true
```

#### 9.5.3 对象、函数之间的关系

函数本质上也是一个对象，函数有显示原型对象 Foo.prototype，同时也有隐式原型Foo.__ proto__

Foo.prototype = { constructor }

Foo.__ proto__ 是new Function() 出来的，本质上是Function.prototype

#### 9.5.4 多态 (Polymorphism)

不同数据类型提供单一的接口，或者使用单一符号表示不同的数据类型

- 必须有继承
- 必须重写方法
- 父类引用指向子类对象

但是因为javascript函数是第一公民，所以会有不同形式的多态

### 9.6 ES6 class对象

ES6的class对象本质上是ES5 function的变形，prototype也是一个对象，含有constructor和__ photo__

```javascript
// 类的声明
class Person {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
}

// 类的表达式
var Animal = class {

}

var p = new Person("rzh", 18)
console.log(p)
```

constructor函数：

1. 在内存中创建一个空的对象
2. 这个对象的prototype属性会被赋值为这个类的prototype
3. 构造函数内部的this，会指向创建出来的新对象
4. 执行构造函数内部的代码
5. 构造函数没有返回非空对象，返回创建出来的对象

属性劫持

```javascript
class Person {
	constructor() {
		this._address = "default"
	}
  
  get address() {
    console.log("data thief")
    return this._address
  }
  
  set address(new_address) {
    console.log("data thief", new_val)
    this._address = new_address
  }
}
```

类方法

```javascript
/**
     * 静态方法
     */
static createPerson() {
  return new Person()
}
```

Super()

- 构造函数中调用父类的构造方法，否则会报错

- 在函数重写时，调用原来的方法

- 继承静态方法

Mixins

使用函数和继承进行混入，实现多继承

```javascript
function MixinRunner(BaseClass) {
    return class extends BaseClass {
        running() {
            console.log('running')
        }
    }
}
```

可以类比React的High Order component

```javascript
// 柯里化
function connect(state, dispatch) {
	return function EnhanceHoc(BaseComponent) {
    // 混入，最终返回了一个新的class component
		class EnhanceComponent extends Component {
      render() {
        return (
          <BaseComponent ...props ...dispatch>
        )
      }
		}
	}
}
```

## 10. ES6语法

### 10.1 property shorthand

```javascript
var obj = {
	name: name, => name
}
```

### 10.2 method shorthand

```javascript
var obj = {
	bar: function() {
	
	} => bar() {}
}
```

### 10.3 computed property name

```javascript
var obj = {
	[firstName + lastName]: 'haha'
}

obj[firstName + lastName] = 'haha'
```

### 10.4 Destructuring

```javascript
var obj = {
	name: '123',
	age: 123
}
var { name } = obj // '123' 

var arr = ['123', '12', '1']
var [item1, item2, item3] = arr
var [item1, ...item2] = arr
```

### 10.5 let 和 const

const本质上是传递的值不可以修改，但是如果传递的是引用对象的话，是可以修改的

```javascript
const obj = {
	foo: 'foo'
}
可以修改foo，但是不能直接重写obj, i.e., const obj = {}
因为引用对象只是保存了一个内存地址
```

- 通过let和const定义的对象，不能重复声明
- let和const没有作用域提升的(scope hoisting)，虽然也能够提前解析，但是不会被访问

```javascript
console.log(hoist) //undefined
console.log(_hoist) //Error
/**
* hoist在parse阶段被放入了内存空间 hoist = undefined
*/
var hoist = '123'
/**
* _hoist在parse阶段也会放入调用栈，但是不能被访问，直到access的时候才会被调用。
*/
const _hoist = '1'
```

**ES6下的GO，VO等**

每个execution context关联到variable environment上，在执行代码中变量和函数的声明作为environment record添加到环境变量中

VE -> 一个VaribaleMap(HashMap) <- GO 

variables_: VariableMap

Window和GO不再指向同一个_variable，因为ES6有了新特性，Window会指向另外一个object

**如果用var定义，会直接添加到window上，很容易出现bug**

### 10.6 块级作用域

ES5 中是没有块级作用域的，只有三个东西会形成作用域，function，with和全局作用域

```javascript
/**
 * 三个作用域，全局+两个function作用域
 */
function foo() {
    function bar() {

    }
}
```

ES6中开始有块级作用域，对const，let，function和class有效

```javascript
{
	let name = "123"
	function demo() {
		
	}
	class Person {}
}
```

浏览器为了兼容ES5，所以function不会报错

if，switch和for都是具备了块级作用域，所以在if，switch和for中如果定义了let或者const对象，都不能访问

块级作用域的好处：

```javascript
/** 
* 永远都是4，因为function会到全局作用域中去找i，此时i已经是4了
*/
html..
<button>1</button>
<button>2</button>
<button>3</button>
<button>4</button>

js..
var btns = document.querySelectorAll('button')

for (i = 0; i < btns.length; i++) {
	btns[i].onclick = function () {
		console.log(i) // 4
	}
}

/**
* 解决方法1: 使用立即执行函数，添加一个作用域
* 此时n向外层的function查找，查找到n值，n值是每次i++后都会重新传递给function的参数
* 闭包
*/
for (var i = 0; i < btns.length; i ++) {
    (function (n) {
        btns[n].onclick = function () {
            console.log(n)
        }
    })(i)
}

/**
* 解决方法2: 使用let变量代替var，直接形成一个for为主体的块级作用域
* 此时i会到for里面找，找到对应的值
*/
for (let i = 0; i < btns.length; i ++) {
  btns[i].onclick = function () {
    console.log(i)
  }
}
```

temporal dead zone

如果在块级作用域中，用const和let声明变量，除非已经被初始化了，否则不能被访问

```javascript
var foo = "foo"

if (true) {
	console.log(foo)
	
	let foo = "123"
}
```

### 10.7 let，const，var的选择

var 不推荐使用，很容易出现bug，比如作用域提升等很奇怪的特性

尽量使用const来声明变量，如果之后要修改变量，再改成let

这是为了保证变量的安全性，不让externel随意修改变量

### 10.8 模版字符串 (*template literals*)

```javascript
const template_hello_world = `Hello Wolrd ${name}!`
```

标签模版字符串是可以调用函数的

```javascript
/**
* styled-components
*/
function foo(a, b) {
	console.log(a, b)
}

const name = "name"
// 第一个参数：['hello', 'wolrd']，被切成了多块，放到了数组中
// 第二个参数：$变量
foo`hello${name}world`
```

### 10.9 函数默认值

```javascript
// ES6
function foo(m=1, n=2) {
	console.log(m,n)
}
foo()
--------------------------
// ES5
function foo() {
	var m = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 1
	var n = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : 2
	console.log(m, n)
}
```

有默认值的话，会改变function.length的大小，因为从默认值之后开始的就不会计算length了

### 10.10 函数剩余参数

```javascript
/**
* args是一个数组
*/
function (a, b, ...args) {
	console.log(a, b)
	console.log(args)
}
```

Arguments 本质上不是数组，但是rest parameters是一个数组，同时必须放到参数列表的最后

### 10.11 展开运算符

```javascript
const names = ['abs', 'ss', 'gf']
const ages = [18, 10]
function (...names) {
	console.log(names)
}
const person = [...names, ...ages]
const obj = {...info, address: 'beijing', ...names}
```

浅拷贝

```javascript
const info = {
	name: 'rzh',
	age: 19,
  friend: {name: 'aa'}
}

const obj = {...info, name: 'abc'}

// 改变obj的friend name同时也会改变info的friend name，内存表现是一个地址存储了info，同时friend是另外一个地址。obj是直接将info内容复制到了一个新的地址中，但是引用的friend并没有改变，所以改变obj会改变info，因为指向同一个地址
// 深拷贝 JSON.stringify() + JSON.parse()
```

### 10.12 **Symbol (基本数据类型)

- 对象的属性都是以字符串形式表示的，容易引起冲突。如果不知道对象内部有哪些属性，直接添加会覆盖key（第三方框架）

```javascript
var obj = {
	name: 'rzh',
	friend: {name: 'ss'}
}
```

- 开发中使用了混入，如果出现同名属性，会被覆盖

**Symbol是一个函数，每个Symbol都是唯一的，即使内容一致也不会重复。**

生成后作为对象的属性key值

```javascript
const name = Symbol('name')
const age = Symbol('age')

const obj = {
    [name]: 'rzh',
    [age]: 18
}

obj[name] = 'rzh'

Object.defineProperty(obj, 'name', {
    enumerable: true,
    configurable: true,
    writable: true,
    value: 'ss'
})

console.log(obj[name])
```

Object.keys()直接获取keys，是获取不到Symbol的值的

**Object.getOwnPropertySymbols()**

Symbols 如果想要相同：Symbol.for()如果descriptor是相同的情况下，两者是相同的

获取key：Symbols.keyfor()

### 10.13 **Set

#### 10.13.1 Set

Es6之前的数据存储方式： Array和Object

ES6之后的数据存储：Set，Map，以及另外的形式WeakSet，WeakMap

set和其他language的set一致，元素不能重复

```javascript
const set = new Set()
set.add(10)
set.add(100)
set.add(10)
console.log(set) //Set(2) { 10, 100 }

const set = new Set()
set.add(10)
set.add({name: 'rzh'})
set.add({name: 'rzh'})
console.log(set) // Set(2) { {name: 'rzh'}, {name: 'rzh'}}

// 数组去重
const nums = [11, 22, 33, 44, 11, 11]

const set_1 = new Set()
for (num of nums) {
    set_1.add(num)
}

const new_nums = [...set_1]
const new_nums_from_array = Array.from(set_1)
-----------------------------------------------
const set_2 = new Set(nums)
const new_nums = [...set_1]
const new_nums_from_array = Array.from(set_1)
```

- add
- delete
- clear
- Has

Set的遍历

- ForEach，类似数组
- for of

#### 10.13.2 WeakSet（弱引用，GC回收）

Weakset中只能存放对象类型

Weakset对对象是弱引用，如果没有引用，会被GC回收

strong reference：回收时，引用是有效的

Weak reference：回收时，引用是没有用的，照样会被回收，但是其他和普通的引用一致

```javascript
const set = new Set()
const weak_set = new WeakSet()
let _obj = {
    name: 'rzh'
}

set.add(_obj)
weak_set.add(_obj)
```

分析上面的代码：

**_obj => 0x100{name: rzh} <= set(weak_set)，这时如果 _obj = null, 那么0x100的引用剩下了 set(weak_set) => 0x100，**

**set的引用是强引用，所以0x100不会被回收，但是weakset会被回收**

- has
- add
- delete

Weakset无法遍历，存储到weakSet中的对象无法获取

```javascript
const person_set = new WeakSet()
class Person {
    constructor() {
        person_set.add(this)
    }
    running() {
        if (!person_set.has(this)) {
            throw new Error('Cannot use running')
        }
        console.log('running')
    }
}
/**
* 如果销毁person，p = null
* 如果是set，则Person class不会被回收，因为person_set还引用着Person
*/
var p = new Person()
p.running()

p.running.call({name: 'ss'})
```

### 10.14 **Map

#### 10.14.1 Map

允许对象类型等其他数据类型作为key。

```javascript
const obj = {
    name: 'rzh'
}
const map = new Map()
map.set(obj, 'aa')
console.log(map) // Map(1) { { name: 'rzh' } => 'aa' }

const map_1 = new Map([[obj, 'aa']])
console.log(map_1) // Map(1) { { name: 'rzh' } => 'aa' }

map_1.get(obj)
map_1.has(obj)
map_1.delete(obj)

// 遍历
map.forEach(() => {})
for of
```

#### 10.14.2 WeakMap （响应式原理）

WeakMap的key只能是对象，不能接受其他类型

WeakMap是弱引用，会被GC回收

```javascript
let obj = {
	name: 'rzh'
}
const map = new Map([[obj, 'aa']])
const weak_map = new WeakMap([[obj, 'aa']])
obj = null
console.log(weak_map)
console.log(map)
// WeakMap { <items unknown> } obj的空间会被回收
// Map(1) { { name: 'rzh' } => 'aa' } obj的空间不会被回收
```

WeakMap针对响应式原理

```javascript
const obj = {
	name: 'rzh',
	age: 18
}

const obj_fun1 = () => {
	console.log('aa')
}

const obj_fun2 = () => {
	console.log('aa')
}

const obj_fun3 = () => {
	console.log('aa')
}

const obj_fun4 = () => {
	console.log('aa')
}

const map = new Map()
map.set('name', [obj_fun1, obj_fun2, obj_fun3, obj_fun4])
map.set('age', [obj_fun1, obj_fun2, obj_fun3, obj_fun4])
/**
* 使用weakMap的原因是如果obj销毁了，能直接将obj回收
*/
const weak_map = new WeakMap()
weak_map.set(obj1, map)

Object.defineProperties(obj, 'name', {
  enumerable: true,
  configurable: true,
  get() {
    return obj.name
  },
  set(new_val) {
    weak_map.get(obj1).get('name').forEach((item) => {
      item()
    })
  }
})
```

## 11. ES6+语法

### 11.1 Array.includes (ES7)

```javascript
const arr = [1, 2, 3]
arr.includes(1) // true false boolean值

arr.indexof(1) // 0 下标值
```

### 11.2 平方运算符 (ES7)

```javascript
Math.pow(3, 3)
or
3 ** 3
```

### 11.3 Object.values (ES8)

```javascript
var obj = {
	name: 'rzh',
	age: 18
}

console.log(Object.values(obj))

// 如果是数组
Object.values([1, 2, 3]) // 1, 2, 3
// 如果是字符串
Object.values('123') // 1, 2, 3
```

### 11.4 Object.entries (ES8)

```javascript
// 键值对数组
var obj = {
	name: 'rzh',
	age: 18
}
Object.entries(obj) // [ [ 'name', 'rzh' ], [ 'age', 18 ] ]

// 如果是数组，那么索引值作为key
console.log(Object.entries(arr)) // [ [ '0', 1 ], [ '1', 2 ], [ '2', 3 ] ]
```

### 11.5 String padding (ES8)

```javascript
const message = "Hello World"

/**
*	从开头开始填充 padStart
* 从最后开始填充 padENd
* 填充到指定长度
*/
const new_message = message.padStart(15, '-').padEnd(20, '+')
console.log(new_message) //----Hello World+++++
```

### 11.6 Object.getOwnPropertyDescriptors (ES8)

### 11.7 flat flatMap (ES10)

flat能够根据指定的深度遍历数组，然后将所有元素与遍历到的子数组中的元素合并成一个数组返回

```javascript
const nums = [1, 2, 3, [[123, 43], [12, 33], [[123, 22], [22]]], 4, [12, 56]]

console.log(nums.flat(3)) // 降3维
```

flatMap首先使用映射函数映射每个元素，然后将结果压缩成一个数组

和普通的map相比，自动降了一维

```javascript
const messages = ['hello world', 'hello 123', 'sdsa ss']

const words = messages.flatMap(item => {
	return item.split(" ")
})
```

### 11.8 Object.fromEntries (ES10)

```javascript
const entry = [[1, 2], [3, 4], [5, 6]]

Object.fromEntries(entry) // { '1': 2, '3': 4, '5': 6 }
```

### 11.9 trimStart, trimEnd (ES10)

去除字符串首部或者尾部的空格

### 11.10 BigInt (ES11)

在不使用BigInt的情况下，有些无法正确的表示int

**Number.MAX_SAFE_INTEGER**

```javascript
const maxInt = Number.MAX_SAFE_INTEGER
const bigInt = 900719925474099100n
// 加减乘除运算必须将数字转成大数
console.log(bigInt + 10n)

const smallNum = Number(bigInt)
```

### 11.11 optional chain (ES11)

```javascript
const info = {
    name: 'rzh',
    friend: {
        name: 'lil',
        friend: {
            name: 'rs'
        }
    }
}
// 如果是undefined，直接就不执行代码了
console.log(info.friend?.friend?.name)
```

### 11.12 globalThis全局对象 (ES11)

在浏览器中毫无疑问是window，在node中是global

globalThis这个包可以自动识别environment

### 11.13 FinalizationRegistry (ES12)

可以监听对象的回收情况，注册对象

GC是不定时的回收对象的，所以即使设置为null，也不会立马打印"done"，需要等待GC进行回收

```javascript
// FinalizationRegistry
const finalization_registry = new FinalizationRegistry((val) => {
    console.log(val + ' is done')
})

let person = {
    name: '123'
}

finalization_registry.register(person, "person")

person = null
```

### 11.14 WeakRef (ES12)

弱引用，可以传入一个对象，创建一个弱引用

```javascript
const finalization_registry = new FinalizationRegistry(() => {
    console.log('done')
})

let person = {
    name: '123'
}
let person_1 = new WeakRef(person)

finalization_registry.register(person)

person = null
```

### 11.15 逻辑赋值运算 Logical Assignment (ES12)

```javascript
let message ||= "default"
let obj &&= obj.name

```

## 12. Proxy, Reflect and 响应式原理

### 12.1 Object.defineProperty

使用Object.defineProperty + forEach 给所有的属性添加set和get方法

```javascript
/**
 * Object.defineProperty
 */
var obj = {
    name: '123',
    age: 18
}

Object.keys(obj).forEach(key => {
    let val = obj[key]
    Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        get() {
            console.log('Get...')
            return val
        },
        set(new_val) {
            console.log('Set..')
            val = new_val
        }
    })
})

obj.name = 'r'
obj.age = 20
console.log(obj.name)
```

缺点：

- 无法监听新增和删除属性

### 12.2 Proxy类

Proxy是一个类，可以使用new Proxy创建代理对象

对原对象的操作，全部在代理对象上面完成（捕获器 trap）

```javascript
/**
 * Proxy
 */
const o_proxy = new Proxy(obj, {
    get(target, key) {
        console.log('get from Proxy')
        return target[key]
    },
    set(target, key, val) {
        console.log('set from Proxy')
        target[key] = val
    }
})
o_proxy.name = "rzh"

console.log(o_proxy.name)
console.log(obj.name)
```

Set有四个参数：

target, key, val, receiver

get有三个参数：

Target, key, receiver

其他的一些trap：

```javascript
// 监听in的trap
has(target, key) {
console.log('in')
return key in target
},
// 删除
deleteProperty(target, key) {
console.log('delete')
delete target[key]
}
// getPrototypeOf
// setPrototypeOf
// ownKeys() getOwnPropertyNames getOwnPropertySymbols
// apply 函数对象
// constructor 函数对象

function foo() {
    
}
const foo_proxy = new Proxy(foo, {
    apply(target, thisArg, argArray) {
        console.log('apply')
        target.apply(thisArg)
    },
    construct(target, argArray) {
        console.log('constructor')
        return new target(argArray)
    }
})
foo_proxy.apply(obj)
new foo_proxy()
```

### 12.3 Reflect对象

提供了很多操作javascript的方法，类似于object的一系列方法

Reflect.defineProperty => Object.defineProperty

Reflect.getPrototypeOf => Object.getPrototypeOf

为了减轻Object的负担，将很多方法迁移到Reflect上

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods

Reflect的方法和Proxy提供的方法是完全一致的，所以可以避免在Proxy内部直接操作原对象

```javascript
const obj = {
    name: 'rzh',
    age: 18
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        console.log('get')
        return Reflect.get(target, key, receiver)
    },
    set(target, key, val, receiver) {
        console.log('set')
        Reflect.set(target, key, val, receiver)
    }
})

objProxy.name = "ff"
console.log(objProxy.name)
console.log(obj)
```

recevier是代理对象，考虑一个场景：

```javascript
const obj = {
    _name: 'rzh',
    get name() {
        return this._name
    },
    set name(new_val) {
        this._name = new_val
    }
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        Reflect.get(target, key, receiver)
    },
    set(target, key, val, receiver) {
        Reflect.set(target, key, val)
    }
})
```

使用Reflect.get的时候，会进入原obj的getter内部，调用this._name. 这里的this是obj，所以直接对原对象的私有 _name进行了操作，绕过了Proxy的机制。

如果同时传递了recevier，receiver会修改getter中this的指向，指向为proxy对象，这样会再次进入到get函数中，对_name进行拦截并且执行side effect

### 12.4 **响应式原理 (reactivity)

1. 首先定义一个函数收集依赖，所有需要响应式的函数作为参数传入到这个函数中。

```javascript
function watchEffect(fn) {
	
}

fn()
```

2. 定义一个subscriber和publisher，用一个class来区分不同的属性

```javascript
class Dependency {
    constructor() {
        this.reactiveFns = []
    }

    addDependency(reactiveFn) {
        this.reactiveFns.push(reactiveFn)
    }

    notify() {
        this.reactiveFns.forEach(fn => {
            fn()
        })
    }
}
```

3. 监听对象的属性变化

```javascript
const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        return Reflect.get(target, key)
    },
    set(target, key, val, receiver) {
        Reflect.set(target, key, val)
        depend.notify()
    }
})
```

4. 对不同属性进行不同的dependency区分

```javascript
/**
 * 为不同的对象创建Map结构
 */
const dependency_map = new WeakMap()
function getDepend(target, key) {
    // get target obj
    let map = dependency_map.get(target)
    if (! map) {
        map = new Map()
        dependency_map.set(target, map)
    }
    // get dependency by key
    let dependency = map.get(key)
    if (! dependency) {
        dependency = new Dependency()
        map.set(key, dependency)
    }
    return dependency
}
```

5. 重新处理收集依赖的逻辑

```javascript
// 全局的reactiveFn对象可以让Proxy获取到fn
let activeReactiveFn = null
function watchFn(fn) {
    // 执行fn之后，到了get方法之后可以拿到depend
    activeReactiveFn = fn
    fn()
    activeReactiveFn = null
}

const objProxy = new Proxy(obj, {
    get(target, key, receiver) {
        const depend = getDepend(target, key)
        depend.addDependency(activeReactiveFn)
        return Reflect.get(target, key)
    },
```

6. 将普通的对象变成响应式的对象

```javascript
function reactive(obj) {
    return new Proxy(obj, {
        get(target, key, receiver) {
            const depend = getDepend(target, key)
            depend.addDependency(activeReactiveFn)
            return Reflect.get(target, key)
        },
        set(target, key, val, receiver) {
            Reflect.set(target, key, val)
            const depend = getDepend(target, key)
            depend.notify()
        }
    })
}
```

完整代码：

```javascript
class Dependency {
    constructor() {
        this.reactiveFns = []
    }

    addDependency(fn) {
        this.reactiveFns.push(fn)
    }

    notify() {
        this.reactiveFns.forEach((fn) => {
            fn()
        })
    }
}
const data_dependency = new WeakMap()
function getDepend(target, key) {
    let map = data_dependency.get(target)
    if (!map) {
        map = new Map()
        data_dependency.set(target, map)
    }
    let dependency = map.get(key)
    if (! dependency) {
        dependency = new Dependency()
        map.set(key, dependency)
    }
    return dependency
}
function reactive(obj) {
    return new Proxy(obj, {
        get(target, key, receiver) {
            const depend = getDepend(target, key)
            depend.addDependency(activeReactiveFn)
            return Reflect.get(target, key)
        },
        set(target, key, val, receiver) {
            Reflect.set(target, key, val)
            const depend = getDepend(target, key)
            depend.notify()
        }
    })
}
function reactive_by_property(obj) {
    Object.keys(obj).forEach(key => {
        let value = obj[key]
        Object.defineProperty(obj, key, {
            get() {
                const depend = getDepend(obj, key)
                depend.addDependency(activeReactiveFn)
                return value
            },
            set(new_val) {
                val = new_val
                const depend = getDepend(obj, key)
                depend.notify()
            }
        })
    })
    return obj
}
obj = {
    name: 'rzh',
    age: 18
}

const obj_proxy = reactive(obj)

let activeFn = null
function watchFn(fn) {
    activeFn = fn
    fn()
    activeFn = null
}

watchFn(function () {
    console.log(obj_proxy.name)
    console.log('hahaha')
})

watchFn(function () {
    console.log(obj_proxy.age)
    console.log('xixixi')
})

obj_proxy.name = 'foo'
obj_proxy.age = 20

/**
* output
*/
rzh
hahaha
18
xixixi
foo
hahaha
20
xixixi
```

## 13. **Promise Async function

### 13.1 类似Ajax的数据处理方式

类似ajax的网络请求

```javascript
function requestData(url) {
    setTimeout(() => {
        // 拿到请求的结果
        if (url == 'asd') {
            let nums = [1, 2, 3]
            return nums
        } else {
            let err = 'error'
            return err
        }
    }, 3000)
}

const res = requestData("asd")
```

拿不到res，因为return的值是在setTimeout这个函数中，并不是requestData这个函数的return值

ajax使用了callback function来解决

```javascript
function requestData(url, successCbFn, errorCbFn) {
    setTimeout(() => {
        // 拿到请求的结果
        if (url == 'asd') {
            let nums = [1, 2, 3]
            successCbFn(nums)
        } else {
            let err = 'error'
            errorCbFn(err)
        }
    }, 3000)
}

requestData("asd", (res) => {
    console.log(res)
}, (err) => {
    console.log(err)
})
```

缺点：

- 如果是第三方库，需要阅读source code，才能了解到requestData function的逻辑

### 13.2 Promise的改进方案

**Promise相当于自己封装了这一套逻辑，并且提供了standard的api来解决网络请求**

Promise接受回调函数，这个回调函数相当于在Promise的constructor中，所以会被立即执行，其中包含两个参数，resolve和reject

内部相当于：

```javascript
class Promise {
	constructor(callback) {
    	function resolve() {
        }
      	function reject() {
        }
      	
      	callback(resolve, reject)
    }
}
```

then方法传入的回调函数，会在Promise执行resolve函数时，被回调;

catch方法传入的回调函数，会在Promise执行reject函数时，被回调

```javascript
promise.then(() => {})
promise.catch(() => {})
```

使用Promise改进的代码

```javascript
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url == 'url') {
                const nums = [1, 2, 3]
                resolve(nums)
            } else {
                const errMessage = 'error'
                reject(errMessage)
            }
        }, 3000)
    })
}

const promise = requestData('url')
promise.then((nums) => {
    console.log(nums)
}).catch(err => {
    console.log(err)
})
```

### 13. 3 Promise的三个阶段

-- pending: request data

-- fulfilled: Get data

-- rejected: fail to get data

reject和resolve两个函数是互斥的，所以只能是fulfilled或者rejected两种状态

### 13.4 Promise的resolve参数

- Promise的resolve参数

如果resolve中的参数是一个Promise，那么当前的Promise的状态会由传入的Promise决定

传入的是一个对象，并且这个对象实现了then方法的话，那么会调用对象的then方法

### 13.5 Promise的then方法

那么其实Promise内部对于then和resolve的关联就应该是：

```javascript
class Promise {
	constructor(executor) {
    function resolve() {
      this.callback()
    }
    
    function reject() {
      
    }
    
    executor(resolve, reject)
  }
  
  then(callback) {
    this.callback = callback
  }
}
```

使用一个变量将callback保存，到resolve方法中执行

**如果then方法返回一个值，这个值会被作为参数传递到一个新的Promise的resolve中作为回调**

```javascript
new Promise((resolve, reject) => {
    resolve()
}).then(res => {
    return 'aaa'
}).then(res => {
    console.log(res) // aaa
})

// promise 链式调用，返回promise就代替之前的promise
new Promise((resolve, reject) => {
    resolve()
}).then(res => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('hello world')
        }, 3000)
    })
}).then(res => {
    console.log(res) // hello world
})
```

### 13.6 Promise的catch方法

catch方法可以捕获reject和throw出来的Error

```javascript
new Promise((resolve, reject) => {
  return new Promise((resolve, reject) => {
    reject('error')
    // throw new Error('123')
  })
}).catch(res => {
  console.log(res)
})

// 在catch中返回一个值，会new promise中调用resolve，所以还是会调用then方法
new Promise((resolve, reject) => {
  reject('111')
}).catch(res => {
  return 'new error'
}).then(res => {
  console.log(res)
}).catch(res => {
  console.log(res)
})
```

### 13.7 Promise的finally方法

finally方法在Promise调用完之后执行一些清理工作

### 13.8 Promise的类方法

- resolve：直接调用Promise.resolve相当于new Promise((resolve, reject) => resolve())

- Reject: 直接调用Promise.reject相当于new Promise((resolve, reject) => reject())

- All: 可以创建多个Promise，当所有Promise对象执行结束后，得到结果集合。

如果其中一个promise被reject了，那么所有的promise会被reject，不执行

```javascript
Promise.all([p1, p2, p3]).then(res => {console.log(res)})
```

- allSettled: 会返回一个数组，里面每个对象包含status和res，不会受reject影响

```js
[
	{status: 'rejected', reason: 'network err'},
  {status: 'fulfilled', value: 'success'}
]
```

- Race: 只要有一个promise对象fulfilled，那么就直接终止Promise

- Any: 至少要等到一个promise fulfilled，才会终止Promise，不会受reject的影响

|            | 都会调用 | 受reject影响 |
| ---------- | -------- | ------------ |
| All        | 是       | 是           |
| allSettled | 是       | 否           |
| Race       | 否       | 是           |
| Any        | 否       | 否           |

### 13.9 Promise的简单实现

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'
class MyPromise {
    constructor(fn) {
        this.status = PENDING
        this.value = undefined
        this.reason = undefined
        const resolve = (value) => {
            /**
             * setTimeout是宏任务，会在所有代码执行后，才会执行
             */
            setTimeout(() => {
                if (this.status == PENDING) {
                    this.status = FULFILLED
                    this.value = value
                    console.log('resolve...')
                    this.onFulfilled(value)
                }
            }, 0)     
        }

        const reject = (reason) => {
            queueMicrotask(() => {
                if (this.status == PENDING) {
                    this.status = REJECTED
                    this.reason = reason
                    console.log('reject...')
                    this.onRejected(reason)
                }
            })         
        }
        fn(resolve, reject)
    }
    then(onFulfilled, onRejected) {
        this.onFulfilled = onFulfilled
        this.onRejected = onRejected
    }
}

const promise = new MyPromise((resolve, reject) => {
    console.log('pending...')
    resolve(11)
    reject(22)
})

promise.then(res => {
    console.log(res)
}, err => {
    console.log(err)
})
```

改进1:保证多个promise调用也能使用，将onFilfilled和onRejected放入数组中，遍历执行

```
this.onFulfilledFns和this.onRejectedFns
```

## 14. Iterator和generator

### 14.1 迭代器

Iterator:在容器对象中遍历所有的数据（链表，数组等），不需要关心容器的内部结构

迭代器定义了数据生成的方式，在javascript中是next函数

next是一个无参数的函数，返回两个值：

- done： 如果是最后一个，为true，否则为false
- value：done为true时会忽略，否则为任意一个值

```javascript
const names = ['asd', 'd', 'vvf', 'zc']
let index = 0
const name_iterator = {
    next() {
        if (index < names.length) {
            return {
                done: false,
                value: names[index ++]
            }
        } else {
            return {
                done: true,
                value: undefined
            }
        }
    }
}

console.log(name_iterator.next())
console.log(name_iterator.next())
console.log(name_iterator.next())
console.log(name_iterator.next())
console.log(name_iterator.next())
console.log(name_iterator.next())

// result
{ done: false, value: 'asd' }
{ done: false, value: 'd' }
{ done: false, value: 'vvf' }
{ done: false, value: 'zc' }
{ done: true, value: undefined }
{ done: true, value: undefined }
```

将迭代器进行封装

```javascript
function createArrayIterator(arr) {
    let idx = 0
    return {
        next: function() {
            return {
                done: idx < arr.length? false: true,
                value: arr[idx++]
            }
        }
    }
}

console.log(createArrayIterator(names).next())
```

无限迭代器：

如果done一直是false的话，那么就是无限的迭代器

### 14.2 可迭代对象

当一个对象实现了iteratable protocol，就是可迭代函数（next function）

实现Symbol.iterator访问实现的@@iterator属性

```javascript
const iterableObj = {
    nums: [1, 2, 3],
    [Symbol.iterator]: function () {
        let index = 0
        return {
            next: () => {
                return {
                    done: index < this.nums.length ? false: true,
                    value: index < this.nums.length ? this.nums[index++]: undefined
                }
            }
        }
    }
}

const iterator = iterableObj[Symbol.iterator]()
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
```

使用for...of进行遍历，要求对象必须是可迭代对象，本质上是使用iterator.next()，然后取值value

### 14.3 内置的可迭代对象

javascript内置的array，Map，set， arguments都是iterator对象

```javascript
const foos = ['abc', 'cba', 'nba']
foo_iterator = foos[Symbol.iterator]()
console.log(foo_iterator.next())
```

### 14.4 给Class创建迭代器

```javascript
/**
 * 使用场景
 */
class Classroom {
    constructor(location, name, students) {
        this.location = location
        this.name = name
        this.students = students
    }

    entry(new_stu) {
        this.students.push(new_stu) 
    }

    [Symbol.iterator]() {
        let idx = 0
        return {
            next: () => {
                if (idx < this.students.length) {
                    return {done: false, value: this.students[idx ++]}
                } else {
                    return {done: true, value: undefined}
                }
            },
            return() {
                console.log('finish...')
                return {done: true, value: undefined}
            }
        }
    }
}

var classroom = new Classroom("east", "ll", [
    'rts',
    'asd',
    'thgt'
])

for (const item of classroom) {
    console.log(item)
    if (item === 'asd') break;
}

```

使用return可以监听迭代器的提前终止

### 14.5 Generator 生成器

生成器能够对函数进行精准的控制，例如暂停和运行

生成器函数：

- 在function后面加上*
- 生成器函数中可以使用yield关键字来控制函数的执行流程
- 返回一个生成器(特殊的迭代器，也有next方法)

执行生成器函数，返回了一个生成器对象，使用next function可以遍历。

generator的返回值是最后一次迭代的迭代器返回对象的value

如果在最后一次iteration之前return的话，会直接终止function

如果想在某次iteration之后返回值可以 yield value

### 14.6 生成器传参数

每个iteration的参数都是上一个iteration的返回值的value

```javascript
function* _generator() {
    console.log('start...')
    console.log('111')
  	// 这里yield的value会变成10
    const n = yield 
    console.log(100 * n)
    console.log('222')
    yield 
    console.log('end...')
}

const generator = _generator()
generator.next()
generator.next(10)
generator.next()
```

### 14.7 生成器return和throw方法

如果直接调用generator.return，会直接终止代码

如果调用throw方法的话，会抛出异常

### 14.8 yield*

会把所有的可迭代对象都进行yield，类似yield* array

```javascript
class _Classroom {
    constructor(name, students) {
        this.name = name
        this.students = students
    }

    entry(student) {
        this.students.push(student)
    }

    [Symbol.iterator] = function* () {
        yield* this.students
    }
}

const class_generator = new _Classroom('123', [1, 2, 3])
for (item of class_generator) {
    console.log(item)
}
```

### 14.9 **使用generator解决回调地狱 & async await

```js
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(url)
        }, 1000)
    })
}

requestData('user').then(res => {
    requestData(res + '111').then(res => {
        requestData(res + '222').then(res => {
            console.log(res)
        })
    })
})
```

```javascript
function* getData() {
    const res1 = yield requestData('user')
    const res2 = yield requestData(res1 + 'bbb')
    const res3 = yield requestData(res2 + 'ccc')
    console.log(res3)
}

function exec_generators(genFn) {
    const generator = genFn()

    function exec(res) {
        // 得到当前一次yield的结果
        const result = generator.next(res)
        // 如果done是true，说明结束了，返回result
        // 如果done是false，说明还没结束，将res传入重新执行requestData
        if(result.done) {
            return result.value
        } else {
            result.value.then(res => {
                exec(res)
            })
        }
    }

    exec()
}

exec_generators(getData)
```

generator和Promise的语法糖是async和await

## 15. async await 事件循环

### 15.1 async中是普通的代码

async 如果内部都是普通的代码，还是同步的执行所有的逻辑

```javascript
async foo() {
	console.log('111')
}

console.log('start') // start
foo() // 111
console.log('end') // end
```

async函数默认的返回值是一个Promise，return的值会作为Promise对象的参数进行传值

```javascript
async foo() {
	console.log('foo')
}

const promise = foo()
promise.then(res => {
	console.log(res) // undefined
})
```

也可以传Promise或者thenable Object

### 15.2 async中有异常代码

会把异常作为Promise中的reject的值

```javascript
async foo() {
	throw new Error('123')
}

foo().catch(err => console.log(err))
// 不会阻塞代码
console.log('start')
```

### 15.3 async 中使用await

await 返回值（Promise对象）

结果是Promise执行了resolve之后得到的数据

如果await一直没有resolve，那么后续代码都不会执行。

本质上是一个**回调地狱**

```javascript
async function foo() {
	await requestData()
	await requestData2()
	console.log(res)
	=>
	requestData().then(res => {
    ...
		requestData2().then(res => {
      ...
			console.log('res')
		})
	})
}
```



### 15.4 Process and Thread

Process: 计算机已经运行的程序，是操作系统管理程序的一部分

Thread: 操作系统能够调度的最小单位，包含在Process中。在一个Process中至少需要一个Thread来执行程序，这个Thread叫做main Thread

javascript是单线程的，进程容器是node或者浏览器

如果一个事件非常耗时，那么javascript的线程就会被阻塞

比如像定时器和网络请求，由浏览器的其他线程来存储callback function，到特定的时刻进行调用

如何存储事件：事件队列（event queue），到需要执行的时候出列进行执行

事件循环（event loop）：js线程 =》第三方线程 =〉事件队列 =》js线程

### 15.5 MacroTask and MicroTask

宏任务队列：定时器，DOM，ajax

微任务队列：queueMicroTask，Promise.then，MutationObserver API

在执行宏任务之前，会要求先把微任务全部清空（微任务的优先级大于宏任务）

### 15.6 Node事件循环

Node中用libuv实现的，主要维护了一个EventLoop和worker Threads（线程池），EventLoop负责调用系统的一些其他操作：I/O，Network...

事件循环就像一个桥梁，连接着javascript和系统调用之间的通道

无论是文件I/O，数据库，定时器等一系列javascrit无法承受的操作，都会交给libuv，放入到事件循环队列中，之后从任务队列中不断拿出事件进行调用

一次完整的事件循环（Tick）阶段：

1. 定时器阶段，本阶段执行SetTimeout和setInterval等
2. 待定回调（pending callback）：TCP连接等，出现错误
3. idle，prepare: node内部
4. 事件轮询（poll）：检索I/O事件，执行I/O回调(容易阻塞，因为事件轮询很多操作立即执行)
5. 检测（Check）：setImmediate(长时间运行的操作，当其他函数全部执行完，再执行这个函数)
6. 关闭回调：Socket.close（）

宏任务：setTimeout，setInterval，IO，setImmediate

微任务：process.nextTick，QueueMicroTask，Promise.then

**在任何宏任务阶段执行之前，都会把微任务结束**

### 15.6 面试题

```javascript
/**
* 不会马上执行，因为在宏队列
*/
setTimeout(() => {
    console.log('settimeout1')

    new Promise(function (resolve) {
      	// 加入到microTask
        resolve()
    }).then(function () {
        new Promise(function (resolve) {
          	// 加入到microTask
            resolve()
        }).then(function () {
            console.log('then4')
        })
        console.log('then2')
    })
})

new Promise(function (resolve) {
  	// 立即执行
    console.log('promise1')
  	// 调用then，加入microQueue
    resolve()
}).then(function () {
    console.log('then1')
})

/**
* 加入到宏任务中
*/
setTimeout(() => {
    console.log('setTimeout2')
})

// 立即执行
console.log(2)

/**
* 加入到MicroQueue
*/
queueMicrotask(() => {
    console.log('queueMicroTask1')
})

/**
* 立即执行
* then加入到MicroQueue
*/
new Promise(function (resolve) {
    resolve()
}).then(function () {
    console.log('then3')
})

// promise1 2 then1 queueMicroTask1 then3 settimeout1 then2 then4 settimeout2
```

```javascript
async function async1() {
    console.log('async1 start')
    await async2()
    console.log('async1 end')
}

async function async2() {
    console.log('async2')
}

console.log('script start')

setTimeout(() => {
    console.log('setTimeout')
}, 0)

async1()

new Promise(function (resolve) {
    console.log('promise1')
    resolve()
}).then(function () {
    console.log('promise2')
})
console.log('script end')

// script start
// async1 start
// async2 
** async2 之后就会返回一个 New Promise(resolve => resolve())，所以之后的async1 end默认全在thenable里面，所以加入到microTask
// promise1 
// script end
// async1 end 
// promise2 
// settimeout
```

```javascript
Promise.resolve().then(() => {
	console.log(0)
	return Promise.resolve(4)
}).then(res => {
	console.log(res)
})

Promise.resolve().then(() => {
	console.log(1)
}).then(() => {
	console.log(2)
}).then(() => {
	console.log(3)
}).then(() => {
	console.log(5)
}).then(() => {
	console.log(6)
}) 

// return 普通的基本数据类型，那么是直接加入到microTaskQueue中
// return 一个thenable对象，那么会多加一层microTask，等于有两个MicroTask，再加入到MicroTaskQUeue中
// return 一个Promise，那么会多加两层微任务（因为不是基本数据类型，多加一层，因为要then，所以再加一层）
```

```javascript
async function async1() {
	console.log('async1 start')
  await async2()
  console.log('async1 end')
}

async function async2() {
  console.log('async2')
}

console.log('script start')

setTimeout(() => {
  console.log('setTimeout0')
}, 0)

// 300ms之后加入MicroTask
setTimeout(() => {
  console.log('setTimeout2')
}, 300)

setImmediate(() => {
  console.log('setImmediate')
})

// 在所有的microTask中优先执行
process.nextTick(() => console.log('nextTick1'))

async1()

process.nextTick(() => console.log('nextTick2'))

new Promise(function (resolve) {
  console.log('promise1')
  resolve()
  console.log('promise2')
}).then(function () {
  console.log('promise3')
})

console.log('script end')

/**
* script start
* async1 start
* async2 
* promise1 
* promise2
* script end 
* nexttick1
* nexttick2
* async1 end
* promise3
* setTimeout0
* setImmediate
* setTimeout2
*/
```

## 16. Error Handling

### 16.1 Throw Error

当代码抛出异常后，后续代码就不能继续执行

```js
class MyError {
    constructor(err_code, err_msg) {
        this.err_code = err_code
        this.err_msg = err_msg
    }
}
```

默认的Error类，new Error(message: string)

会抛出异常和详细的函数的调用栈

一些子类

- RangeError
- TypeError
- SyntaxError

处理异常的方案：

- 继续向函数调用栈的上层抛出异常
- try catch block来捕获异常,finally的代码一定会执行

## 17. 模块化开发

将代码划分成为不同的小的结构，每个结构有属于自己的代码逻辑，有自己的作用域，同时可以将内部的变量，函数方法等导出

不同文件之间的变量冲突？

使用立即执行函数包裹代码，避免变量冲突。同时将变量return出去，然后拿到变量

```js
// a.js
var moduleA = (function() {
    var flag = true
    var name = "123"

    return {
        flag,
        name
    }
})()

// b.js
var moduleB = (function () {
    var flag = false
    var name = "fff"

    return {
        flag,
        name
    }
})()

if (moduleA.flag) {
    console.log(moduleA.name)
}
```

### 17.1 Commonjs 规范

Commonjs核心关键词 Module.exports,exports, require

导入导出的原理：因为导出的是对象，是引用数据类型，那么就有地址。module.exports = obj，等于module的exports属性指向了obj，同时require的时候通过os找到module.exports也可以找到这个对象。同时指向这个地址。

```javascript
module.exports = {}
exports = module.exports
export.name = name

const name = require('...')
```

如果exports = {name, age}，会有问题，因为{name, age}是一个新的对象，会导致module.exports和exports脱钩

Require的查找规则：

- require node的一个核心模块，如path，fs等，会返回对应的对象
- require 当前project的文件，如果没有后缀名，那么查找的顺序是js》json〉node，如果找不到，就找的是path/index.js>path/index.json>path/index.node

模块在第一次加载时，会被运行一次，多次引用之后会被缓存，每个module都有一个属性loaded，一旦被引用之后loaded为true

Node如果循环引用的话，那么会用深度优先搜索。

Commonjs的缺点

- commonjs同步加载，容易阻塞Dom的操作
- Webpack中或者server中使用是没有问题的

### 17.2 ES Module

#### 17.2.1 import和export关键字

- import 和 export的关键字
- 编译器静态分析，也可以动态引用
- 自动使用了use strict 严格模式

export的导出形式

```javascript
export const name = 'rzh'

const name = 'rzh'
export {
	name
}

export {
	name as myname
}
```

import的导入形式

```javascript
import {name} from '..'

import {name as myname} from '..'

import * as myname from '..'
```

特殊的一些导入导出方式

```javascript
export {name, age} from '@/path'

export * as obj from '@/path'

/**
* 默认导出只能存在一个
*/
export {
	name,
  age,
  foo as default
}

export default foo
```

上面所有的代码都是同步执行的

#### 17.2.2 import函数

import函数返回的结果是一个Promise，内部会调用resolve函数。使用import('').then(res => )，res是拿到的导入的对象.

会进行异步的原因是下载js文件是在浏览器的thread中进行的，但是编译阶段需要由js来执行。直接使用import关键字相当于执行了下载+编译，那么必须是同步执行代码流程的。但是import函数封装了promise，先执行下载，然后到需要使用的时候在调用then方法进行编译。

import的meta属性，包含当前文件的url

#### 17.2.3 ES module的原理

1. 构建（Construction）：根据地址，查找js文件，下载，解析成为module record（静态分析）
2. 实例（Instantiation）：对module record进行实例化，然后分配内存空间，将模块指向对应的内存地址。
3. 验证（Evaluation）：运行代码，计算值，填充到内存地址中

所以说模块化的相关代码是优先运行的，分配空间地址，值为undefined，之后运行main script，更新值

构建阶段只会分析静态代码，所以import语句声明最好全部放在最上方

导入和导出都会绑定到module record的实例，当运行代码更新module record之后，导入的值也会进行相应的变化。**mention：单向数据流，import 进来的object不能随便修改值，被lock住了**

### 17.3 Commonjs和EsModule相互引用

使用commonjs导出，使用ESModule导入

- 在浏览器中是不能识别Commonjs的，所以相互引用在浏览器中是不行
- 在node环境中，需要区分node的版本，有些node版本不支持esmodule
- Webpack中EsModule和Commonjs都是可以使用的，因为webpack会用babel最后转化成ES5的代码

## 18. 包管理工具：npm, yarn, npx

### 18.1 Npm

Npm publish to registry, then npm install the corresponding packages.

Npm: Node package manager

根据package.json中的dependencies和devDependencies进行第三方库的安装

PeerDependencies:和这些库同等依赖，最好一起安装，如React+React-Dom+antDesign

Semver 版本规范X.Y.Z

X: 做了可能不兼容之前API的修改

Y: 新功能的增加，同时向下兼容

Z: 没有新功能，只是Fix了部分bugs

Npm install 下载的是压缩包，里面有配置文件，每次下载之前会比对一下版本，如果版本号相同，就会直接解压package

package-lock.json 检测依赖的一致性，查找缓存

npm cache clean 清除缓存

### 18.2 Yarn

Yarn是npm的代替工具，fb推出的

命令和npm有些不一样，其他都差不多

### 18.3 Npx

常用的是调用项目中的模块，比如全局和局部都有webpack(3/4)，直接使用webpack会查找到全局的webpack

```javascript
webpack --version 3

npx webpack --version 4
```

## 19. JSON

JSON是一种数据格式，帮助数据在前端和后端之间进行传输（javascript object notation)

- 网络传输数据
- 项目的配置文件
- NoSql将json作为存储格式

JSON数据的形式

- Object
- Array
- 基本数据值

JSON序列化

例如Object转字符串 变成了"Object obejct"

JSON.stringfy + JSON.parse, stringfy可以加replacer参数，设置哪些内容需要转化

如果对象中含有一个toJSON方法，那么会直接调用toJSON的方法进行序列化

利用JSON进行深拷贝

# React

## 1. Redux

redux的核心概念：store, action, reducer

store：将所有的state存储到一个独立的空间中，方便track state

action：dispatch action，action是一个对象，type和content

reducer：处理完action之后，更新store中的state

原则：

1. 单一的数据源，保证只有一个store
2. state是read-only的，只能通过action触发修改state，不需要考虑race condition
3. 所有的reducer函数都必须要是纯函数

```
通过源代码，我们发现，`var nextStateForKey = reducer(previousStateForKey, action)`, \**nextStateForKey\**就是通过 reducer 执行后返回的结果(state)，然后通过`hasChanged = hasChanged || nextStateForKey !== previousStateForKey`来比较新旧两个对象是否一致，此比较法，比较的是两个对象的存储位置，也就是浅比较法，所以，当我们 reducer 直接返回旧的 state 对象时，Redux 认为没有任何改变，从而导致页面没有更新.
```

所以在reducer中必须返回一个新的对象，这是为了节省javascript的性能，不然每次都要深拷贝。

如何在react中使用redux？

1. 在useEffect中使用subscribe，对数据进行订阅
2. 使用高阶函数把redux的相关代码抽离出去，并且混入进不同的component

Redux的异步操作：

在请求和响应之间加入一些代码，被称为中间件，在redux中反映为action和reducer

Redux-thunk: 让dispatch也可以传入函数，调用函数

```javascript
// App.js
dispatch(getData)
// actionCreators.js
const getData = (dispatch, getState) => {
	axios.get('').then(res => {
		dispatch(changeData(res.data))
	})
}
```

Redux-Thunk的实现：Monkey-patch

```javascript
function thunkPatch(store) {
	const next = store.dispatch
	function dispatch_thunk(action) {
		if (typeof action === "function") {
			action(store.dispatch, store.dispatch)
		} else {
			next(action)
		}
	}
	store.dispatch = dispatch_thunk
}
```

# VUE

## VueX

state的管理方式：

- setUp中的响应式数据(Ref和reactive)，通过props或者context传递数据（层级太深，如何维护sibling nodes？）
- Redux 进行状态管理

Vuex的核心要素：

- state：在VueX状态管理仓库里面保存的state($store.state.xx)
- Action：异步请求的数据由action完成 （redux-thunk）
- Mutation：提交修改，修改store中的状态

mutation如果进行异步操作，没法及时的生成snapshot，提交到dev-tools进行状态追踪。

单一状态树（single source of truth）：用一个对象来包含所有应用层级别的状态，仅仅使用一个store，但不同模块可以进行抽离

mapState，mapSetters，mapMutations，mapActions可以将$store.state在setup中进行映射

```javascript
import { createStore } from 'vuex'

export default createStore({
  state() {
    return {
      counter: 0,
      name: 'name',
      age: 17,
      books: [
        { price: 1, count: 2 },
        { price: 2, count: 1 },
        { price: 3, count: 6 }
      ]
    }
  },
  getters: {
    totalPrice(state) {
      return state.books.reduce((pre, cur) =>
        pre + cur.price * cur.count
        , 0)
    },
    totalPriceGreaterN(state) {
      return function (n) {
        if (state.books.length > n) {
          return state.books.reduce((pre, cur) =>
            pre + cur.price * cur.count
            , 0)
        } else {
          return 'No'
        }
      }
    }
  },
  mutations: {
    increment(state) {
      state.counter++
    },
    decrement(state) {
      state.counter--
    },
    incrementN(state, num) {
      state.counter += num
    }
  },
  actions: {
    asyncFn(context, num) {
      console.log(num)
      setTimeout(() => {
        context.commit('incrementN', num)
      }, 1000)    
    }
  },
  modules: {
  }
})

```

不同module的命名空间：module_name/action(state, ...)

## Vue的nextTick原理

vue的组件更新等其他操作都是microTask，为了让一些操作在update之后执行，可以用nexttick将部分代码也加入microtaskqueue，让它延迟执行。（一个tick之后执行）

# Typescript

对传入参数和返回值类型进行类型检测，javascript代码本身存在安全的隐患。所以要引入一种类型检测机制来规范代码

webpack+babel进行转换代码，或者tcs(typescript compiler system)进行代码转化

string,number对应的是typescript的数据类型，String,Number对应的是javascript中的wrapper class

类型推断，即使没有声明类型，typescript也会根据声明的值进行相应的推断

数据类型：

- number
- string
- Array[string] 容易和jsx语法冲突，string[]
- object （最好不要用）
- null （只针对null这一个关键字）
- undefined
- Symbol类型
- any
- unknown：类型不确定，unknown只能赋值给any和unknown类型，any可以赋值给任意类型
- void：指定一个函数没有返回值
- never：永远不会出现的值，死循环或者抛出异常

```typescript
function handleMessage(message: number | string | boolean) {
	switch (typeof message) {
		case "string":
			...
		case "number":
			...
		default:
			const check: never = message
	}
}
```

- Tuple: 多种不同数据类型的组合，是array类型的一种优化

```typescript
const _tup: [number, string, number] = [1, '22', 2]

console.log(typeof _tup[0])
```

- 函数类型：() => void

  上下文的函数可以不加类型注解，类似map, forEach, filter等高阶函数

- 对象类型：{x: number, y:number}

可选类型，?x，代表x这个参数可以不传

联合数据类型 string | number | boolean

类型断言(相当于父子类类型转换)：有的时候无法获取到非常具体的数据类型，那么就可以进行断言

```typescript
const el = document.getElementById("img") as HTMLImgElement
```

非空类型断言：在变量后面加！

可选链：?. 对象属性不存在，直接返回undefined，如果有值，才会运行代码

```typescript
info.friend?.age
info.friend?.name
```

!!: 将其他的类型转换成boolean类型

??: 左侧是undefined或者null时，返回右侧具体的值

字面量类型，当variable是一个constant，那么他就直接是一个字面量，即类型和值是一致的（结合联合类型）

```typescript
let direction: 'left': 'right': 'up' = 'left'
```

类型缩小（type narrowing) 类型保护（type guarding)：typeof / instanceof / in / ===

in：某个属性是否在某个字面量对象中

函数类型：

```typescript
// 如果是可选参数，那么需要放在必选参数的后面
// 使用默认值，可以传入undefined
const add: (num1: number, num2: number) => number = (num1: number, num2: number) => {
    return num1 + num2
}
```

函数的overload

不同的函数只定义，不实现，后面使用一个函数提供具体的实现

如果联合类型和overload都可以实现，那么使用联合类型

```typescript
function add(num1: string, num2: string): number

function add(num1: number, num2: number): string

function add(num1: any, num2: any) {
    return num1 + num2
}

console.log('123', '22')
```

类

类的继承和多态

修饰符：public，protected（内部和子类能够访问），private

readonly：属性只能访问，不能修改，只能在构造器里面赋值

setters/getters: set name(newName) {this._name = newName}

```typescript
// inheritance
class Person {
    name: string
    age: number

    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }

    eating() {
        console.log(this.name + " is eating...")
    }

}

const p = new Person("rzh", 18)
p.eating()

class Student extends Person {
    sno: number
    constructor(name: string, age: number, sno: number) {
        super(name, age)
        this.sno = sno
    }

    eating(): void {
        super.eating()
        console.log(`student ${this.name} eating...`)
    }

    studying() {
        console.log(this.name + ' is studying...')
    }
}

// 多态
// 父类Animal指向了子类对象new Dog()
Animal dog = new Dog()

```

抽象类

```typescript
// 抽象类不能被实例化，且抽象方法必须被子类实现
abstract class Shape {
    abstract getArea(): number
}

class Rectangle extends Shape {
    private x: number
    private y: number
    constructor(x: number, y: number) {
        super()
        this.x = x
        this.y = y
    }

    getArea(): number {
        return this.x * this.y
    }
}
```

类可以作为变量的类型，必须有对应的属性和方法

Interface

- 可以作为一个类型声明
- 可以作为索引类型
- 可以作为函数
- 可以定义同样名称的接口，同时能把interfaces合并

intersaction type: x & y，同时具备x和y的所有特性

接口和abstract class最大的差别：一个是多继承，一个是单继承

枚举类型

```typescript
// 默认值是0，1，2...递增的
Direction {
	LEFT,
	RIGHT,
	UP,
	DOWN
}

Direction.LEFT
```

泛型

将类型参数化，让调用者决定具体的类型

```typescript
function sum<T, E> (num: T, num2: E) {
	return num1 + num2
}

sum<number, string>(10, '2')
```

命名空间 namespace

```typescript
namespace time {
	export function format() {
		return '2020:03'
	}
}

namespace math {
	export function format() {
		return 123.123
	}
}

math.formart()
```

类型查找：

内置类型声明 例如document等 lib.dom.d.ts

第三方库的类型声明文件，需要下载对应的d.ts文件

