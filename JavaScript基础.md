# JavaScript语言
概念：是一门客户端脚本语言
    * 运行到客户端浏览器中，每一个浏览器都有JavaScript的解析引擎<br>
脚本语言：不需要编译，直接就可以被浏览器解析执行<br>
功能：可以用来增强用户和html页面的交互过程，可以用来控制html元素，让页面有一些动态效果，增强用户体验<br>
## JavaScript = ECMAScript + JavaScript特有的(BOM + DOM)
### ECMAScript：客户端脚本语言的标准
    * 与html结合的方式：
        * 内部JS代码：定义<script>，标签体内容就是js代码
        * 外部JS代码：定义<script>，通过src属性引入外部的JS文件
        * 注意：
            * <script>可以定义在html页面的任何地方，但是定义的位置会影响执行顺序
            * <script>可以定义多个
    
    * 数据类型：
        * 原始数据类型：
            * number：数字，包含了整数、小数和NaN(not a number：不是一个数字的数字类型)
            * string：字符串，例如："abc","a",'abc'都算是字符串
            * boolean：布尔类型，只有true和false两个值
            * null：一个对象为空的占位符
            * undefined：未定义，如果一个变量没有给初始化值，则会被默认赋值为undefined
        * 引用数据类型：对象
    
    * 变量：一小块存储数据的内存空间
        * JavaScript是弱类型语言，Java是强类型语言
            * 强类型：在开辟变量存储空间时，定义了空间将来存储的数据的数据类型，只能存储固定的数据类型的数据
            * 弱类型：在开辟变量存储空间时，不定义空间将来存储的数据类型，可以存放任意类型的数据
        * 语法：var 变量名 = 初始化值;
    
    * JavaScript中运算“/”时，会直接算出小数部分
    
    * ===符号：全等于，在比较之前，先判断类型，如果类型不一样，则直接返回false
    
    * 其他类型转boolean：
        * number：0或NaN为false，其他都为true
        * string：空字符串为false，其他都为true
        * null和undefined都为false
        * 对象：所有对象都为true
    
    * alert(值)方法：表示弹出一个对话框，只有确定按钮
    
    * confirm(值)方法：表示弹出一个对话框，有确定和取消两个按钮。确定返回true，取消返回false
    
    * console.log(值)方法：表示在控制台输出对应值

## JS的对象
* Array对象用于定义数组
    * 格式：
        * var 变量名 = new Array(元素列表); 例如： var arr = new Array(1,2,3,4);
        * var 变量名 = [元素列表]; 例如： var arr = [1,2,3,4];
    * 访问：arr[索引] = 值;
    * 特点：长度可变，类型可变(一个数组中可以存储不同类型的数据)；
    * 常用属性：length：设置或返回数组中元素的数量；
    * 常用方法：
        * forEach()：遍历数组中的每个有值的元素，并调用一次传入的函数；
        * push()：将新元素添加到数组的末尾，并返回新的长度；
        * splice()：从数组中删除元素；
* String字符串对象
    * 格式：
        * var 变量名 = new String("..."); 与Java一样
        * var 变量名 = "...";或 var 变量名 = '...'; 与Java一样
    * 常用属性：length：获取字符串的长度
    * 常用方法：
        * charAt()：返回在指定位置的字符
        * indexOf()：检索字符串
        * trim()：去除字符串两边的空格
        * substring()：提取字符串中两个指定的索引号之间的字符
* 自定义对象
    * 定义格式：
        <pre>
            var 对象名 = {
                属性名1:属性值1,
                属性名2:属性值2,
                属性名3:属性值3,
                函数名称:function(形参列表){}
            };
        </pre>
    * 调用格式：
        * 对象名.属性名;
        * 对象名.函数名();
* JSON字符串介绍
    * 概念：JavaScript Object Notation，JavaScript对象标记法
    * JSON是通过JavScript对象标记法书写的文本
    * 其语法简单，层次结构鲜明，现多用于作为数据载体，在网络中进行数据传输
    * 定义格式：var 变量名 = '{"key1":value1,"key2":value2}'; 例如：var userStr = '{"name":"Jerry","age":18,"addr":["北京","上海","西安"]}';
    * JSON字符串转为JS对象：var jsObject = JSON.parse(userStr);
    * JS对象转为JSON字符串：var jsonStr = JSON.stringify(jsObject);
* BOM对象
    * 概念：Browser Object Model(浏览器对象模型)，允许JS与浏览器对话，JS将浏览器的各个组成部分封装为对象
    * 组成：
        * window：浏览器窗口对象
            * 使用方法：直接使用window，其中“window.”可以省略。如：window.alert();  =>  alert();
            * 常用属性，以下几个属性都是用来获取其他BOM对象的：
                * history：对History对象的只读引用
                * location：用于窗口或框架的Location对象
                * navigator：对Navigator对象的制度引用
            * 常用方法：
                * alert()：显示带有一段消息和一个确认按钮的警告框
                * confirm()：显示带有一段消息以及确认按钮和取消按钮的对话框
                * setInterval(function,值)：按照指定的周期(以毫秒计)来调用函数或计算表达式
                * setTimeout(function,值)：在指定的毫秒数后调用函数或计算表达式
        * Navigator：浏览器对象
        * Screen：屏幕对象
        * History：历史记录对象
        * Location：地址栏对象
            * 获取：使用window.location获取，其中“window.”可以省略
            * 常用属性：herf：设置或返回完整的URL。例如：location.href="https://www.itcast.cn";，当设置改URL时，会自动跳转到改URL
* DOM对象
    * 概念：Document Object Model，文档对象模型
    * 将标记语言的各个组成部分封装为对应的对象：
        * Document：整个文档对象，通过该对象获取出来其他对象，可以通过<span style="color:red;">参考文档</span>去查看其他属性即方法，怎样使用
            * 根据id属性值获取，返回单个Element对象：var h1 = document.getElementById('h1');
            * 根据标签名称获取，返回Element对象数组：var divs = document.getElementsByTagName('div');
            * 根据name属性值获取，返回Element对象数组：var hobbys = document.getElementsByName('hobby');
            * 根据class属性值获取，返回Element对象数组：var clss = document.getElementsByClassName('cls');
        * Element：元素对象
        * Attribute：属性对象
        * Text：文本对象
        * Comment：注释对象
    * JS通过DOM，就能对HTML进行操作：
        * 改变HTML元素的内容
        * 改变HTML元素的样式
        * 对HTML DOM事件做出反应
        * 添加和删除HTML元素

## 事件监听
* 事件：HTML事件是发生在HTML元素上的“事情”。例如：按钮被点击；鼠标移动到元素上；按下键盘按键等
* 事件监听：JS可以在事件被侦测到时执行代码
* 事件绑定：
    * 方式一：通过在HTML标签中的事件属性进行绑定，例如：
        <pre>
            &lt;input type="button" onclick="on()" value="按钮1"&gt;
            &lt;script&gt;
                function on(){
                    alert('我被点击了！');
                }
            &lt;/script&gt;
        </pre>
    * 方式二：通过DOM元素属性绑定，例如：
        <pre>
            &lt;input type="button" id="btn" value="按钮2"&gt;
            &lt;script&gt;
                document.getElementById('btn').onclick=function(){
                    alert('我被点击了！');
                }
            &lt;/script&gt;
        </pre>
* 常见事件
    * onclick：鼠标单击事件
    * onblur：元素失去焦点
    * onfocus：元素获得焦点
    * onload：某个页面或某个图片被完成加载时触发
    * onsubmit：当表单提交时触发该事件
    * onkeydown：某个键盘的键被按下触发
    * onmouseover：鼠标被移到某元素上触发
    * onmouseout：鼠标从某元素移开触发


## 鼠标事件
* event：当前发生的事件，发生咋那个事件源元素
    * srcElement：事件源
        * tagName：获取事件源的元素的名字
    * parentElement：获取指定事件源元素的父元素
    * style：可以通过调用这个来设置某元素的样式
    * cells：获取tr中的所有单元格，返回一个数组
    * cursor：展示光标样式
        * pointer：手势图标，不能用hand
* onmouseover：鼠标悬停状态
* onmouseout：鼠标离开状态
* onclick：鼠标点击状态
    * deleteRow(IndexRow)方法：删除对应表格中的对应IndexRow(索引)处的行
        * rowIndex：获取指定行在表格中的索引
    * insertRow([IndexRow])方法：在对应索引添加一行，返回一个tr元素
    * insertCell([IndexRow])方法：在对于索引添加一个单元格，返回一个td元素
    * innerHTML：表示设置当前元素的内部HTML代码，会覆盖原本的内部HTML代码
    * innerText：表示设置或获取当前元素的内部文本，获取出来的是字符串
    * nodetype：有两个常量
        * 1：表示是元素常量，即ElementNode
        * 3：表示是文本常量，即TextNode
    * select()方法：选择输入框内部的文本
* onblur：元素失去焦点状态
* onkeydown：键盘输入状态
    * keyCode：获取到键盘对应的字符的ASCII码值

