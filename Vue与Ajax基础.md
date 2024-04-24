# Vue
* Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写；
* 基于MVVM（Model-View-ViewModel）思想，实现数据的双向绑定，将编程的关注点放在数据上；
* 基本定义：

```vue
new Vue({
	el:"",<!-- 要绑定的区域，可以是类名、id名等等 -->
	data(){
		数据名:数据值<!-- 可以实现数据的双向绑定 -->
	},
	methods:{<!-- 用于存放一些方法，可以与事件绑定一起使用 -->
		show(){
			alert("我被点击了！！");
		}
	},
	mounted(){<!-- 挂载完成的时间点，vue被初始化成功，HTML页面渲染成功，并且可以发送异步请求，加载数据 -->
		alert("vue挂载完成，发送异步请求");
	}
});
```



指令：HTML标签上带有v-前缀的特殊属性，不同指令具有不同含义

* vue的组件文件以.vue结尾，每个组件由三部分组成：&lt;template&gt;(模板部分，由它生成HTML代码)、&lt;script&gt;(控制模板的数据来源和行为，即JS代码部分)、&lt;style&gt;(css样式部分)



## 常用指令

    * v-bind：为HTML标签绑定属性值，如设置href，css样式等，如果绑定的值是null或者undefined，那么该attribute将会从渲染的元素上移除，也可以绑定对象或数组
    * v-model：在表单元素上创建双向数据绑定
    * v-on：为HTML标签绑定事件
    * v-if。v-else-if、v-else：条件性的渲染某元素，判定为true时渲染，否则不渲染，不满足条件时，源码中看不见
    * v-show：根据条件展示某元素，区别在于切换的是display属性的值，在源码中能看见其style属性里有display:none
    * v-for：列表渲染，遍历容器的元素或者对象的属性



* 文本时用{{键}}绑定，标签的属性值用v-bind绑定



## 生命周期

生命周期：指一个对象从创建到销毁的整个过程
* 生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法(钩子)
    * beforeCreate：创建前
    * created：创建后
    * beforeMount：挂载前
    * mounted：挂载完成，Vue初始化成功，HTML页面渲染成功；
      * 发送异步请求，加载数据；
      * 定义的方法与data方法一样；
    * beforeUpdate：更新前
    * updated：更新后
    * beforeDestroy：销毁前
    * destroyed：销毁后

​	![image-20231129001250417](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231129001250417.png)



## Vue组件

* 组件最大优势就是可复用性
  * 当使用构建步骤时，一般会将Vue组件定义在一个单独的.vue文件中，这被叫做单文件组件（简称SFC）

![image-20240118133440232](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240118133440232.png)

* 组件传递prop校验![image-20240118131949578](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240118131949578.png)
  * 注意：prop是只读的，不可以修改

* 组件之间传递数据的方案
  * 父传子：prop
  * 子传父：自定义事件（this.$emit）





# Vue CLI脚手架

* Vue CLI是一个基于Vue.js进行快速开发的完整系统。使用Vue脚手架后我们开发的页面将是一个完整系统（项目）

* Vue CLI优势
  * 通过vue-cli搭建交互式的项目脚手架。
  * 通过@vue/cli+@vue/cli-service-global快速开始零配置原型

* npm：node package mangager ，node js包管理工具
  * 配置淘宝镜像：npm config set registry http://registry.npmmirror.com
  * 配置npm下载依赖位置：
    * npm config set cache "D:\WorkSpace\nodereps\npm-cache"
    * npm config set prefix "D:\WorkSpace\nodereps\npm_global"
  * 验证node js环境配置：npm config ls

* 安装脚手架
  * 卸载脚手架：
    * npm uninstall -g @vue/cli ：卸载3.x版本脚手架
    * npm uninstall -g vue-cli ：卸载2.x版本脚手架
  * 安装vue cli：
    * npm install -g vue-cli ：安装2.x版本脚手架
    * npm install -g @vue/cli ：安装3.x版本脚手架
* 创建vue脚手架项目：npm init vue@latest



# Ajax
概念：Asynchronous JavaScript And XML，异步的JavaScript和XML
作用：

   * 与服务器进行数据交换：通过AJAX可以给服务器发送请求，并获取服务器响应的数据
        * 使用AJAX和服务器进行通信，就可以使用HTML+AJAX来替换JSP页面
   * 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用校验等等....



## 同步和异步

* 同步：![image-20231127220925003](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231127220925003.png)
* 异步：![image-20231127220940212](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20231127220940212.png)



# JSON数据格式

* JSON基础语法：

  * 定义：

    ```javascript
    var 变量名 = {
        "key1" : value1,
        "key2" : value2,
        ...
    };
    ```

  * value的数据类型可以为：

    * 数字（整数或浮点数）
    * 字符串（在双引号中）
    * 逻辑值（true或false）
    * 数组（在方括号中）
    * 对象（在花括号中）
    * null

  * 获取数据：变量名.key，例如：json.name;

  

# Vue 与 Axios

* 导入axios：npm install axios
* 下载完成后可以在node_modules目录下看见axios目录





# Vue路由
组成：

* VueRouter：路由器类，根据路由请求在路由视图中动态渲染选中的组件
* &lt;router-link&gt;：请求链接组件，浏览器会解析成&lt;a&gt;
*  &lt;router-view&gt;：动态视图组件，用来渲染展示与路由路径对应的组件

* 安装vue-router：npm install vue-router@4

* 在src/router/index.js中创建路由器，并导出（没有这个路径就重新创建一个）

* 在定义路由关系时，如果谁有子路由就在component后面加children数组，里面的路由写法与前面一致

* ![image-20240119114046467](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240119114046467.png)

  ![image-20240119114257765](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240119114257765.png)

  ![image-20240119114920086](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240119114920086.png)

* 可以在任意组件中以this.$router的形式访问
