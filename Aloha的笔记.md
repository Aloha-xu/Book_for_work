[Aloha的笔记]()

# 前端

## 框架

### vue

#### Vue是什么？

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://v3.cn.vuejs.org/guide/single-file-component.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#components--libraries)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。



#### vue项目打包过大，怎么办？

路由懒加载

UI框架按需加载

组件打包

cdn外部加载一些包

静态资源本地缓存

图片资源的压缩

开启GZip压缩

tree shaking





#### Vue为什么不利于SEO？⭐

因为vue页面是根据匹配路由动态的生成的，seo搜索是根据静态的html文件搜索的，seo不会等vue的axios请求，返回数据，在渲染出来。就是说vue一开始服务器返回的html上面是没有**有效的数据**。





#### 为什么需要export default组件 而不是直接new vue（）组件实例  

new Vue({}) ->创建一个Vue的实例 就是相当于创建一个根组件；

而export default 相当于使用Vue.component注册了一个全局组件或者是一个单纯的局部组件。就像一个模板一样，还没有被用到 。

那在什么时候用呢。创建实例的时候 也就是 new Vue({})创建一个实例之后 如果这个根实例中有调用这个组件，这时就发挥作用。



其实导出的这个对象就是一个`组件的选项对象`，你在其它地方import这个组件的选项对象，然后在components注册这个局部组件时，实际上vue就会通过这个选项对象来构造一个新的vue组件实例

一个项目里面就一个new Vue创建出来的根组件，不用new Vue继续创建组件是因为，vue只需要一个根组件，与其他组件形成组件树。



#### vuex 、localStorage区别

localStorage是本地存储，localStorage只能存储字符串类型的数据，是将数据存储到浏览器的方法，一般是在跨页面传递数据时使用。vuex不能跨页面传输数据

Vuex是存储在内存种，能做到数据额响应式，localStorage不能。

刷新页面时Vuex存储的值会丢失，localStorage不会。



#### 路由守卫 请求拦截响应 分别可以做什？

路由守卫 

​	登录状态

​	有角色权限就权限判断，动态生成可访问的路由表



请求拦截

​	文件上传 包一层formdata

​	设置一些请求头参数（自定义的x-token）

响应拦截	

​	判断登录死否过期了

​	根据返回的不同状态码提示信息



#### 使用 Object.defineProperty() 来进行数据劫持有什么缺点？

在对一些属性进行操作时，使用这种方法无法拦截，**比如通过下标方式修改数组数据或者给对象新增属性，这都不能触发组件的重新渲染，因为 Object.defineProperty 不能拦截到这些操作。**更精确的来说，对于数组而言，大部分操作都是拦截不到的，只是 Vue 内部通过重写函数的方式解决了这个问题。

在 Vue3.0 中已经不使用这种方式了，而是通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用Proxy 的好处是它可以完美的监听到任何方式的数据改变，唯一的缺点是兼容性的问题，因为 Proxy 是 ES6 的语法。



#### computed和watch的区别

computed 有缓存  ，就是computed的触发是需要根据它所检测对象的值改变了，才会触发，重新计算，否者就直接使用之前缓存的哪个值，直接返回 。 所以比watch更省性能。

watch  每一次监听的对象   改变就会触发



#### data为什么是个函数，而不是对象

最主要就是为了防止组件复用的时候，重复创建，那么如果不用函数作为返回对象，形成一个新的变量，那么就会造成 相同组件的变量互连关联了。数据错误。



#### watch

第一次初始化的时候是不会触发handler里面的代码 watch也不能使用箭头函数



#### vue的响应式原理

get ----》依赖收集  dep。depend（）

set ---- 》 派发更新 dep。notify（）

vue2是通过Object.defiendProperty来劫持各个属性 的setter，getter，在实例化数据的时候就会触发getter里面的dep.depend（）收集依赖，当数据有变化的时候，就会触发dep.notify（）遍历dep里面所有的watcher的update，通知视图更新。

#### Vue生命周期

new vue

初始化 生命周期函数 事件函数

before create 钩子函数

初始化 data methods

created 钩子函数

生成虚拟dom

before mounte钩子函数

把虚拟dom转化为真实的dom挂载到挂载点上

mounted钩子函数

------------------------------------------------------------------》    更新那一块       beforeupdate钩子函数   然后就根据修改的变化修改虚拟dom更新 真实的dom   uodated钩子函数

before destroy 钩子函数

清除组件里面的数据 方法 指令  过滤器 。。。。

destoryed 钩子函数   



生命周期可以补充一下 **activated deactivated** 这两个是在keep-alive标签里面缓存路由页面的时候触发的钩子函数



**网页直接关闭 des毁灭钩子函数 不触发 只有在切换vue页面的时候才会触发**



#### mounted拿到数据可以后可以直接获取dom吗？

可以



#### nextTick原理

封装了适配各个浏览器的微任务进程，就是把回到函数放到微任务队列中



#### vue模板（template）里为什么不能使用多个头结点？

其内只能有一个节点是因为当前 构建 和 diff virutalDOM 的算法还未支撑这样的结构，也很难在保证性能的情况下支撑。



#### vue的优化方案

https://juejin.cn/post/7099365409400291336



#### vue做过什么性能优化？

v-if v-for 不能放到同一个标签上面 v-for 需要有key

长列表渲染的时候 这个列表上面的数据如果不需要有改变 就是纯属的展示的话 可以冻结这个变量，减少响应式的绑定 Object.freeze

不需要响应式绑定的数据 不要放到data上面  直接放到created里面

事件代理 子节点如果是通过for循环 并且绑定事件  可以考虑绑定到父节点上

函数式组件

路由懒加载  等等其他懒加载

v-show v-if

computed计算属性 的 保存上一次结果

变量本地化 就是减少在函数方法里面经常调用响应式的数据 ，其实这样每一次调用就会触发响应式的getter收集依赖

第三方插件按需引入

keep alive 缓存

手动绑定的事件 不用的时候需要手动销毁  settimeout这些也要 计时器销毁



#### Vue子传父通过$emit 如果没有额外的代码操作，那么就可以直接内联写在tmplete模板里面

```
子组件
<button @click="$emit('enlarge-text'，1)">
  Enlarge text
</button>
父组件
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
//可以直接使用$event拿到 子传过来的 1
```



#### Vue组件之间的通信方法有哪些？

父传子 props 子传父 $emit

vuex

eventBus 就是new一个新的Vue 在这个干净的vue实例上面通信。

$attrs 和$listeners 这个就是跨多个组件之间不需要多次的props/$emit

```
$emit('xxx',a,b,c) 子组件这样传参出去
父组件 接收  func(a,b,c)
```



#### 修饰符lazy

```
<!-- 在“change”时而非“input”时更新 --> 
<input v-model.lazy="msg">
回车的时候才会改变
```



#### vue 路由 push   replace   go  的区别 

vue的路由api是模仿window的  路由其实是有一个栈的  通过push就进站一个 等等规则 

push  就是跳转到目标路由的，并且把路由地址推进栈  

replace  也是跳转到目标路由的，但是不会把路由地址推进站，就是说他跳转之后你dosomething退后路由是退后不到跳转过来的路由的

go  就是通过整数  1 2 -1 -3  这些 进行路由的跨栈跳转   前进多少步  退后多少步  表示在历史堆栈中前进或后退多少步



#### v-for 与 v-if 的性能优化

vue2: v-for 优先级高于 v-if
vue3: v-if  优先级高于 v-for



#### Vue Loader（加载器）

vue loader 实际就是webpack的一个loader，用于打包的一个加载器/工具。



vue loader 做了什么？

处理资源路径：就是那些路径可以写成相对路径。eg：@ 指指到 ./src 之类的

使用预处理器：预处理器顾名思义就是预先处理一些文件，eg：处理一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

Scoped CSS：就是给css添加了作用域，需要注意的点：

- 子组件的根节点会同时受其父组件的 scoped CSS 和子组件的 scoped CSS 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。
- 有些像 Sass 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 或 `::v-deep` 操作符取而代之——两者都是 `>>>` 的别名，同样可以正常工作。
- 通过 `v-html` 创建的 DOM 内容不受 scoped 样式影响，但是你仍然可以通过深度作用选择器来为他们设置样式。就是还是需要：：v-deep 穿透

- **Scoped 样式不能代替 class**。考虑到浏览器渲染各种 CSS 选择器的方式，当 `p { color: red }` 是 scoped 时 (即与特性选择器组合使用时) 会慢很多倍。如果你使用 class 或者 id 取而代之，比如 `.example { color: red }`，性能影响就会消除。
- **在递归组件中小心使用后代选择器!** 对选择器 `.a .b` 中的 CSS 规则来说，如果匹配 `.a` 的元素包含一个递归子组件，则所有的子组件中的 `.b` 都将被这个规则匹配。



#### $event 事件传递原生事件属性

 @click="setFormData(subTrebleItem,$event,subItem,subTrebleIndex)"

$event？？？这个

1.$event 是 vue 提供的特殊变量，用来表示原生的事件参数对象 event

1.1在原生事件中，$event是事件对象 可以点出来属性 

2.在原生事件中，$event是事件对象，在自定义事件中，$event是传递过来的数据（参数）

2.1在自定义事件中，$event是传递过来的数据



#### template标签不能v-for



#### vue中@click.native的使用

怎么理解这一个native呢
你可以这么理解
一般的div button a 这些标签都是html原生有的，
但是呢，如果你用vue，又使用了组件，
那么你创建的那个组件是不是需要<xxx></xxx>这样子写呢，那么他就是不是原生标签了，
那么在这里绑定原生的事件是无效的。
因为vue组件不是走原生标签那一套的。（因为vue有自己事件运行机制，子组件son不是原生DOM元素，我们是无法直接给其绑定原生事件并触发的）


需求，就是在自定义的组件标签上面绑定原生的事件，例如click事件这些，
那么就可以直接使用click.native="func"

也可以通过子传父的$emit方式绑定自定义的事件。

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\Snipaste_2022-11-25_15-04-19.png)



#### vue config配置 跨域

```
 '/sms': {
    target:'xxxx', //代理到的目标
    changeOrigin: true, //是否允许跨越
    pathRewrite: {
      '^/sms': '' //重写,
    }
  },
 
 接口那边的url : /sms/student/noToken/saveInfo
 
 pathRewrite: {'^/sms': ''}//路径重写
 就是匹配到的路径/sms 就替换为空
 
 所以 接口那里url就需要/sms/student/noToken/saveInfo的 sms ，是用来匹配的,用于触发这个代理的。
```

#### 过滤器 

 vue3已经移除了过滤器，想要在vue3中做同样的效果只能用func或者computer处理数据了。

过滤器一般是用来给数据进行一些简单的处理，大小写替换，加一个符号等等，

```
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
<el-form-item :label="$t('common.new')|AddColon" prop="name">
```

#### foreach里面如果使用splice连续的删数据，会导致splice只删了一条。

```
splice导致遍历的元素发生了变化，但是前面两次都没有什么问题。第三次的时候遍历的索引为2，删除该元素之后，后面还有一个num值为0的元素，这个元素的索引变为了2，但由于索引为2的元素已经遍历过了，因此遍历索引为3的元素，最后存在pub属性，因此产生了这个bug。

总结：在vue里面虽然splice是响应式的，但是不建议与forEach连用，一般来讲forEach无法中断遍历，当需要过滤数据的时候还是采用filter比较好，需要修饰数组（不删减数组）的时候采用map遍历比较好。
```













### Vant

#### Overlay 遮罩层  vant 遮盖层    后面的内容会移动 应该是不能滚动的

显示遮罩层时设置document.documentElement.style.overflowY = 'hidden';

隐藏遮罩层时设置document.documentElement.style.overflowY = 'auto'; 



#### Overlay 遮罩层  vant 遮盖层   遮罩层后面的页面 会重置到顶部

显示遮罩层时设置     window.scrollTo(0, this.getScrollTop())

```
getScrollTop () {
      let scrollTop = 0;
      if (document.documentElement && document.documentElement.scrollTop) {
        scrollTop = document.documentElement.scrollTop;
      } else if (document.body) {
        scrollTop = document.body.scrollTop;
      }
      return scrollTop;
    }
```

#### 



### ElementUI



#### Message

element 的Message这样的组件，有两种方式可以引入使用，

其中一种是局部的引入，

```
import { Message } from 'element-ui';

Message({message: xxxxx | string | vnode ,type: 'warning' | string })
```



一种是全局的引入

```
如果你在main.js里面直接把element全部引入，没有按需引入，那么就直接可以使用了。不需要额外再引入

this.$message({messgae:string|vnode,type:string})
```



#### upload

elementui的upload组件  他是没有事件的，全都是props进行传参。

举个例子 ：

：on-success = 这里可以赋值一个func   但是 不能  ：on-success =  func（）  这样写，这样写就会直接触发这个方法了。

简单来说就是upload只有props传值，没有绑定事件。就是说是不能传参的，也不能拿到$event这些属性。

如果需要做一些传参的，只可以在upload外面自己包一层组件了。



如果你使用http-request重写上传的方法，那么action就可以不用给他赋值，但是还是要有action。

upload   如果重写了http-request  那么上传成功的on-sucess/error这些方法是不会自动触发的，需要你在重写的方法里面 通过拿到的参数（自己定义的那个方法有默认回传的参数的，参数名字随便起）手动调用onSuccess方法，然后在onsccess方法里面给list push进去，这样就不会出现闪一下的问题，闪一下的问题就是因为push进去的数据格式有问题（你需要push进去他这些事件返回的file），如果需要加一些其他字段进去，那就在onsuucess方法里面object.assign（）一下，加进去，这样就不会闪了。



####  ios el-select下拉框，要点两次才选中

```
/** ios el-select下拉框，要点两次才选中 */
.el-scrollbar >.el-scrollbar__bar {
    opacity: 1 !important;
}

要写在app.vue文件下 ，原生方式写样式
```



#### 解决Element表单回车提交会刷新页面的Bug

https://blog.csdn.net/qq_52697994/article/details/127002246





### react



### framework7



#### framework 路由

framework路由是没有区别像vue-router那样需要层级的，framework路由是直接通过路径进行判断跳到那里，back就返回上一级，framework路由是会预加载页面的，eg：我从页面3返回到页面2，那么framework还会去history去加载页面1（该页面是页面2的上一个页面），就是说从右边返回时候路由才会预加载页面。



路由 传参    let url = '/community_helper_routine_tasks/?police_station=' + g_login_police_station

router.navigate('/some-page/', { transition: 'f7-cover' })

取参数this.$f7route.query.police_station;



#### [page:reinit](https://framework7.io/docs/page#event-page:reinit) 相当于vue生命周期的actived钩子函数



#### f7  提示api

```
this_component.$f7.toast.show({text: "请填写管理员手机号码", position: "center", closeTimeout: 2000});

this_component.$f7.dialog.preloader("请稍后...");

this_component.$f7.dialog.close();

this_component.$f7.dialog.alert("请输入记录内容", "提示");
```







## 前端的一些兼容ios



### 使用原生的input，兼容ios弹出九宫格数字输入框

需求  使用input （type=  number） 但是ios就不会自动的弹出九宫格的数字键盘，就需要设置

```
<input type="number" pattern="[0-9]*">  
```

才可以弹出九宫格键盘



```
css 隐藏掉input number类型会出现  会出现这个增减数值的按钮，这里建议使用css去控制不显示：

兼容一下各个浏览器
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}

//火狐
input {
  -moz-appearance: textfield;
}
```



### 使用 css 适配 iphoneX     （ 刘海屏   、 底部黑色的长条）

```
1）适配方案一：使用已知底部小黑条高度34px/68rpx来适配（不推荐）

      
（2）适配方案二：使用微信官方API，getSystemInfo()中的safeArea对象进行适配（推荐）

       （3）适配方案三：使用苹果官方推出的css函数env()、constant()来适配 （推荐）
       H5适配iPhoneX底部小黑条(Home Indicator)
        · 适配方案：使用苹果官方推出的css函数env()、constant()来适配 （推荐）


```

https://blog.csdn.net/qq_39224266/article/details/105200198

https://juejin.cn/post/6844904106088202254



### vue 隐藏h5页面在微信端底部出现的白色导航条

这个鬼东西 只有ios才会出现

```
使用微信的接口，关掉导航栏。没试过，不知道行不行
使用replace，还得考虑back，有时候是需要history的，解决办法是replace的时候手动的加history了。

```











## JS





let 块级作用域  const

模板字符串

```
 `${}`
```

箭头函数

for..of 、for...in

Promise async await 

模块化语法  export import 

proxy对象代理

函数参数的默认值

类class

set map数据结构   symbol新的数据结构

对象解构

object.assign（）、object.is（）、object.keys（）、object.values（）

Array.prototype.includes，Includes 方法用来检测数组中是否包含某个元素，返回布尔类型值

...   延展操作符

?.   可选链操作符

迭代器 生成器

简化对象写法，就是对象是key，value也是与key同名的，那么就可以直接写一次。

bigInt数据类型

replaceAll

 Array.flat()  [1, 2, [3]].flat() // [1, 2, 3]  

Array.flatMap() 

人如其名，它的效果就是 `Array.flat` + `Array.map`，理解它的最好方式就是举个例子

https://blog.csdn.net/zxd1435513775/article/details/119862852





### 什么是闭包？⭐

闭包就是内部函数引用了外部函数的变量，就是说内部函数可以访问到外部函数的对应变量作用域。

一般就是用来让一些变量不被浏览器清除内存机制清除。

不用的时候需要手动清除一下 就是设置变量为null

```
function makeAdder() {
  var sum = 0
  return function(y) {
    sum += y;
    return sum
  };
}
```

经典闭包问题

```
for (var i = 1; i <= 5; i++) {
setTimeout(function timer() {
console.log(i)
}, i * 1000)
}

==> 6 6 6 6 6
```

解决办法

```
//使用let
for (let i = 1; i <= 5; i++) {
setTimeout(function timer() {
console.log(i)
}, i * 1000)
}

//函数作用域
for (var i = 1; i <= 5; i++) {
;(function(j) {
setTimeout(function timer() {
console.log(j)
}, j * 1000)
})(i)
}
//使用settimeout第三个参数传参
for (var i = 1; i <= 5; i++) {
	setTimeout(
		function timer(j) {
		console.log(j)
		},
	i * 1000,
	i
	)
}

```



### 数组的常用方法⭐

https://vue3js.cn/interview/JavaScript/array_api.html#%E4%B8%80%E3%80%81%E6%93%8D%E4%BD%9C%E6%96%B9%E6%B3%95



### 谈谈对This对象的理解

https://vue3js.cn/interview/JavaScript/this.html#%E4%B8%80%E3%80%81%E5%AE%9A%E4%B9%89



### 简单来说执⾏上下⽂是什么⭐

创建执行上下文两个阶段：创建、执行

创建：this的绑定  全局---this就绑定到全局对象window对象   函数上下文就绑定到调用函数的对象上。

​			创建词法环境    词法就是标识符 ----变量的 数据结构的映射

​										内部有两个组件  环境记录器  记录变量函数声明的实际位置

​																		外部环境的引用  可以访问父作用域

执行：在js中，js代码在执行之前，这个时候就会创建全局的上下文，他会先预编译代码，预编译代码会把需要用到的变量和函数都拿出来，变量先赋值undefiend 、函数声明好。在一个函数执行前也会创建一个或多个函数上下文。会有多个this、arguments



### 分别介绍一下作用域和作用域链的含义和使用场景。⭐

作用域

- 全局作用域。
- 函数作用域。
- 块级作用域。

作用域链。就是变量查找的路径。如果需要获取变量的值，那么他首先在自己的作用域中查找。如果没有，去上一层作用域，直到查到全局作用域。如果没有就报错。**请注意，这种作用域链式基于词法作用域的，就是根据变量定义的位置决定的，而不是执行的上下文环境。**

那他为什么会知道上层作用域是谁呢？

每个执行上下文中的**变量环境**中都有一个outer属性指向外部执行上下文。所以就依靠这个指向查找的。并且作用域链是由词法作用域决定的。





### call apply bind 三者的区别与作用⭐

这三个⽅法都可以显示的指定调⽤函数的 this 指向。

 其中 apply ⽅法接收两个参数：⼀个是 this 绑定的对象，⼀个是参数数组。

call ⽅法接收的参数， 第⼀个是 this 绑定的对象，后⾯的其余参数是传⼊函数执⾏的参数。也就是说，在使⽤ call() ⽅法 时，传递给函数的参数必须逐个列举出来。

bind ⽅法通过传⼊⼀个对象，返回⼀个 this 绑定了传 ⼊对象的新函数。这个函数的 this 指向除了使⽤ new 时会被改变，其他情况下都不会改变。



### 实现call apply bind （手写代码）⭐

这三者都是用来修改this的指向的，本来this指向window，现在传参让他指向传参的那个值

```
Function.prototype.myCall = function (context, ...args) {
    //判断上下文
    context = context || window

    //把要被调用的函数指向context的fn属性上，这个this就是.mycall调用的那个方法
    context.fn = this;

    //调用
    let result = context.fn(...args)

    //删除fn属性
    delete context.fn

    return result
}


Function.prototype.myApply = function (context, arr) {
    //判断上下文
    context = context || window

    //把要被调用的函数指向context的fn属性上
    context.fn = this;

    //调用  需要判断有没有传arr
    let result
    if (arr) {
        result = context.fn(arr)
    } else {
        result = context.fn()
    }

    //删除fn属性
    delete context.fn

    return result
}


Function.prototype.myBind = function (context, args) {

    fn = this;

    //匿名函数
    return function () {
        return fn.apply(context, args)
    }
}


```



例子

```
function dog() {
        name = "wangwang"
        console.log(this);
        console.log(this.name);
    }

    let cat = {
        name: 'maioamiao'
    }
    dog()
    dog.myCall(cat)
  
```

![](http://114.132.239.118:3003/getpic/1658299900885.png)



### 数据类型⭐

JavaScript共有⼋种数据类型，分别是 Undefined、Null、Boolean、Number、String、Object、 Symbol、BigInt。

(基本类型)：String、Number、Boolean、Null、Undefined

引用数据类型：Object、Array、Function。

基本类型的数据是存放在**栈**内存中的，而引用类型的数据是存放在**堆**内存中的

ES6 引入了一种新的原始数据类型 Symbol ，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。

或者常量

```
let sy = Symbol("key1");
 
// 写法1
let syObject = {};
syObject[sy] = "kk";
console.log(syObject);    // {Symbol(key1): "kk"}
```



### null和undefined区别⭐

undefined 代表的含义是**未定义**，null 代表的含义是**空对象**。





### JavaScript为什么要进⾏变量提升，它导致了什么问题？⭐



JS在拿到⼀个变量或者⼀个函数的时候，会有两步操作，即**解析**和**执⾏**。

解析的时候就把var定义的变量提升了 

好处：**提⾼性能** 、**容错性更好**

在JS代码执⾏之前，会进⾏语法检查和预编译，并且这⼀操作只进⾏⼀次。

```
a = 1; 
var a; 
console.log(a);
写成这样 js也可以把var a 给你提上去
```

在ES6中提出了**let、const**来定义变量，它们 就**没有变量提升的机制**。





### 对原型、原型链的理解 ⭐

原型：在JavaScript中是使⽤构造函数来新建⼀个对象的，每⼀个构造函数的内部都有⼀个 prototype 属性， 它的属性值是⼀个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和⽅法。当使⽤构造 函数新建⼀个对象后，在这个对象的内部将包含⼀个指针，这个指针指向构造函数的 prototype 属性对 应的值，在 ES5 中这个指针被称为对象的原型。⼀般来说不应该能够获取到这个值的，但是现在浏览 器中都实现了 __proto__ 属性来访问这个属性，但是最好不要使⽤这个属性，因为它不是规范中规定 的。ES5 中新增了⼀个 Object.getPrototypeOf() ⽅法，可以通过这个⽅法来获取对象的原型。

原型链：当访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 Object.prototype 所以这就是新建的对象为什么能够使用 toString() 等方法的原因。

**prototype** -> 函数的一个属性    只有function有  普通的对象，实例都没有prototype属性的

__proto__ -> 对象Object的一个属性



```
function Foo(){
Foo.a = function(){
console.log(1);
}
this.a = function(){
console.log(2)
}
}
Foo.prototype.a = function(){
console.log(3);
}
Foo.a = function(){
console.log(4);
}
Foo.a();
let obj = new Foo();
obj.a();
Foo.a();

4
2
1

Foo.a() 这个是调⽤ Foo 函数的静态⽅法 a，虽然 Foo 中有优先级更⾼的属性⽅法 a，但 Foo 此
时没有被调⽤，所以此时输出 Foo 的静态⽅法 a 的结果：4
2. let obj = new Foo(); 使⽤了 new ⽅法调⽤了函数，返回了函数实例对象，此时 Foo 函数内部的属
性⽅法初始化，原型链建⽴。
3. obj.a() ; 调⽤ obj 实例上的⽅法 a，该实例上⽬前有两个 a ⽅法：⼀个是内部属性⽅法，另⼀个是
原型上的⽅法。当这两者都存在时，⾸先查找 ownProperty ，如果没有才去原型链上找，所以调⽤
实例上的 a 输出：2
4. Foo.a() ; 根据第2步可知 Foo 函数内部的属性⽅法已初始化，覆盖了同名的静态⽅法，所以输出：
1

```



``` 
/*
    js的 prototype 与 __proto__ 
        prototype : 原型
        __proto__ : 原型链（连接点）
    从属关系
        prototype -> 函数的一个属性 也就是一个对象{} 但是这个对象有一些规定好的属性就在里面了
        __proto__ -> 对象Object的一个属性 也就是一个对象{}
        对象的__proto__保存着该对象的构造函数的prototype
    */


    //这是一个构造函数
    function T() {
        this.a = 1;
    }

    //使用prototype给Object的prototype绑定了b属性
    Object.prototype.b = 2

    //使用prototype给T的prototype绑定了c属性
    T.prototype.c = 3

    console.log(T.prototype) //构造函数有prototype

    console.log(T.__proto__) //ƒ () { [native code] }
    console.log(T.__proto__ === Function.prototype) // true
    console.log(T.__proto__ === Object.prototype) // false
    console.log(T.__proto__ === Object.__proto__) // true

    console.log(T.prototype.__proto__ === Object.prototype) //指向Object的prototype true
    console.log(Object.prototype)//这里的Object的原型对象

    //实例对象
    let t = new T()

    console.log(t) //t对象
    console.log(t.__proto__) //T构造函数的prototype
    console.log(t.__proto__.__proto__) //Object的prototype
    console.log(t.prototype) //实例对象没有prototype

    console.log(t.__proto__ === T.prototype) //t的__proto__保存着该对象的构造函数的prototype（T的prototype）

    console.log(T.prototype.__proto__ === Object.prototype)

    /*
    t{
        a : 1
        __proto__:T.prototype{
            c:3
           __proto__:Object.prototype{
               b:2
               x __proto__ 、没有 __proto__  、__proto__ == null
           }
        }
    }
    */

    console.log(t.a)
    console.log(t.b)
    console.log(t.c)

    // const T = new Function() 
    // T函数也是new Function 出来的，那么Function也是一个函数，那么函数就会有prototype和__proto__,他们都是一个函数，至于怎么实现就不给看。Function.prototype已经是最底层了,Function.__proto__指向的就是Function.prototype，所以他们全等 这都是底层已经规定好的了。
    console.log(Function) // ƒ Function() { [native code] }
    console.log(Function.prototype) // ƒ () { [native code] }
    console.log(Function.__proto__) // ƒ () { [native code] }
    //Function.__proto__指向的就是Function.prototype
    console.log(Function.__proto__ === Function.prototype) // true

    // const obj = {} 这是怎么来的？ 那就是通过new Object()出来的
    // const obj = new Object(); 那么这个Object()也是一个function(函数) 
    // 那么new Function就会指向function的prototype
    console.log(typeof Object) // function
    console.log(Object) // ƒ Object() { [native code] }
    console.log(Object.__proto__) // ƒ () { [native code] }
    console.log(Object.__proto__ === Function.prototype) //true //因为Object也是一个函数，Object的__proto__就会指向Function的prototype

    console.log(Function.__proto__ === Function.prototype) // true
    console.log(Object.__proto__ === Function.prototype) // true
    //  => 得出Function.__proto__ === Object.__proto__
    console.log(Function.__proto__ === Object.__proto__) // true

    console.log(Object.prototype.__proto__)// null //这就是原型链的顶端 

```

![js原型链](C:\Users\lovelife_xu\Desktop\总结资料\js原型链.png)

### 为什么要用prototype

```js
function NewObj(name,num){
    this.name = name;
    this.num = num;
    this.changNum = function(c){
        this.num = c;
    }
}

const obj1 = new NewObj('aloha',10)

obj1 = {
    name:'aloha';
    num:10;
    changeNum :function(c){
        this.num = c
    }

obj1.changNum(100);
obj1.num； // 结果是100

//如果我们现在想要改变changeNum函数呢？

//变成 ==>  this.changNum = function(c){
//              this.num = c*2;
//              }

//那么你会这样做，想要修改这个changeNum函数就需要修改这个NewObj这个构造函数
function NewObj(name,num){
    this.name = name;
    this.num = num;
    this.changNum = function(c){
        this.num = c*2;
    }
}
//然后再调用这个修改后的方法
obj1.changNum(100);
obj1.num; //依然是100，也就是说这个对象并不受我们修改的模板影响到

//这是为什么呢？
//原因是通过构造函数模板new出来的instance(实例)是直接copy构造函数本身自己一份赋值给刚new出来的instance，所以这个instance是完全在一个独立的内存中，而不是引用（指向）构造函数的内存的。


//所以如果你使用修改构造函数的changeNum函数模板的这个方法，你就需要这样做才能看到修改changeNum函数后的NewObj正确的结果。
// 你需要再new一个新的实例出来 obj2
const obj2 = new NewObj('aloha',10)
obj2 = {
    name:'aloha';
    num:10;
    changeNum :function(c){
        this.num = c*2
    }
//这个obj2与上面的obj1是不一样的
//obj1 = {
//    name:'aloha';
//    num:10;
//    changeNum :function(c){
//        this.num = c
//    }

//这个时候你再调用ChangeNum（100）
obj2.changNum(100);
obj2.num； // 结果是200

//而构造函数这种形式每次都会把自身的属性全部copy一份给每个instance，这就造成了不必要的浪费；并且，当我们想修改这个方法时，就必须重新生成所有的instance才能获得更新

//怎么解决这个问题呢？有一个原型对象。原型对象里的属性和方法并不是像构造函数自身属性一样copy给每个instance，而是“引用”，也可以理解为给每个instance提供一个指向该原型对象的指针，这样每个instance就能找到原型对象里的属性，而很明显，这是一种共享，也就是说，当你修改了这个原型里的属性，那么所有共享该属性的instance都能获得这个修改。因此，原型恰好解决了上面提到的两个问题。

function NewObj(name,num){
    this.name = name;
    this.num = num;
}
NewObj.prototype.changNum = function(c){
        this.num = c;
        }
var newObj1 = new NewObj("kemi",10);
newObj1.changNum(100);
newObj1.num; //很明显是100
NewObj.prototype.changNum = function(c){
        this.num = c*2;
        }//我们重新修改一下这个方法
newObj1.changNum(100);
newObj1.num; //变成200了。

//为什么一般情况下会把属性直接写在构造函数内，而方法通过prototype添加呢？这两种方式的区别上面其实已经有所展现了：大部分的instance的属性都是不同的，比如说name，因此在构造函数内通过this直接绑定给instance无疑是个好方案，而方法通常是通用的，使用prototype可以让每个instance共享同一个方法，而不用每个都copy一次，又能实现实时更新。
```



### 可枚举概念

每个对象都有一个 `propertyIsEnumerable` 方法。此方法可以确定对象中指定的属性是否可以被 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环枚举，但是通过原型链继承的属性除外。如果对象没有指定的属性，则此方法返回 `false`。

**继承过来的属性是不可以被枚举的。**

```
const object1 = {};
const array1 = [];
object1.property1 = 42;
array1[0] = 42;

console.log(object1.propertyIsEnumerable('property1'));
// expected output: true

console.log(array1.propertyIsEnumerable(0));
// expected output: true

console.log(array1.propertyIsEnumerable('length'));
// expected output: false
```

例如：数组的长度属性也是不可被枚举的。

### 浅拷贝和深拷贝

浅拷贝方法：

1.for只遍历第一层   

2.Object.assign()  方法将所有[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)和[自有](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)属性从一个或多个源对象复制到目标对象，返回修改后的对象。

3.=直接赋值



深拷贝方法：

1.递归遍历所有层级   （终止递归条件 判断不是对象的时候就退出）

2.利用JSON对象【JSON.stringify() 、JSON.parse()】 但是这种方式存在弊端，会忽略`undefined`、`symbol`和`函数`

3.通过jQuery的extend方法实现深拷贝 



### 数据类型检测的方式有哪些⭐

#### **（1）typeof**

```javascript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object    
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
console.log(typeof Symbol('1'));     // symbol
```

其中**数组**、**对象**、**null**都会被判断为object，其他判断都正确。

#### **（2）instanceof**

`instanceof`可以正确判断对象的类型，**其内部运行机制是判断在其原型链中能否找到该类型的原型**。

```javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

可以看到，`instanceof`**只能正确判断引用数据类型**，而**不能判断基本数据类型**。`instanceof` 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype` 属性。

#### **（3） constructor**

```javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```

`constructor`有两个作用，一是判断数据的类型，二是对象实例通过 `constrcutor` 对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了：

```javascript
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

#### **（4）Object.prototype.toString.call()**

`Object.prototype.toString.call()` 使用 Object 对象的原型方法 toString 来判断数据类型：

```javascript
var a = Object.prototype.toString;
 
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```

同样是检测对象obj调用toString方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？

这是因为toString是Object的原型方法，而Array、function等**类型作为Object的实例，都重写了toString方法**。不同的对象类型调用toString方法时，根据原型链的知识，调用的是对应的重写之后的toString方法（function类型返回内容为函数体的字符串，Array类型返回元素组成的字符串…），而不会去调用Object上原型toString方法（返回对象的具体类型），所以采用obj.toString()不能得到其对象类型，只能将obj转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用Object原型上的toString方法。



### 判断数组的⽅式有哪些⭐

通过Object.prototype.toString.call()做判断

Object.prototype.toString.call(obj).slice(8,-1) === 'Array';

通过原型链做判断

obj.__proto__ === Array.prototype;

通过ES6的Array.isArray()做判断

Array.isArrray(obj);

通过instanceof做判断

obj instanceof Array

通过Array.prototype.isPrototypeOf

Array.prototype.isPrototypeOf(obj)



### intanceof 操作符的实现原理及实现⭐

```js
function MyIntanceof(left,right){
    let proto = Object.getPrototypeOf(left)
    let prototype = right.prototype
    
    while(true){
        if(!proto) return false
        if(proto === prototype) return true
        proto = Object.getPrototypeOf(proto)
    }
    
}
```



### Object.is() 与⽐较操作符 “ === ”、“ == ” 的区别？ ⭐

使⽤双等号（==）进⾏相等判断时，如果两边的类型不⼀致，则会进⾏强制类型转化后再进⾏⽐ 较。 

使⽤三等号（===）进⾏相等判断时，如果两边的类型不⼀致时，不会做强制类型准换，直接返回 false。 

使⽤ Object.is（a,b） 来进⾏相等判断时，⼀般情况下和三等号的判断相同，它处理了⼀些特殊的情况， **⽐如 -0 和 +0 不再相等，两个 NaN 是相等的。**



### 对类数组对象的理解，如何转化为数组⭐

⼀个拥有 length 属性和若⼲索引属性的对象就可以被称为类数组对象，类数组对象和数组类似，但是 不能调⽤数组的⽅法。

通过 call 调⽤数组的 slice ⽅法来实现转换

```
Array.prototype.slice.call(arrayLike);
```

通过 call 调⽤数组的 splice ⽅法来实现转换

```
Array.prototype.splice.call(arrayLike, 0);
```

通过 apply 调⽤数组的 concat ⽅法来实现转换

```
Array.prototype.concat.apply([], arrayLike);
```

通过 Array.from ⽅法来实现转换

```
Array.from(arrayLike);
```





### 字符串转数组三种方式⭐

1. split拆分 "abc".split(' ') ==> ["a","b","c"] 

2. [...] [..."abc"] ==> ["a","b","c"] 

​    3. Array.from("abc")  ==> ["a","b","c"]



### 数组转字符串三种方式⭐

toString()、toLocalString()、join() 





### 函数柯里化

求和  sum（2，3） sum（2）（3）  这两种形式得出 5

```
    function add(x) {
    //进去就通过arguments判断有几个参数，2个的时候就直接返回x+y
    //
            if (arguments.length === 2) {
                return arguments[0] + arguments[1]
            }
//只有一个参数的时候就返回一个函数
            return (y) => {
                return x + y
            }
        }
```

```
//进阶版
function curry (fn, currArgs) {
    return function() {
        let args = [].slice.call(arguments);

        // 首次调用时，若未提供最后一个参数currArgs，则不用进行args的拼接
        if (currArgs !== undefined) {
            args = args.concat(currArgs);
        }

        // 递归调用
        if (args.length < fn.length) {
            return curry(fn, args);
        }

        // 递归出口
        return fn.apply(null, args);
    }
}

//test
function sum(a, b, c) {
    console.log(a + b + c);
}

const fn = curry(sum);

fn(1, 2, 3); // 6
fn(1, 2)(3); // 6
fn(1)(2, 3); // 6
fn(1)(2)(3); // 6
```



### new 手写实现⭐

```
function mynew(Func, ...args) {
    // 1.创建一个新对象
    const obj = {}
    // 2.新对象原型指向构造函数原型对象
    obj.__proto__ = Func.prototype
    // 3.将构建函数的this指向新对象
    let result = Func.apply(obj, args)
    // 4.根据返回值判断
    return result instanceof Object ? result : obj
}
```



### 对尾递归的理解

尾递归，即在函数尾位置调用自身（或是一个尾调用本身的其他函数等等）。尾递归也是递归的一种特殊情形。尾递归是一种特殊的尾调用，即在尾部直接调用自身的递归函数

两个特点：

- 在尾部调用的是函数自身
- 可通过优化，使得计算仅占用常量栈空间

在递归调用的过程当中系统为每一层的返回点、局部量等开辟了栈来存储，递归次数过多容易造成栈溢出

这时候，我们就可以使用尾递归，即一个函数中所有递归形式的调用都出现在函数的末尾，对于尾递归来说，由于只存在一个调用记录，所以永远不会发生"栈溢出"错误

递归

```
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

factorial(5) // 120

//如果n等于5，这个方法要执行5次，才返回最终的计算表达式，这样每次都要保存这个方法，就容易造成栈溢出，复杂度为O(n)
```

尾递归

```
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120

//可以看到，每一次返回的就是一个新的函数，不带上一个函数的参数，也就不需要储存上一个函数了。尾递归只需要保存一个调用栈，复杂度 O(1)
```



### 运算符

1.<<          24<<1       把1向左移动24位       1    =>   1 0000 0000 0000 0000 0000 0000      十进制就是 ：16777216

2.>>       同理



```js
(~~(Math.random()*(1<<24))).toString(16)
```

~~ 取整  

toString(16) 转16进制



Math.min() => 没有传入值的时候是返回无穷大 Infinity

Math.max() => 无穷小  -Infinity







### 0.1 + 0.2 === 0.3？ 不等于

```
	要弄清这个问题的原因，首先我们需要了解下在计算机中数字是如何存储和运算的。在计算机中，数字无论是定点数还是浮点数都是以多位二进制的方式进行存储的。
	对于整数来说，十进制的35会被存储为： 00100011 其代表 2^5 + 2^1 + 2^0。
	对于纯小数来说，十进制的0.375会被存储为： 0.011 其代表 1/2^2 + 1/2^3 = 1/4 + 1/8 = 0.375
	而对于像0.1这样的数值用二进制表示你就会发现无法整除，最后算下来会是 0.000110011....由于存储空间有限，最后计算机会舍弃后面的数值，所以我们最后就只能得到一个近似值。
	另外说一句，除了那些能表示成 x/2^n 的数可以被精确表示以外，其余小数都是以近似值得方式存在的。
	
解决办法：	
//其实ES6 已经在Number对象上面，新增一个极小的常量Number.EPSILON。根据规格，它表示 1 与大于 1 的最小浮点数之间的差。Number.EPSILON实际上是 JavaScript 能够表示的最小精度。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。

Number.EPSILON=(function(){   //解决兼容性问题
        return Number.EPSILON?Number.EPSILON:Math.pow(2,-52);
      })();
      
//上面是一个自调用函数，当JS文件刚加载到内存中，就会去判断并返回一个结果，相比if(!Number.EPSILON){
  //   Number.EPSILON=Math.pow(2,-52);
  //}这种代码更节约性能，也更美观。
  
function numbersequal(a,b){ 
    return Math.abs(a-b)<Number.EPSILON;
  }
  
//接下来再判断   
    var a=0.1+0.2, b=0.3;
  console.log(numbersequal(a,b)); //这里就为true了
```

### 箭头函数this

箭头函数的`this`跟常规函数的规则完全不同。无论用什么方式、在哪里调用箭头函数，里面的`this`永远是外层函数的`this`。也就是说，箭头函数的`this`是由词法决定的，它没有定义自己的执行上下文。

```
let obj = {
    myFunc(items){
        console.log(this)
        let callback = ()=>{
            console.log(this);
        };
        items.forEach(callback)
    }
}
obj.myFunc([1,2,3])

//{myFunc: ƒ}
//{myFunc: ƒ}
//{myFunc: ƒ}
//{myFunc: ƒ}


在上面的代码中，myFunc()就是箭头函数callback()的外层函数，箭头函数里的this等于外层函数的this，也就是myFunc()
  综上所述，由词法决定this的指向，是箭头函数非常使用的特性之一。在方法里使用回调函数就很方便，this指向很明确，不用去写替换this和绑定this的方法了。
```



### Js数组去重

1. new Set（arr)

2. for嵌套，两层循环，如果第二个等于第一个，删掉第二个，用splice方法
   splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目。
   注释：该方法会改变原始数组。

3. indexOf ：建一个空数组，循环目标数组，判断新数组中是否有当前单参数，没有就push，有就跳过

4. includes：查找数组中是否有某个元素









### js运行机制

JavaScript 单线程，任务需要排队执行
同步任务进入主线程排队，异步任务进入事件队列排队等待被推入主线程执行
定时器的延迟时间为 0 并不是立刻执行，只是代表相比于其他定时器更早的被执行
以宏任务和微任务进一步理解js执行机制
整段代码作为宏任务开始执行，执行过程中宏任务和微任务进入相应的队列中
整段代码执行结束，看微任务队列中是否有任务等待执行，如果有则执行所有的微任务，直到微任务队列中的任务执行完毕，如果没有则继续执行新的宏任务
执行新的宏任务，凡是在执行宏任务过程中遇到微任务都将其推入微任务队列中执行
反复如此直到所有任务全部执行完毕

```
console.log(1.)
    setTimeout(() => {
        console.log(2.)
        setTimeout(() => {
            console.log(3.)
        }, 1000);
        console.log(5.)
    }, 1000);
console.log(4.)
    
结果 1 4 2 5 3
```

### 浏览器loop循环

![http://114.132.239.118:3003/getpic/1655871692185.png](http://114.132.239.118:3003/getpic/1655871692185.png)

浏览器为了能够使得JS内部宏任务与DOM任务能够有序的执行，会在一个task执行结束后，在下一个 task 执行开始前，对页面进行重新渲染 （task->渲染->task->…）

鼠标点击会触发一个事件回调，需要执行一个宏任务，然后解析HTMl。但是，setTimeout不一样，**setTimeout的作用是等待给定的时间后为它的回调产生一个新的宏任务。**这就是为什么打印‘setTimeout’在‘promise1 , promise2’之后。因为打印‘promise1 , promise2’是第一个宏任务里面的事情，而‘setTimeout’是另一个新的独立的任务里面打印的。

![http://114.132.239.118:3003/getpic/1655871791293.png](http://114.132.239.118:3003/getpic/1655871791293.png)







### 谈谈对单页面应用与多页面应用的理解

#### 多页面应用

**原理**：每一次页面跳转，后台就会返回一个新的HTML。如果有公共资源，每一次跳转都需要重新加载。（整页刷新）

**页面跳转**：使用`window.location.href = "./index.html"`进行页面间的跳转。

**页面间数据传递**：可以使用`path?account="123"&password=""`路径携带数据传递的方式，或者`localstorage、cookie`等存储方式



#### 单页面应用

**原理：用vue写的项目是单页应用，刷新页面会请求一个HTML文件，切换页面的时候，并不会发起新的请求一个HTML文件，只是页面内容发生了变化**（局部刷新）

**页面跳转**：路由

**页面间数据传递**：vuex





### 客户端渲染（CSR）VS服务端渲染(SSR)



渲染：就是将数据和模版组装成html



服务端渲染：

就是用户在浏览你的网页的时候，请求一个网页，服务端直接把这个网页的静态HTML显示在你眼前。

解析模板的工作完全交给后端来做，客户端只需要解析HTML页面。

后端生成静态化文件。即生成缓存片段，减少数据库查询的浪费时间。高效。

有利于SEO。

使前端耗时少。（不需要跑一遍JS）

不利于前后端分离，开发效率低。（前端修改了css，后端就需要跟着修改）



客户端渲染：

客户端渲染模式下，服务端把渲染的静态文件给到客户端，客户端拿到服务端发送过来的文件自己跑一遍js，（这里的js就是写好怎么拼接html）根据JS运行结果，生成相应DOM，然后渲染给用户。

前后端分离。前端专注UI，后端专注api开发。前端有更多选择性，不需要遵循后端特定 的模板

前端响应较慢。客户端渲染，前端还需要进行拼接字符串的过程，需要耗费额外的时间，不 如服务器渲染的速度快。

不利于SEO



服务端渲染与客户端渲染之间最大的区别就是HTML的拼接解析在服务器service完成还是客户端完成（浏览器）



不谈业务场景而盲目选择使用何种渲染方式都是耍流氓。比如企业级网站，主要功能是展示而没有复杂的交互，并且需要良好的SEO，则这时我们就需要使用服务器端渲染；而类似后台管理页面，交互性比较强，不需要seo的考虑，那么就可以使用客户端渲染。
另外，具体使用何种渲染方法并不是绝对的，比如现在一些网站采用了首屏服务器端渲染，即对于用户最开始打开的那个页面采用的是服务器端渲染，这样就保证了渲染速度，而其他的页面采用客户端渲染，这样就完成了前后端分离





b站：点击动态、历史等等这些选项会跳转到一个新的页面，是通过`<a>`标签`href='地址' target="_blank"`直接跳转到bilibili的子域名

https://www.bilibili.com/

https://t.bilibili.com/



vue Cli 4  webpack 打包







### Map和Set的区别，Map和Object的区别

Map是以键值对的形式存储数据，键的类型可以是任何数据类型（基本数据类型、对象、函数），值也是一样的。Set类似数组，存储无重复元素的集合。

Map键可以任意值，object只可以是String/Symbol。关于覆盖，map只有当地址相同的键名才会被覆盖。

### Map和WeakMap区别

WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap 只接受对象作为键名（ null 除外），不接受其他类型的值作为键名。**而且 WeakMap 的键名所指向的对象，不计入垃圾回收机制。**










### 箭头函数和普通函数的区别

箭头函数不会创建自己的`this`，所以它没有自己的`this`，它只会从自己的作用域链的上一层继承`this`。

箭头函数继承而来的this指向永远不变

也没有Arguments、原型prototype ---所以不可以New箭头函数







### 堆和栈的区别

栈是存放基本数据类型 String Number Boolean Null Undefiend Symbol

堆是存放引用类型的值的

![](http://114.132.239.118:3003/getpic/1655455419785.png)





### 隐式类型转换

**转换为String**，就是使用 （ + ） 拼接的时候就会转换为字符串。

```js
let a = 123
a = a + ""  // "123"
console.log(typeof a) // "string"
```

**转换为Number**

算法运算符（- * / %），任何值做 - * /，都会将其转换为Number,再做运算；可以利用这一特点，来将任意数据类型转换为Number，只需（任意字符串 - 0）（任意字符串 * 1）（任意字符串 / 1）即可转换为Number，这是一种隐式类型转换，由浏览器自动完成，实际上也是调用了Number()函数；

```js
let a = "123"
+a  ==》 123
console.log(typeof a) // "number"
```

一元运算符（+(正号) ），对于非Number类型的值，它会先转换为Number，然后再运算；可以对一个其他数据类型使用+，来将其装换为number，隐式类型转换，它的原理和Number()函数一样；

```js
var a = "123"
a = +a // 123
a = -a // -123
console.log(typeof a) // "number"
```

**转换为Boolean**

逻辑计算符（!非），! 对一个布尔值做取反运算；如果对非布尔值取反，则会将其转换为布尔值，再取反；可以利用其特点，将其他数据类型取两次反，转换为布尔值；原理和Boolean()一样；

```js
var a = "123"
a =  !!a  // true
console.log(typeof a) // "Boolean"
```



### ！！ 、     ？？ 、    ？.   

！！就是用来把undefiend、null 转化为false

？？就是用来判断前面的是undefined 或者 null  是就输出后面    null ？？ 1   ==>  1

？. 是用在js语法中判断前面一个属性是否存在  一般用在点寻找属性值





### let、const、var的区别⭐

块级作用域

let 、const存在块级作用域 --- 使用指针 （let ---可以改本指针指向，const不可以改变指针指向）

var的作用域是全局的  可以覆盖前面的声明



### export  、export default 区别

export 可以暴露出多个

export default 只暴露出一个

1. `1.当文件存放着很多方法，变量不同场景需要引用不同方法，请用export`
2. `2.当类只有某几个方法，并且每次引用都需要用到里面的大部分方法，请用export default，`
3. `  毕竟还有方法提示`
4. `3.当值导出一个方法，类请用export default`
5. `4.如果一个文件只会被某一个其他文件的子文件，不会被其他文件引用，并且其中的方法都会被用到，`
6. `  考虑用export default。比如某个业务文件夹下的action.js，用的时候用import api from "./action";`
7. `  方便识别，不用重复在import的{}中添加，也可以用方法提示。`
8. `4.如果一个文件兼有以上需求 可以同时export和export default`



### try 的 catch 和 promise 的 catch 有什么区别

**try… catch… 不能捕获 Promise 里 reject 出来的错误信息**

**Promise 里 reject 出来的错误信息，可用 Promise.catch() 的方法来获取**

```
try {
		Promise.reject('出错了')
		.catch(err => { console.log('2', err) });
		console.log(a);
	} catch (error) {
		console.log(error)
	}
	// ReferenceError: a is not defined
	// 2 出错了
```



### for...in  for...of

1.循环数组

区别一：for in 和 for of 都可以循环数组，for in 输出的是数组的index下标，而for of 输出的是数组的每一项的值。

const arr = [1,2,3,4]

// for ... in
for (const key in arr){
    console.log(key) // 输出 0,1,2,3
    }

// for ... of
for (const key of arr){
    console.log(key) // 输出 1,2,3,4
    }
2.循环对象

区别二：for in 可以遍历对象，for of 不能遍历对象，只能遍历带有iterator接口的，例如Set,Map,String,Array

const object = { name: 'lx', age: 23 }
    // for ... in
    for (const key in object) {
      console.log(key) // 输出 name,age
      console.log(object[key]) // 输出 lx,23
    }

    // for ... of
    for (const key of object) {
      console.log(key) // 报错 Uncaught TypeError: object is not iterable
    }








### ES6 数组新方法

#### **Array.from && newSet()**

Array.from的设计目的是快速便捷把一个类似数组的可迭代对象创建成一个新的数组实例。

通俗的讲，只要一个对象有length，Array.from就能把它变成一个数组，返回新的数组，而不改变原对象。

```js
let likeArr = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(likeArr); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(likeArr); // ['a', 'b', 'c']
```

```js
// String
Array.from('abc'); // ["a", "b", "c"]
// Set
Array.from(new Set(['abc', 'def'])); // ["abc", "def"]
// Map
Array.from(new Map([[1, 'abc'], [2, 'def']])); // [[1, 'abc'], [2, 'def']]
```

Array.from 接收的第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，处理后的值放入返回的数组：

```js
let arrayLike = [1,3,5];
Array.from(arrayLike, x => x+1);        // [2, 4, 6]
// 等同于
Array.from(arrayLike).map(x => x+1);    // [2, 4, 6]
```

#### **Array.reduce(callback)** 

reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

```js
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

| 参数                                      | 描述                                                         |
| :---------------------------------------- | :----------------------------------------------------------- |
| *function(total,currentValue, index,arr)* | *total*必需。*初始值*, 或者计算结束后的返回值。                              *currentValue*必需。当前元素                                                                                            *currentIndex*可选。当前元素的索引                                                           *arr*可选。当前元素所属的数组对象。 |
| *initialValue*                            | 可选。传递给函数的初始值                                     |

```js
arr1 = [1,2,3]
arr1.reduce((t,v)=> t += v)
//6
```

#### **fill()**

使用给定值，填充一个数组。

```js
[1,2,3,4,5].fill('a');
// ["a", "a", "a", "a", "a"]

new Array(3).fill(12)
// [12, 12, 12]
```

fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```js
[1,2,3,4,5].fill('a',2,4);
// [1, 2, "a", "a", 5]
```

注意，如果填充的类型为对象，那么**被赋值的是同一个内存地址的对象**，而不是**深拷贝对象**。

#### copyWithin（）

可以在当前数组内部，将指定位置的数组项复制到其他位置，会覆盖原数组项，然后返回当前数组。使用该方法会修改当前数组。

它接受三个参数：

（1）target（必需）：从该位置开始替换数据。如果为负值，表示倒数。

（2）start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。

（3）end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

这三个参数都应该是数值，如果不是，会自动转为数值。

```reasonml
Array.prototype.copyWithin(target, start = 0, end = this.length)
```

#### Array.of（）

当参数个数大于1时，Array() 才会返回由参数组成的新数组。当参数个数只有一个时，实际上是指定数组的长度。

```stylus
Array() // []

Array(3) // [, , , length = 3] 
//length: 3
//[[Prototype]]: Array(0)

Array(3, 4, 5) // [3, 4, 5]
```



然而 Array.of 弥补了数组构造函数 Array() 的不足

```js
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(3) // [3]
Array.of(3, 4, 5) // [3, 4, 5]
```



### js常用的方法



修改原数组 splice pop unshift  shift  push reverse sort
不修改原数组(返回一个新的数组) slice concat join 

#### Arry.concat() 

连接两个或更多的数组，并返回结果。

#### Arry.join() 

把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。

如果是数组对象，则'[object Object]-[object Object]-[object Object]-[object Object]-[object Object]'，所以一般是用在字符串数组/数字数组

#### Arry.pop() 

删除最后一个元素并返回数组的最后一个元素（**就是返回被删除的元素**）

#### Arry.push() 

向数组的末尾添加一个或更多元素，**并返回新的长度。**

#### Arry.reverse() 

颠倒数组中元素的顺序 ， **并返回结果。**

#### Arry.shift() 

删除并返回数组的第一个元素 ，**返回被删除元素。**

#### Arry.unshift() 

将一个或多个元素添加到数组的**开头**，并返回该数组的**新长度**。（**返回的是长度**）

#### Arry.slice() 

从某个已有的数组返回选定的元素

#### Arry.sort() 

对数组的元素进行排序

#### Arry.splice(start,length,...items) 

删除元素，并向数组添加新元素。

可以向指定位置插入任意数量的项，需要3个参数，  起始位置   、0（要删除的项数）   、要插入的任意数量的项。

如果第二个参数不是0，是1 ，那么就先删除再添加items。



#### String.substr(start,length)      String.substring(start,stop)      String/Arry.slice(start,stop) 

substr()含头含尾，substring()含头不含尾，slice()也是含头不含尾

区别 传参不同 取值方式不同 substr官方考虑废弃，少用。



#### String.padStart(targetLength,string)     String.padEnd(targetLength,string)：

方法用另一个字符串填充当前字符串（如果需要的话，会重复多次），以便产生的字符串达到给定的长度。从当前字符串的左侧开始填充。

console.log(str1.padStart(2, '0'));
// expected output: "05"



#### 字符串转数组三种方式

1. split拆分 "abc".split(' ') ==> ["a","b","c"]   ， split（‘  可以放以什么来分割字符串 ’） split（‘ - ’）

2. [...] [..."abc"] ==> ["a","b","c"] 

​    3. Array.from("abc")  ==> ["a","b","c"]



#### Arry.forEach()

```
forEach(function(element, index, array) { /*    业务   */ }, thisArg)
```

没有返回值 ，不会创建副本。

 `forEach` 不会直接改变调用它的对象，但是那个对象可能会被 `callbackFn` 函数改变。

除了抛出异常以外，没有办法中止或跳出 `forEach()` 循环。如果你需要中止或跳出循环，`forEach()` 方法不是应当使用的工具。

forEach方法跳出循环------通过抛出异常的方式跳出整个循环 通过return跳过当次循环

```js
var arr = [1,3,5,7,9];
var id = 5;
try {
     arr.forEach(function (curItem, i) {
         if(curItem === 1) return;
         console.log(curItem)
         if (curItem === id) {
             throw Error();         //满足条件，跳出循环
         }
     })
 } catch (e) {
 }

//输出 3 5 


map同理
var arr = [1,3,5,7,9];
var id = 5;
try {
     arr.map(function (curItem, i) {
         if(curItem === 1) return;
         console.log(curItem)
         if (curItem === id) {
             throw Error();         //满足条件，跳出循环
         }
     })
 } catch (e) {
 }

//输出 3 5 
```





#### Arry.map()

```
map(function(element, index, array) { /*    业务   */ }, thisArg)
```

返回一个新的数组，前提你得在callback函数里面return，如果你不return东西出去，那么新的数组就是[undefiend，undefiend]

并且需要注意，只要其中一层循环没有return东西出去，那么就是undedfiend。

```
const numbers = [1, 2, 3, 4];
const filteredNumbers = numbers.map((num, index) => {
  if (index < 3) {
    return num;
  }
});

filteredNumbers 是 [1, 2, 3, undefined]
```



#### Arry.some()

返回的是一个布尔类型，只要callback里面返回ture，那么整个循环返回就是ture，并且只要有一个ture，那么就终止循环，直接返回ture。

注意：callback是需要return （ture/false）出去的。



#### Arry.every()

和some一样，区别就在需要全部都满足ture才返回ture，否者也是一旦遇到返回的是false，直接终止循环，返回false。

注意：callback是需要return （ture/false）出去的。



#### Arry.find(callback)

find 返回数组中第一个满足条件的元素（如果有的话）， 如果没有，则返回undefined。

```
const inventory = [
  {name: 'apples', quantity: 2},
  {name: 'bananas', quantity: 0},
  {name: 'cherries', quantity: 5}
];

function isCherries(fruit) {
  return fruit.name === 'cherries';
}

console.log(inventory.find(isCherries));
// { name: 'cherries', quantity: 5 }


const inventory = [
  {name: 'apples', quantity: 2},
  {name: 'bananas', quantity: 0},
  {name: 'cherries', quantity: 5}
];

const result = inventory.find(({ name }) => name === 'cherries');

console.log(result) // { name: 'cherries', quantity: 5 }

```

**这两个实质上都是在判断条件为ture的时候返回了ture，然后这个find方法就返回这一层循环的item。**

```
const inventory = [
    { name: 'apples', quantity: 2 },
    { name: 'bananas', quantity: 0 },
    { name: 'cherries', quantity: 5 }
];

const result = inventory.find(function (item) {
    if (item.name === 'cherries') {
        return 1
    }
});

console.log(result) // { name: 'cherries', quantity: 5 }
```



#### Arry.findIndex(callback)

findIndex 返回数组中第一个满足条件的元素的下标， 如果没有，则返回-1。

同理find。



#### Array.includes(item, finIndex)

方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。

```arcade
[NaN].includes(NaN)
// true
```

includes 第二个参数表示搜索的起始位置：

```js
['a','b','c','d'].includes('c',2)  //true
['a','b','c','d'].includes('c',3)  //false
```

如果第二个参数为负数，则表示从倒数第几位向后搜索：

```js
['a','b','c','d'].includes('c',-1)  //false     (-1指的是倒数第一位'd')
['a','b','c','d'].includes('c',-2)    //true     (-2指的是倒数第二位'c')
```





#### String.indexOf() / Arry.indexOf()

indexof的返回值是找到第一个符合要求的元素的下标    

可以找字符串，也可以找数组里面的元素。

indexof与findIndex的区别就是传参不一样 一个是callback   一个是字符串、数字之类的。

indexOf 方法有两个缺点：

一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于 -1，表达起来不够直观。

二是，**它内部使用严格相等运算符（===）进行判断**，这会导致对 NaN 的误判。

```js
[NaN].indexOf(NaN)
// -1
```

```
let a = '1234657984sfasd'
let b = [1, 2, 5, 8, 7, 7]

console.log(a.indexOf('5')); //5
console.log(b.indexOf(5)); // 2 
```





#### lastIndexOf()

lastIndexOf() 方法可返回一个指定的字符串值最后出现的位置，如果指定第二个参数 start，则在一个字符串中的指定位置从后向前搜索。

第二个参数就是指定在该位置开始往前找

**不一样的地方就是从后面开始找**



#### filter()

主要是用来过滤数据，只需要写条件在 `callbackFn` 里面**返回 true 或 等价于 true 的值**，那么就会直接返回改item。

我觉得和map区别在于，filter只返回符合条件的item（**那么这个时候不符合条件的，就不返回了，所以返回的新数组长度是可能小于原数组的**），map返回的是原数组长度的新数组（**不符合条件的，就undefiend**）

```
let words = ['spray', 'limit', 'exuberant', 'destruction', 'elite', 'present'];

const modifiedWords_filter = words.filter((word, index, arr) => {
    return word.length < 6;
});

const modifiedWords_map = words.map((word, index, arr) => {
    if (word.length < 6) {
        return word
    }
});

console.log(words);
console.log(modifiedWords_filter);
console.log(modifiedWords_map);

[ 'spray', 'limit', 'exuberant', 'destruction', 'elite', 'present' ]
[ 'spray', 'limit', 'elite' ]
[ 'spray', 'limit', undefined, undefined, 'elite', undefined ]

```



### 使用async await 怎么捕获错误或者异常

```
export function async_axios(url,post_data)
{
  return new Promise(function(resolve,reject)
    {
      service.post(url,post_data)
          .then(function(response)
          {
              resolve(response);
          })
          .catch(function(error)
          {
              console.log("https call errror:",error);
              reject(error);
          });
    }
  );
}
```



为什么不用try catch包着await 语句，捕捉异常？

try catch 捕捉异常里面如果有其他语句发生了异常，就不知道具体是谁异常。

那么就直接使用promise包装这个请求方法，用.catch 捕获每一个异常。

如果需要做业务逻辑捕获到异常提示异常信息，就try catch 里面写把。



注意：**能捕捉到的异常，必须是线程执行已经进入 try catch 但 try catch 未执行完的时候抛出来的。**

https://zhuanlan.zhihu.com/p/142932660



### encodeURI这个函数的功能是做什么的呢?

（我知道它是对 URI 进行编码,但是我不知道它在编程时在什么地方使用.）

如果你是通过form提交的,那就不需要用这个了.但是如果是你使用url的方式

例如：ajax提交到后台的,就需要对url进行encodeURI编码,
否则,会导致后台出现各种乱码,不加encodeURI的话,默认浏览器编码格式提交,
这样的话,浏览器不同,传到后台的值也就不同了,
所以建议使用encodeURI统一编码为utf-8的格式到后台,然后后台再处理再解码就行了,
如果后台是utf-8的,好像也可以不手动解码,
但是建议加上解码,避免发布环境不同的时候,会出现问题吧.



### forEach里面不能async await

```js
let array = [{ name: 'jisoo' }, { name: 'lisa' }, { name: 'jennie' }, { name: 'rose' }]
async function func() {

    console.log('start');


    for (const item of array) {
        await test(item)
    }

    array.forEach(async element => {
        await test(element)
    });

    for (let index = 0; index < array.length; index++) {
        await test(array[index])
    }

    console.log('end');

}


func()
```



forEach出来的

```
start
end
{ name: 'jisoo' }
{ name: 'lisa' }
{ name: 'jennie' }
{ name: 'rose' }
```



for循环出来 和 es6新语法 for of 出来的

```
start
{ name: 'jisoo' }
{ name: 'lisa' }
{ name: 'jennie' }
{ name: 'rose' }
end
```



### Promise

https://es6.ruanyifeng.com/#docs/promise





### continue  break 的区别

​	**continue 是跳出当前这个循环**	

​	**break是跳出整个循环** 

 

#### break

```
for(var i=0;i<=10;i++){
		if(i==5){
			break;   //break的作用  当i值等于5时,直接跳出循环,后面的值不输出
		}
		console.log(i)  //输出打印0 1 2 3 4  
	}
```

```
		var num=1;
			switch(num){
				case 1:
					x="1";
				case 2:
					x="2";
				case 3:
					x="3";
				case 4:
					x="4";
				default:
					x="num不存在";	//当没有用到break时输出最后面的值
			}
			console.log(x) //输出 num不存在
			
			少了break;退出整个循环，导致输出了最后一个edfault
```

**注意:break在forEach、map数组循环不能用**。会报错 Illegal break statement（非法中断语句）



#### continue

```
	 for(var i=0;i<=10;i++){
		if(i==5){
			continue; //continue的作用是  跳过i等于5这个判断,继续下面的循环
		}
		console.log(i)  //输出打印 0 1 2 3 4 6 7 8 9 10
	}
```

**注意:continue在switch和forEach、map中不能用**会报错 Illegal break statement（非法中断语句）



### while

```
while (1 == 1) {
    console.log('1')
    continue;
    console.log('2')
    break;
}

无线循环 1   因为 打印了1之后碰到continue跳出这一层循环，while又是永远ture，所以无线循环在 console.log('1')   ----   continue;  之间。


while (1 == 1) {
    console.log('1')
    break;
}

输出 1  结束
```











## CSS

#### **box-sizing **

**box-sizing：content-box|border-box|inherit**

| content-box | 默认值。如果你设置一个元素的宽为 100px，那么这个元素的内容区会有 100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。 |
| :---------- | ------------------------------------------------------------ |
| border-box  | 告诉浏览器：你想要设置的边框和内边距的值是包含在 width 内的。也就是说，如果你将一个元素的 width 设为 100px，那么这 100px 会包含它的 border 和 padding，内容区的实际宽度是 width 减 去(border + padding) 的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。 **注：**border-box 不包含 margin。 |
| inherit     | 指定 box-sizing 属性的值，应该从父元素继承                   |

content-box  就是如果你标签有padding border 的话，那么实际标签的宽度是 标签的width + padding + border 

border-box  在上述条件一样的情况下，你width是多少那么显示的就是多少了。



#### 为什么不给body html 设置100vh高度

那为什么要设置基于百分比的 height，而不改用 vh 单位呢？问题就在于 vh 单元在移动设备上无法正常工作；100vh 将占据超过 100% 的屏幕空间，因为移动浏览器会在浏览器 UI 出现和消失的地方做这件事。



#### .a.b 与 .a .b区别

```
.a.b 与 .a .b

当两个元素名写在一起时，表示当前元素在html中同时出现才会出现效果

当两个元素名之间用空格分隔时，表示后代选择器，选择的是.a内的.b，只有当前的子元素才会出现效果，它们之间属于从属包含关系
用scss less sass 写法就是
.a{
	.b{
	
	}
}
```

https://blog.csdn.net/Alive_m/article/details/127603493

```
//处理描述文字超长的title
//就在需要处理很长文字的标签上面写一个动态class，给他添加一个自己名命的clss（.over-long-title），然后css里面就连写，如下：
/deep/ {
  .over-long-title.van-cell {
    xxxxxxx
  }
}
```

一些其他css选择器

https://blog.csdn.net/dxnn520/article/details/124168144

```
1、>(大于号) 子元素选择器
>大于号的意思是 选择某元素后面的第一代子元素

<style type="text/css">
	h1>strong {
		color: red;
	}
</style>
 
<body>
	<h1>
		<strong>一代子元素</strong>
	</h1>
	<h1>
		<span>
			<strong>二代子元素</strong>
		</span>
	</h1>
</body>

2、~（波浪号）
~波浪号的意思是 选取 某个元素之后的所有相同元素

.box~h2 这句就是 选取 .box 后面所有的 h2

这个选择器 两种元素必须处于同一个父元素内，被选取的元素不必直接紧随。

<style type="text/css">
	.box~h2{
		background: aqua;
	}
</style>
 
<body>
	<div class="box"></div>
	<h2>1</h2>
	<em>2</em>
	<h2>3</h2>
	<h2>4</h2>
</body>
```



#### z-index层级

z - index  是相对的  他只会在同一级的标签才会有用

div{
	p
	h1
	span
}

p h1 span 是同一级的 zindex 才有用  

p h1 span 这几个标签会永远是div的上面/前面



#### CSS选择器及其优先级⭐

- 第一优先级：!important 会覆盖页面内任何位置的元素样式

- 1.内联样式，如 style="color: green"，权值为 1000

- 2.ID 选择器，如#app，权值为 0100

- 3.类、伪类、属性选择器，如.foo, :first-child, div[class="foo"]，权值为 0010

- 4.标签、伪元素选择器，如 div::first-line，权值为 0001

- 5.通配符、子类选择器、兄弟选择器，如*, >, +，权值为 0000

- 6.继承的样式没有权值

  

  选择器

  ```
  id选择器（#box），选择id为box的元素
  
  类选择器（.one），选择类名为one的所有元素
  
  标签选择器（div），选择标签为div的所有元素
  
  后代选择器（#box div），选择id为box元素内部所有的div元素
  
  子选择器（.one>one_1），选择父元素为.one的所有.one_1的元素
  
  相邻同胞选择器（.one+.two），选择紧接在.one之后的所有.two元素
  
  群组选择器（div,p），选择div、p的所有元素
  ```

  

#### 实现元素水平垂直居中的方式⭐

- 利用定位+margin:auto
- 利用定位+margin:负值
- 利用定位+transform
- table布局
- flex布局
- grid布局

#### 利用定位+margin:auto

```
.father{
		position: relative;
        width:500px;
        height:300px;
        border:1px solid #0a3b98;
        
    }
    .son{
    	position: absolute;
        top:0;
        left:0;
        right:0;
        bottom:0;
        margin:auto;
        width:100px;
        height:40px;
        background: #f0a238;
    }
```

#### 利用定位+margin:负值

```
.father {
        position: relative;
        width: 200px;
        height: 200px;
        background: skyblue;
    }
    .son {
        position: absolute;
        top: 50%;
        left: 50%;
        margin-left:-50px;
        margin-top:-50px;
        width: 100px;
        height: 100px;
        background: red;
    }
```



#### 利用定位+transform

```
.father {
        position: relative;
        width: 200px;
        height: 200px;
        background: skyblue;
    }
    .son {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width: 100px;
        height: 100px;
        background: red;
    }
```



#### table布局

```
.father {
        display: table-cell;
        width: 200px;
        height: 200px;
        background: skyblue;
        vertical-align: middle;
        text-align: center;
    }
    .son {
        display: inline-block;
        width: 100px;
        height: 100px;
        background: red;
    }
```



#### flex布局

```
.father {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        background: skyblue;
    }
    .son {
        width: 100px;
        height: 100px;
        background: red;
    }
```



#### gird布局

```
.father {
            display: grid;
            align-items:center;
            justify-content: center;
            width: 200px;
            height: 200px;
            background: skyblue;

        }
        .son {
            width: 10px;
            height: 10px;
            border: 1px solid red
        }
```









#### 画一条0.5px的线⭐

```
height: 1px;
transform: scaleY(0.5);
transform-origin: 50% 100%;
```



#### 隐藏元素的⽅法有哪些 ⭐

visibility:hidden    隐藏  占空间 不触发事件

display:none  不占空间  不响应任何事件

opacity : 0  占空间 会触发事件（如果有绑定）

position: absolute：通过使⽤绝对定位将元素移除可视区域内，以此来实现元素的隐藏。

 z-index: 负值：来使其他元素遮盖住该元素，以此来实现隐藏。 

 transform: scale(0,0)：将元素缩放为 0，来实现元素的隐藏。这种⽅法下，元素仍在⻚⾯中占据 位置，但是不会响应绑定的监听事件。



#### display:none与visibility:hidden的区别 ⭐

display  none  是在整个渲染树中消失的   **节点还在的**  不占位置的   会使元素重排  没有继承

visibility：hidden  只是不显示 但是还是占位置   只会使元素重绘   有继承   子孙如果要显示的话就设置visible



#### display的block、inline和inline-block的区别⭐

（1）block：会独占⼀⾏，多个元素会另起⼀⾏，可以设置width、height、margin和padding属性； （2）inline：元素不会独占⼀⾏，设置width、height属性⽆效。但可以设置⽔平⽅向的margin 和padding属性，不能设置垂直⽅向的padding和margin； 

（3）inline-block：将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对 象会被排列在同⼀⾏内。



#### position 有哪些值，作用分别是什么 ⭐

https://www.ruanyifeng.com/blog/2019/11/css-position.html

- static

  static(没有定位)是 position 的默认值，元素处于正常的文档流中，会忽略 left、top、right、bottom 和 z-index 属性。

- relative

  relative(相对定位)是指给元素设置相对于原本位置的定位，元素并不脱离文档流，因此元素原本的位置会被保留，其他的元素位置不会受到影响。

  **使用场景**：子元素相对于父元素进行定位

- absolute absolute(绝对定位)是指给元素设置绝对的定位，相对定位的对象可以分为两种情况：

  1. 设置了 absolute 的元素如果存在有祖先元素设置了 position 属性为 relative 或者 absolute，则这时元素的定位对象为此已设置 position 属性的祖先元素。

  2. 如果并没有设置了 position 属性的祖先元素，则此时相对于 body 进行定位。

     **使用场景**：跟随图标 图标使用不依赖定位父级的 absolute 和 margin 属性进行定位，这样，当文本的字符个数改变时，图标的位置可以自适应

- fixed 可以简单说 fixed 是特殊版的 absolute，**fixed 元素总是相对于 body 定位的**。 

  **使用场景**：侧边栏或者广告图

- **inherit** 继承父元素的 position 属性，但需要注意的是 IE8 以及往前的版本都不支持 inherit 属性。

- **sticky** 设置了 sticky 的元素，在屏幕范围（viewport）时该元素的位置并不受到定位影响（设置是 top、left 等属性无效），当该元素的位置将要移出偏移范围时，定位又会变成 fixed，根据设置的 left、top 等属性成固定位置的效果。 当元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置。亦即如果你设置了 top: 50px，那么在 sticky 元素到达距离相对定位的元素顶部 50px 的位置时固定，不再向上移动（相当于此时 fixed 定位）。

  **使用场景**：跟随窗口



#### 单⾏、多⾏⽂本溢出隐藏  ⭐

```
//单⾏⽂本溢出
overflow: hidden; // 溢出隐藏
text-overflow: ellipsis; // 溢出⽤省略号显示
white-space: nowrap; // 规定段落中的⽂本不进⾏换⾏

//多⾏⽂本溢出
overflow: hidden; // 溢出隐藏
text-overflow: ellipsis; // 溢出⽤省略号显示
display:-webkit-box; // 作为弹性伸缩盒⼦模型显示。
-webkit-box-orient:vertical; // 设置伸缩盒⼦的⼦元素排列⽅式：从上到下垂直排列
-webkit-line-clamp:3; // 显示的⾏数
```





#### 盒模型 ⭐

标准盒模型的width和height属性的范围只包含了content， 

IE盒模型的width和height属性的范围包含了border、padding和content。

<img src="http://114.132.239.118:3003/getpic/1655961993374.png" alt="http://114.132.239.118:3003/getpic/1655961993374.png" style="zoom:80%;" />

#### 实现⼀个三⻆形⭐

```
div {
width: 0;
height: 0;
border-top: 50px solid red;
border-right: 50px solid transparent;
border-left: 50px solid transparent;
}

实现了倒三角
```



#### 对BFC的理解，如何创建BFC ⭐

BFC是⼀个独⽴的布局环境，可以理解为⼀个容器，在这个容器中按照⼀定规则进⾏物品摆 放，并且不会影响其它环境中的物品。如果⼀个元素符合触发BFC的条件，则BFC中的元素布局不受外 部影响。

创建BFC的条件： 

根元素：body； 

元素设置浮动：float 除 none 以外的值； 

元素设置绝对定位：position (absolute、fixed)； 

display 值为：inline-block、table-cell、table-caption、flex等；

 overflow 值为：hidden、auto、scroll；



#### 怎么让Chrome支持小于12px 的文字

针对谷歌浏览器内核，加webkit前缀，用transform:scale()这个属性进行缩放！

```
span{
        font-size: 12px; //为了一些可以12px一下的网站
        -webkit-transform: scale(0.5);//缩放比例
        display: inline-block; //transform需要块级元素
    }
    
    ==》font-size: 6px;
```



#### 响应式公布方案



https://juejin.cn/post/6844903630655471624#heading-11



媒体查询  bootstrap

```
@media screen and (max-width: 960px){
    body{
      background-color:#FF6699
    }
}

@media screen and (max-width: 768px){
    body{
      background-color:#00FF66;
    }
}

@media screen and (max-width: 550px){
    body{
      background-color:#6633FF;
    }
}

@media screen and (max-width: 320px){
    body{
      background-color:#FFFF00;
    }
}


```

rem

rem单位都是相对于根元素html的font-size来决定大小的,根元素的font-size相当于提供了一个基准，当页面的size发生变化时，只需要改变font-size的值，那么以rem为固定单位的元素的大小也会发生响应的变化。 因此，如果通过rem来实现响应式的布局，只需要根据视图容器的大小，动态的改变font-size即可。

```js
function refreshRem() {
    var docEl = doc.documentElement;
    var width = docEl.getBoundingClientRect().width;
    var rem = width / 10;
    docEl.style.fontSize = rem + 'px';
    flexible.rem = win.rem = rem;
}
win.addEventListener('resize', refreshRem);

```

上述代码中将视图容器分为10份，font-size用十分之一的宽度来表示，最后在header标签中执行这段代码，就可以动态定义font-size的大小，从而1rem在不同的视觉容器中表示不同的大小，用rem固定单位可以实现不同容器内布局的自适应。



#### 如何提高动画的渲染性能

把需要动画的渲染层提到合成层上

那么一个渲染层满足哪些特殊条件时，才能被提升为合成层呢？这里列举了一些常见的情况：

- 3D transforms：translate3d、translateZ 等
- video、canvas、iframe 等元素
- 通过 Element.animate() 实现的 opacity 动画转换
- 通过 СSS 动画实现的 opacity 动画转换
- position: fixed
- 具有 will-change 属性
- 对 opacity、transform、fliter、backdropfilter 应用了 animation 或者 transition



#### css兼容性问题

兼容性就是各个厂家的浏览器内核不一样导致的渲染效果 处理方式 不一样 js css 方面

举个例子  添加监听事件  ie attachEvebt  chrome addEventListener  解决办法就是写一个函数 进行判断 有没有对应的方法el.  分情况监听。

`CSS Hack` 

所有浏览器通用： `height: 100px;` 
IE6 专用： `_height: 100px;` 或者 `*height: 100px;` 
IE7 专用： `*+height: 100px;` 
IE7、FF 共用： `height: 100px !important;` 

css的例子就是不同浏览器的margin padding 默认值是不一样的 。

解决办法就是 CSS: *{margin: 0;padding:0;}

https://juejin.cn/post/6972937716660961317#heading-5



#### link和@import的区别

link引⽤CSS时，在⻚⾯载⼊时同时加载；@import需要⻚⾯⽹⻚完全载⼊以后加载。 

link⽀持使⽤Javascript控制DOM去改变样式；⽽@import不⽀持。



#### 对requestAnimationframe的理解

**CPU节能**：使⽤SetTinterval 实现的动画，当⻚⾯被隐藏或最⼩化时，SetTinterval 仍然在后台执 ⾏动画任务，由于此时⻚⾯处于不可⻅或不可⽤状态，刷新动画是没有意义的，完全是浪费CPU资 源。⽽RequestAnimationFrame则完全不同，当⻚⾯处理未激活的状态下，该⻚⾯的屏幕刷新任务 也会被系统暂停，因此跟着系统⾛的RequestAnimationFrame也会停⽌渲染，当⻚⾯被激活时，动 画就从上次停留的地⽅继续执⾏，有效节省了CPU开销。 

**函数节流**：在⾼频率事件( resize, scroll 等)中，为了防⽌在⼀个刷新间隔内发⽣多次函数执⾏， RequestAnimationFrame可保证每个刷新间隔内，函数只被执⾏⼀次，这样既能保证流畅性，也能 更好的节省函数执⾏的开销，⼀个刷新间隔内函数执⾏多次时没有意义的，因为多数显示器每 16.7ms刷新⼀次，多次绘制并不会在屏幕上体现出来。 

**减少DOM操作**：requestAnimationFrame 会把每⼀帧中的所有DOM操作集中起来，在⼀次重绘或 回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，⼀般来说，这个频率为每 秒60帧。



#### 对 CSSSprites 的理解

CSSSprites（精灵图），将⼀个⻚⾯涉及到的所有图⽚都包含到⼀张⼤图中去，然后利⽤CSS的 background-image，background-repeat，background-position（x,y两个参数设置位置）属性的组合进⾏背景定位。



#### 什么是margin重叠问题？如何解决？

两个块级元素的上外边距和下外边距可能会合并（折叠）为⼀个外边距，其⼤⼩会取其中外边距值⼤的 那个，这种⾏为就是外边距折叠。需要注意的是，浮动的元素和绝对定位这种脱离⽂档流的元素的外边 距不会折叠。重叠只会出现在垂直⽅向。

设置display: inline-block



#### 浏览器的渲染层与合成层

https://juejin.cn/post/6844903966573068301#heading-1

浏览器的图层是分层的 是又很多个渲染层 合成层叠加在一起的，他们除了xy坐标还有z，就是用于区分他们是处于什么层级关系。z-index

##### 渲染层

这是浏览器渲染期间构建的第一个层模型，处于相同坐标空间（z轴空间）的渲染对象，都将归并到同一个渲染层中，因此根据层叠上下文，不同坐标空间的的渲染对象将形成多个渲染层，以体现它们的层叠关系。所以，对于满足形成层叠上下文条件的渲染对象，浏览器会自动为其创建新的渲染层。能够导致浏览器为其创建新的渲染层的，包括以下几类常见的情况：

- 根元素 document
- 有明确的定位属性（relative、fixed、sticky、absolute）

- opacity < 1
- 有 CSS fliter 属性
- 有 CSS mask 属性
- 有 CSS mix-blend-mode 属性且值不为 normal
- 有 CSS transform 属性且值不为 none
- backface-visibility 属性为 hidden
- 有 CSS reflection 属性
- 有 CSS column-count 属性且值不为 auto或者有 CSS column-width 属性且值不为 auto
- 当前有对于 opacity、transform、fliter、backdrop-filter 应用动画
- overflow 不为 visible

##### 合成层

满足某些特殊条件的渲染层，会被浏览器自动提升为合成层。合成层拥有单独的 GraphicsLayer，而其他不是合成层的渲染层，则和其第一个拥有 GraphicsLayer 的父层共用一个。

那么一个渲染层满足哪些特殊条件时，才能被提升为合成层呢？这里列举了一些常见的情况：

- 3D transforms：translate3d、translateZ 等
- video、canvas、iframe 等元素
- 通过 Element.animate() 实现的 opacity 动画转换
- 通过 СSS 动画实现的 opacity 动画转换
- position: fixed
- 具有 will-change 属性
- 对 opacity、transform、fliter、backdropfilter 应用了 animation 或者 transition

因此，文首例子的解决方案，其实就是利用 will-change 属性，将 CPU 消耗高的渲染元素提升为一个新的合成层，才能开启 GPU 加速的，因此你也可以使用 `transform: translateZ(0)` 来解决这个问题。

这里值得注意的是，不少人会将这些合成层的条件和渲染层产生的条件混淆，这两种条件发生在两个不同的层处理环节，是完全不一样的。

##### 隐式合成

这句话可能不好理解，它其实是在描述一个交叠问题（overlap）。举个例子说明一下：

- 两个 absolute 定位的 div 在屏幕上交叠了，根据 `z-index` 的关系，其中一个 div 就会”盖在“了另外一个上边。



![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/9/16daf0c0a871e2e9~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



- 这个时候，如果处于下方的 div 被加上了 CSS 属性：`transform: translateZ(0)`，就会被浏览器提升为合成层。提升后的合成层位于 Document 上方，假如没有隐式合成，原本应该处于上方的 div 就依然还是跟 Document 共用一个 GraphicsLayer，层级反而降了，就出现了元素交叠关系错乱的问题。



![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/9/16daf0c0a89f88c8~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



- 所以为了纠正错误的交叠顺序，浏览器必须让原本应该”盖在“它上边的渲染层也同时提升为合成层。



![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/9/16daf0c0aa2976c4~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



#### z-index

z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。

Z-index 仅能在定位元素上奏效（例如 position:absolute;）

需要在同一级标签

```
<style>
    .box1 {
        width: 100px;
        height: 100px;
        background-color: black;
    }
    .box2 {
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: rgb(153, 1, 1);
        z-index: 99;
    }
    .box3 {
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: rgb(38, 161, 209);
        z-index: 999;
    }
</style>

<body>
    <div class="box1">
        <div class="box2"></div>
        <div class="box3"></div>
    </div>
</body>
```



#### pointer-event属性详解

https://blog.csdn.net/qq_37600506/article/details/99487744

多用在地图  背景水印的应用场景  

1、 任何元素设置pointer-event：none的效果相当于input[type=text|button|radio|checkbox] 设置disabled 属性，可以实现事件的禁用，例如：

    <a href="xxxxxx" style="pointer-events: none">click me</a>
    
    这个链接，是点不了的，并且 hover 也没有效果,但是可以通过tab来选中该元素，并按下enter键来触发链接，当href属性去掉，就不能通过tab进行触发

2、  当要禁用select下拉框可以设置pointer-event:none。

3、  当很多元素需要定位在一个地图层上面，需用到绝对定位、相对定位的元素，这样的话，这些元素就会盖住下面的地图层。

以至于地图层无法操作。这时元素设置 pointer-events: none，然后地图就可以拖动和点击了。但是操作区域本身却无法操作了，可以再给需要操作的元素区域设置为 pointer-events:auto。



#### 图片与文字对齐  

```
 1. line-height: revert;

 2. flex

 3. vertical-align:middle;
```



#### flex-basis

flex-basis  指定了 flex 元素在主轴方向上的初始大小

```

```

#### CSS 背景图片 宽度100% 高度自适应

```
width: 100%;
	height: 100%;
	background-image: url("../../public/img/bg.png");
	background-size: 100% auto;
```



#### vertical-align

vertical-align 属性设置元素的垂直对齐方式。

baseline	默认。元素放置在父元素的基线上。
sub	垂直对齐文本的下标。
super	垂直对齐文本的上标
top	把元素的顶端与行中最高元素的顶端对齐
text-top	把元素的顶端与父元素字体的顶端对齐
middle	把此元素放置在父元素的中部。
bottom	把元素的顶端与行中最低的元素的顶端对齐。
text-bottom	把元素的底端与父元素字体的底端对齐。
length	 
%	使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。
inherit	规定应该从父元素继承 vertical-align 属性的值。

```
经常用在两个子标签的高度是不一样的，那么默认是底部靠最下面，如果这个时候需要居中，靠顶部的效果，就可以用这个
```

![http://114.132.239.118:3003/getpic/1672233754462.png](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\Snipaste_2022-12-28_21-22-25.png)

```
还有就是图片和文字居中对齐
```





## HTML

#### src与href的区别？⭐

src 一般用在script （同步下载的）img  iform

href 一般用在 link （异步下载的）



#### 对HTML语义化的理解⭐

通俗来讲就是⽤正确 的标签做正确的事情。

语义化标签  foot head aside



#### ⾏内元素有哪些？块级元素有哪些？ 空(void)元素有那些？⭐

⾏内元素有： a b span img input select strong ； 

块级元素有： div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p ；

空元素：br hr img input link meta

#### 事件代理（事件委托） 

事件委托的原理：不给每个子节点单独设置事件监听器，而是设置在其父节点上，然后利用冒泡原理设置每个子节点。

**优点：**

- 减少内存消耗和 dom 操作，提高性能 在 JavaScript 中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能，因为需要不断的操作 dom,那么引起浏览器重绘和回流的可能也就越多，页面交互的事件也就变的越长，这也就是为什么要**减少 dom 操作**的原因。每一个事件处理函数，都是一个对象，多一个事件处理函数，**内存**中就会被多占用一部分空间。如果要用事件委托，就会将所有的操作放到 js 程序里面，只对它的父级进行操作，与 dom 的操作就只需要交互一次，这样就能大大的减少与 dom 的交互次数，提高性能；
- 动态绑定事件 因为事件绑定在父级元素 所以新增的元素也能触发同样的事件

但是使用事件委托也是存在局限性：

- `focus`、`blur`这些事件没有事件冒泡机制，所以无法进行委托绑定事件
- `mousemove`、`mouseout`这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的



#### 什么是 DOM 和 BOM？

DOM 指的是文档对象模型，提供了操作页面元素节点的方法和属性

BOM 指的是浏览器对象模型 ， 提供一些属性和方法可以操作浏览器

![http://114.132.239.118:3003/getpic/1655630652321.png](http://114.132.239.118:3003/getpic/1655630652321.png)

#### window.onload用法

用于在网页加载完毕后立刻执行的操作，即当 HTML 文档加载完毕后，立刻执行某个方法。

用于 <body> 元素，在页面完全载入后(包括图片、css文件等等)执行脚本代码。



#### mouseover 、mouseout 、 mouseenter 、mouseleave 

```
一、 mouseover 和mouseenter的区别

mouseover: 只要鼠标指针移入事件所绑定的元素或其子元素，都会触发该事件
mouseenter: 只有鼠标指针移入事件所绑定的元素时，才会触发该事件

换句话说就是，如果一个元素没有子元素，那么该元素绑定mouseover或者mouseenter两种事件效果没有区别，鼠标每次移入元素时都只会触发一次事件；如果绑定了mouseover事件的元素存在子元素，那么，每次移入该元素时都会触发一次事件（包括从外部移入和从子元素移入），移入子元素时也会触发一次事件。

一个是鼠标自动在绑定的元素或者子元素就可以触发，一个是必须移动在绑定的元素才能触发

1、mouseover和mouseout会有事件冒泡，也就是说鼠标移入、移出当前元素的子元素或父元素时都会触发该事件。
2、mouseenter和mouseleave 事件不会冒泡，依旧是说鼠标移入、移出时，单签元素的子元素或父元素不会触发该事件。


二、事件传播的机制(冒泡和捕获), 使用代码来验证, 事件冒泡和事件捕获的区别

事件捕获（event capturing）：当鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件。
事件冒泡（dubbed bubbling）：与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点

事件冒泡： 多个元素嵌套，有层次关系，这些元素都注册了相同的事件，如果里面的元素的事件触发了， 外面的元素事件自动触发。由内向外。
事件捕获： 多个元素嵌套，有层次关系，这些元素都注册了相同的事件，如果里面的元素的事件触发了， 外面的元素事件自动触发。由外向内。
addEventListener("没有on的事件类型","事件处理函数","控制事件阶段") 事件控制阶段中 ：false：冒泡阶段 true：捕获阶段
e.eventPhase 判断事件阶段(冒泡和捕获不能同时出现)

三、阻止事件的默认行为, 事件冒泡和事件捕获可以阻止吗? 怎么阻止?
阻止事件的传播：
• 在W3c中，使用stopPropagation（）方法
• 在IE下设置cancelBubble = true；
在捕获的过程中stopPropagation（）；后，后面的冒泡过程也不会发生了~


阻止事件的默认行为，例如click <a>后的跳转~
• 在W3c中，使用preventDefault（）方法；
• 在IE下设置window.event.returnValue = false;

1.setInterval的 this指向
setInterval的 this指向 是window
2. 怎么设置自定义属性
自定义属性：element.setAttributet("属性 ") 设置属性
3.怎么阻止事件冒泡阻止事件冒泡： e.stopPropagation(); 谷歌火狐阻止事件冒泡的方法
e.cancelBubble = true; IE8及以前版本浏览器阻止冒泡的方法
```













## 同源 跨域

同源的概念 协议 ip 端口 要相同  

同源出现的原因是，为了安全，防止一些攻击，例如：你不小心触发了非法分子的陷阱，向非法分子的服务器发送一个请求，这个请求可能拿着你被偷的数据，这个时候同源就是出来了，浏览器不允许发送不同源的请求，就是说浏览器不允许你发送不在你规定好了的访问里面的请求。





## 浏览器



### 路由history 与 hash

hash是通过#后面跟哈希值进行路由跳转  每一次修改#后面的url  实际是触发了hashchange事件，hash的url改变是不会向服务器发送请求的。

history url改变就会向服务器发送请求，所以需要后端配置一下，否者会404。

history 没有# 好看一点  是通过history api进行路由控制的  go forward back等等方法。

window.pre。 拿到浏览器的上一个路由地址



### 获取URL地址

document.referrer 获取上一页的来源地址

document.URL 获取当前页面地址。URL必须大写

### h5在只能在微信中打开的限制

```
// var ua = navigator.userAgent.toLowerCase();
  //       var isWeixin = ua.indexOf('micromessenger') != -1;
  //       if (!isWeixin) {
  //         if(to.fullPath =='/error'){
  //           next();
  //         }else{
  //           next('/error')
  //         }
  //       }else{
  //         next();
  //       }
  
  可以写在路由守卫那
```



### window.replace('xxxxx') 重定向





## 网络





## Http



### http1与http2区别？⭐

http2使用都是**二进制格式**传输请求头数据体

可**多路复用**  复用同一个tcp 客户端和服务器可以在一个tcp连接里面多次发送请求和回应 避免阻塞

**压缩头部信息**  客户端和服务器一起维护同一张头部信息表 ，所有字段都会存到这样表里面，以后发送同样的字段就直接发送对应的索引。

**服务器可以直接推送静态资源**

**数据流** http2数据包是不按顺序发送的 每一个数据包都有唯一的标识id



连接网络请求都需要通过tcp的三次握手建立连接，第四次握手断开。

长连接就是复用tcp，在一个tcp里面多次请求答应

短连接就是请求完响应完就断开。下一次又需要三次握手连接。



### 手写ajax网络请求⭐

```
function XHR(url) {
    //1. new xhr 对象
    let xhr = new XMLHttpRequest()
    //2. 调用open 创建http请求
    xhr.open('get', url, true)
    //3.添加监听事件 成功 失败
    xhr.onreadystatechange = function () {
        //当对象的 readyState 变为 4 的时候，代表服务器返回的数据接收完成，这个时候可以通过判断请求的状态，如果状态是 2xx 或者 304 的话则代表返回正常。这个时候就可以通过 response 中的数据来对页面进行更新了。
        if (this.readyState !== 4) return;
        if (this.status === '200') {
            handle(this.response)
        } else {
            //log
        }
    }
    xhr.onerror = function () {
        //log
    }
    //4.设置响应数据类型
    xhr.responseType = 'json'
    //5.设置请求头
    xhr.setRequestHeader('Accept', 'applaction/json')
    //6.send
    xhr.send(null)
}


//promise 封装
function getJson(url) {
    return new Promise((resolve, reject) => {
        let xhr = new XMLHttpRequest()
        xhr.open('get', url, true)
        xhr.onreadystatechange = function () {
            if (this.status !== 4) return false
            if (this.status === 200) {
                resolve(this.response)
            } else {
                reject(this.statusText)
            }
        }
        xhr.onerror = function () {
            reject(this.statusText)
        }
        xhr.responseType = 'json'
        xhr.setRequestHeader('Accept', 'appleaction/json')
        xhr.send(null)
    })
}

```



### ⻚⾯有多张图⽚，HTTP是怎样的加载表现？

在 HTTP 2 下，可以⼀瞬间加载出来很多资源，因为，HTTP2⽀持多路复⽤，可以在⼀个TCP连接中 发送多个HTTP请求。

### HTTP2的头部压缩算法是怎样的？

HTTP2的头部压缩是HPACK算法。在客户端和服务器两端建⽴“字典”，⽤索引号表示重复的字符串，采 ⽤哈夫曼编码来压缩整数和字符串，可以达到50%~90%的⾼压缩率。





### http常用状态码

- 200 OK   正常返回信息 
- 301 Moved Permanently  请求的网页已永久移动到新位置。
- 400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。 
- 401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应
- 404 Not Found  找不到如何与 URI 相匹配的资源。  
- 403 forbidden   无权访问此资源HTTP的状态代码
- 500 Internal Server Error  最常见的服务器端错误。 
- 503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。



### LocalStorage、SessionStorage 区别

两者都是大概能存储5m左右大小的  存储到客户端

两者都需要遵守同源策略

localStorage 可以永久的存储下去，除非用户手动删除   同源文档之间可以共享localstorage资源

sessionStorage 一旦窗口或者标签页被删除 ，sessionStorage 就会被删除  。sessionStorage需要同一窗口，同源文档才可以通讯。



### session  cookie区别

session  存放到服务器

cookie 存放到客户端 浏览器



### WebSocket

https://juejin.cn/post/6844903544978407431#heading-0

https://juejin.cn/post/6844903539974602759



### HTTP 缓存机制

https://mp.weixin.qq.com/s/d2zeGhUptGUGJpB5xHQbOA





## 其他

### qs   JSON 区别 

#### （1）qs.stringify()将对象转化为&连接的字符串url

#### （2）qs.parse()将字符串url转化为对象，但属性值均为string类型

#### （3）JSON.stringify()将对象、数组、普通类型的变量转换为JSON数据格式



### BLOB   FILE  BASE64  之间的互相转换

 

obj 可以是 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 对象、[`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象或者 [`MediaSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaSource) 对象

```js
let fileUr = window.URL.createObjectURL(obj);
      this.option.img = fileUr;
      onload = function() {
        // 手动回收
        URL.revokeObjectURL(fileUrl);
      };
```





### 两个数组 找出来两者相同的部分 

两个for循环嵌套

简单的实现就是for循环嵌套  ----- 复杂度高

时间复杂度 ： n*n ---->  2n

```
let innerPlayList = [];
        data.result.songs.some(({ id }) => {
          return this.innerPlayList.some((item) => {
            if (id === item.id) {
              innerPlayList.push(item);
              return true;
            }
          });
        });
```



```
let arr2Map = {};
arr2.forEach((item) => arr2Map[item.id] = item) 
arr1.forEach((item) => {
arr2Map[item.id] && (result.push(item))
})
```



### 输入框input做防抖 

```vue
//方法一：直接input事件防抖
<el-input
	placeholder="请输入内容"
	prefix-icon="el-icon-search"
	v-model="searchValues"
	@input="debounceWatch"
	>
</el-input>

<script>
	import {_debounce} from xxxx;
    data(){
        searchValues: null,
        debounceWatch: null,
    },
    created() {
    //保存这一个防抖函数到data,如果不存储在data里面，那么就会重复去构建一个新的_debiune,防抖就实效了。
    this.debounceWatch = _debounce(() => {
      //do something
    }, 500);
  }
</script>

//方法二：用this.$watch监听searchValues
<el-input
	placeholder="请输入内容"
	prefix-icon="el-icon-search"
	v-model="searchValues"
	>
</el-input>

<script>
	import {_debounce} from xxxx;
    data(){
        searchValues: null,
        unwatch：null,
    },
    created() {
        //this.$watch调用后会返回一个值，就是unWatch方法，你要注销 watch 只要调用unWatch方法就可以了。
   		this.unwatch = this.$watch('searchValues', _debounce(() => {
      		//do something
    	}, 500);
 	 }
    beforeDestroy() {
    // 移除监听
    this.unwatch && this.unwatch()
  }
</script>
```

```vue
//失效的例子
<el-input
	placeholder="请输入内容"
	prefix-icon="el-icon-search"
	v-model="searchValues"
	@input="debounceWatch"
	>
</el-input>
<script>
	import {_debounce} from xxxx;
   methods: {
       //input 每一次触发 都是触发里面_deboune的匿名函数 每一次都是新的 所以实效
       //_debounce是只用了闭包，所以返回的是一个函数。
    debounceWatch(){
        _debounce(() => {
      //do something
    	}, 500)();
    }
  }
</script>
```



### vue 里面使用 v-html 插入的文本不换行的问题解决

```
1、添加样式
在使用 v-html 时添加样式，white-space:pre-wrap ,让浏览器保留空白和换行符。

<p v-html="text" style="white-space:pre-wrap"></p>

2、用 pre 标签包裹
被包围在 pre 标签中的文本通常会保留空格和换行符。

<pre><p v-html="text"></p></pre>

3、正则替换
用正则表达式把 \n 替换成
这样 v-html 就可以识别

<p v-html="text.replace(/\n/g,'<br/>')"></p>

```



# 后端

## sql



### ifnull函数 ：

 ifnull（第一个参数，第二个参数） 先会去判断第一个参数的字段名字在表中是不是为空，如果为空就返回第二个参数，那么就可以在第二个参数里面写一个业务逻辑，例如：修改默认值，没有时间的时候就可以写入当前时间等等。如果不为空，那么就是第一个参数的值了。



### concat(参数1，参数2，参数3 .....)  就可以用sql拼接字段了

可以使用concat拼接一些字段 再like 进行模糊搜索



### LIKE 

like ‘%啊%’  ------   啊是在中间 进行模糊搜索的

like ‘啊%’  ------   啊是在后面   进行模糊搜索的

like '啊_'  ------- 啊后面就找一个任意字符



% 与 _ 的区别就是可以匹配的任意字符个数不同；例如：我需要找姓许的两个字的名字，那么就like ‘许_’  ，如果是like ‘许%’  ，他就找到许xx、许xxxx 。。。三个字以及以上的结果出来。





### 表的连接 

https://www.runoob.com/w3cnote/sql-join-image-explain.html



![微信截图_20220823235430](C:\Users\lovelife_xu\Desktop\微信截图_20220823235430.png)





### distinct  

去除重复行  这个如果你 * 取全部 ，可能去除不了 ；他是根据selsect 后面的需要拿出的字段  这些字段如果有重复的  才去掉重复的。

可以distinct其中一个列名？？  不可以  因为需要select产出的一列列做比较

### group by

group by 分组一定要注意的是，你group by了多少个字段，你select前面就需要有这些字段。

group by 分组你有多少个字段就以这么多个字段分组，



###  UCASE() 

UCASE() 函数把字段的值转换为大写。

SELECT UCASE(column_name) FROM table_name;



### DROP

DROP  可以删除视图





### datediff（）

当天 进入库的数据

login_time 需要是datetime/其他是日期的数据类型

datediff(login_time,now()) = 0





### max（）   min（）  

里面的字段一般是int类型/时间类型  ，一般用来判断这两个字段类型的。



### limit

**用法**：【select * from tableName limit i,n 】

参数：

- tableName : 为数据表；
- i : 为查询结果的索引值（默认从0开始）；
- n : 为查询结果返回的数量

limit 放最后



### desc 降序  asc升序



### having

having的用法 ：大白话就是先通过[sql语句](https://so.csdn.net/so/search?q=sql语句&spm=1001.2101.3001.7020)把所有数据查询出来，再用 group by 进行分组，然后把分完组的数据用聚合函数进行统计，只不过查询语句和聚合函数之间需要用having连接；（group by 、having、聚合函数通常一起使用）



### sql时间范围

时间范围的比较sql ， 可以是datetime等等的时间类型 ，也可以是varchar的统一格式的类型。

例如 ： 2000-02-08  需要相同类型的varchar格式  才可以比较 ，其实他是从左到右比较大小。

startTime >  ?      and   endTime  <  ?   语句是这样写的。





### update  sql  语句写法

update  表名 set 列名1= 'value1', 列名2= 'value2', 列名3= 'value3' where 条件;





### not  /    in



为什么要使用 IN 操作符？其优点如下。  在有很多合法选项时，IN 操作符的语法更清楚，更直观。  在与其他 AND 和 OR 操作符组合使用 IN 时，求值顺序更容易管理。  IN 操作符一般比一组 OR 操作符执行得更快（在上面这个合法选项很 少的例子中，你看不出性能差异）。  IN 的最大优点是可以包含其他 SELECT 语句，能够更动态地建立 WHERE 子句。第 11 课会对此进行详细介绍



为什么使用 NOT？对于这里的这种简单的 WHERE 子句，使用 NOT 确实 没有什么优势。但在更复杂的子句中，NOT 是非常有用的。例如，在 与 IN 操作符联合使用时，NOT 可以非常简单地找出与条件列表不匹配 的行



sql   xxx  in (  需要多个 )  可以用通配符这样写   xxxx in （ ？， ？，？，，，，）



### mod(100,2)  求余  0





在同时使用 ORDER BY 和 WHERE 子句时，应该让 ORDER BY 位于 WHERE 之后，否则将会产生错误





### IS NULL   

是空 ，这里的null 不是 0 / ‘ ’ 空字符串  ，是允许null的null



mysql 里面 varchar 字符串的数据类型 ，如果数据存储的值后面有一个空格，（xxxx ），你在sql找这个字段的值的时候可以不需要最后也有空格，mysql检索引擎会自动去掉空格，只是去掉在最后这个情况的空格，在前面，或者中间都是不会自动去掉再匹配的。

子句 WHERE prod_name LIKE  'F%y'只匹配以 F 开头、以 y 结尾的 prod_name。如果值后面跟空格， 则不是以 y 结尾，所以 Fish bean bag toy 就不会检索出来。简单 的解决办法是给搜索模式再增加一个%号：'F%y%'还匹配 y 之后的字 符（或空格）。更好的解决办法是用函数去掉空格。





sql 里面可以使用 运算符进行计算 ，例如

```
SELECT prod_id,
 quantity,
 item_price,
 quantity * item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008; 
```







说明：HAVING 和 WHERE 的差别  这里有另一种理解方法，WHERE 在数据分组前进行过滤，HAVING 在数 据分组后进行过滤。这是一个重要的区别，WHERE 排除的行不包括在 分组中。这可能会改变计算值，从而影响 HAVING 子句中基于这些值 过滤掉的分组。



### INSERT

```
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)

INSERT INTO table_name (列1, 列2,...) VALUES ( 表 select 但是字段顺序个数得对上)
```



### truncate

(一) truncate使用场景
   truncate主要的使用场景是用于删除某张数据库表中的全部数据。

(二) 使用语法
  truncate table [表名] 
  例子：
      truncate table user_table;  // 重置user_table表结构并清除user_table表所有数据

(三）truncate和delete的区别

  1. truncate 和 delete 都可以清除数据表的所用数据，但 truncate 比 delete 的速度快，且使用的
     系统和事务日志资源少。
  2. truncate没有 where 的条件筛选，只能单独使用，delete 不仅可以单独而且还可以和 where 搭配，
     从而实现删除单条或多条数据。
  3. 删除的实现原理不同。truncate 是通过释放存储表数据所使用的数据页来进行数据的删除，并且只在
     事务日志中记录页的释放。delete 语句每删除一行就在事务日志中为所删除的每行记录一项。



### sql 做统计

可以用group by 分组  再在select 使用 count计数  ，并且count（可以添加条件 case when then else end）

也可以sum 累加  统计总数

```
SELECT
	b.MPWZHM as address,
	b.SQMC as community_name,
	b.JZMC as street_name,
	b.grid as grid_name,
	b.grid_code,
	b.uuid,
	COUNT(case when a.vehicle_type = 1 then 1 else null end ) as ljmpfjdcs, 
	COUNT(case when a.vehicle_type = 2 then 1 else null end ) as ljmpjdcs, 
	COUNT(case when (a.vehicle_type = 1 or a.vehicle_type = 2) then 1 else null end ) as ljmpcs, 
	COUNT(case when (a.vehicle_type = 1 AND to_days(now()) - to_days(create_date) <= 1) then 1 else null end ) as drmpfjdcs, 
	COUNT(case when (a.vehicle_type = 2 AND to_days(now()) - to_days(create_date) <= 1) then 1 else null end ) as drmpjdcs, 
	COUNT(case when ((a.vehicle_type = 1 or a.vehicle_type = 2)AND to_days(now()) - to_days(create_date) <= 1) then 1 else null end ) as drmpcs 
FROM
	standard_mp b
	LEFT JOIN (SELECT car_plate_number ,  community_name,  street_name , grid_code , mlpdz , create_date ,vehicle_type  FROM tb_550_vehicle_touch_check a WHERE state = 1 AND (a.vehicle_type = 1 or a.vehicle_type = 2) GROUP BY car_plate_number ) a ON b.uuid = a.mlpdz 
WHERE
	b.SYZT = 3 
	AND b.ZXZT = 0
	AND b.XZQMC = '白云区' 
GROUP BY
	b.uuid
```





去重的三个方法

https://blog.csdn.net/qq_46416934/article/details/126080141



1.**distinct**

2.**group by**

3.row_number() over (parttion by 分组列 order by 排序列)   



### sql按照一个字段的某个值进行排序  例如state（1 2 3 4）

按照 2 3 1 进行排序

```
。。。。。order by 
	case `state` 
	when 2 then 1		// 当值为2，排在第一个
	when 3 then 2		// 当值为1，排在第二个
	when 1 then 3		// 当值为3，排在第三个
	end
asc		// 按上面顺序，正序排列（也可为desc）
```



### exists 用于判断

```
SELECT *
from a1
where exists (select * from 表 a2 where a2.字段 = a1.字段 )

只有当exists里面的子查询有返回行，exists就是ture，反之false。为ture的时候才会select a1表的数据出来。
not exists 就是反过来了。
```



### sql判断数据是否当日（时间用法）

```
create_time BETWEEN CONCAT(CURDATE(),' 00:00:00') AND CONCAT(CURDATE(),' 23:59:59')
```

```
to_days( now()) - to_days( a.create_date ) <= 1
```





### 数据库注意事项

1.数据库查询，如果一直在查没有停，说明在占用的数据库的线程，超过几分钟的就可以停了，容易死锁。







一、基础技巧

```sql



--2、记录与记录将有关联显示
select a.score, IF(@back <= a.score, @rank:= @rank, @rank:= @rank + 1) as rank, @back:=a.score
from (select s.score
   from scores s
   order by s.score desc) a, (select @rank:=1, @back:=-1) b
   
   
--3、explain的时候需要注意三个字段：id、type、rows


--4、in关键字的使用可以多字段匹配单条记录，与exists（也就是第一个技巧）可以转化
select d.`name`, e.`name`, e.salary
from employee e
inner JOIN department d on e.departmentId = d.id
where (e.departmentId, e.salary) in (
					select e.departmentId, MAX(e.salary)
					from employee e
					GROUP BY e.departmentId
)


--5、ORDER BY 可以添加两个字段做到组内排序的效果，通过@变量获取上下字段关系，达到获取前几项记录的效果
select a.department as Department, a.employee as Employee, a.salary as Salary
from (
			select s.*, IF(@dep=s.departmentId,IF(@sal > s.salary,@rank:=@rank+1,@rank),@rank:=1) as 'rank',
						 @dep:=s.departmentId, @sal:=s.salary
			from (select d.`name` department, e.departmentId, e.`name` as employee, e.salary 
						from employee e
						left join department d on e.departmentId = d.id) s,
						(select @rank:=1, @sal:='', @dep:='') t
			ORDER BY s.departmentId, s.salary desc
) a
where a.rank <= 3


--6、不能查询的同时操作表，需要将查询的数据缓存起来
DELETE FROM person
where id not in (
					select t.id 
					from (select MIN(p.id) id
					from Person p
					GROUP BY p.email) t
)


--7、count()计数可以加条件，但是条件有技巧。如果直接添加他还是计算全部，因为当条件不成立的时候返回false，而COUNT(false)会计算全部的值，但是如果在条件的最后一行添加一个or null，当前面的条件不成立的时候就会到null，null是不会计数的
select t.request_at, COUNT(t.`status` = 'cancelled_by_driver' or t.`status` = 'cancelled_by_client' or null)
from trips t
GROUP BY t.request_at
```

二、函数发现

```sql
--1、时间的计算（增加或者减少几天几秒等）
--DATE_ADD(date,INTERVAL expr type)
--date：合法的日期表达式（就是正常的date属性的数据）
--INTERVAL：必填项，不要动
--expr：是希望添加的时间间隔
--type：时分秒或者天等

--增加
select w1.id
from weather w1
where EXISTS (SELECT 1 
							FROM weather w2 
							where DATE_FORMAT(DATE_ADD(w1.recordDate,INTERVAL 1 DAY),'%Y-%m-%d') = w2.recordDate
							and w1.temperature < w2.temperature)
							
--减少
select w1.id
from weather w1
where EXISTS (SELECT 1 
							FROM weather w2 
							where DATE_FORMAT(DATE_SUB(w1.recordDate,INTERVAL 1 DAY),'%Y-%m-%d') = w2.recordDate
							and w1.temperature > w2.temperature)


--2、两个时间相差
DATEDIFF(weather.date, w.date)


--3、除法保留小数
--第一种方法：DECIMAL(P，D)表示列可以存储D位小数的P位数。P是表示有效数字数的精度。 P范围为1〜65。D是表示小数点后的位数。 D的范围是0~30。MySQL要求D小于或等于(<=)P。
select convert(t/100,decimal(15,2)) as money from test

--第二种方法：TRUNCATE(x,d)：函数返回被舍去至小数点后d位的数字x。若d的值为0，则结果不带有小数点或不带有小数部分。若d设为负数，则截去（归零）x小数点左起第d位开始后面所有低位的值。
SELECT TRUNCATE(t/100,2) as g from test
```



### 在sql里面里面进行求百分比，要注意除数不能为0

就是使用case when then else 做判断

所以可以在select 写 case  when a = 0 then ’0‘ when b = 0 then ’0‘  else TRUNCATE(a/b,2)





### sql性能优化

查询条件中没有索引时，count()比count(1)查询速度要快些。

查询条件中有索引时，count(1)比count(*)查询速度要快些。









## nodejs框架



### expressjs



#### express同名路由，触发第一个

```
app.get('/getpic1', (req, res) => {

 console.log(1)

})

app.get('/getpic1', (req, res) => {

 console.log(2)

})

express 后端  路由 这样的   相同的地址路径 相同的  先执行第一个  所以 log ---1 
```



#### path.join(__dirname, '..', dir);   path 路径问题  resolve

https://blog.csdn.net/qq_51066068/article/details/125877033



#### cluster添加线程

nodejs单线程 开多线程反正值得宕机     

https://juejin.cn/post/6844903968624082958#heading-2

https://www.gxlsystem.com/qianduan-15708.html





router路由思想：每当一个请求到达服务器之后，需要先经过路由的匹配，只有匹配成功之后，才会调用对应的处理函数。在匹配时，会按照路由的顺序进行匹配，如果请求类型和请求的 URL 同时匹配成功，则 Express 会将这次请求，转交给对应的 function 函数进行处理。





next()  ：因为express是使用中间件/路由的思想，next就是表示跳转到下一个middle或者route函数。

但是如果已经res.send('')的话，那么next就没用了。请求已经终结了。





express 热启动 静态文件部署 跨域解决 调试

http://t.zoukankan.com/juexin-p-6831392.html





连接数据库查询数据的时候，返回的数据需要对返回的数据做res.length == 0 的判断 等于0的时候就需要send 查不到数据。防止下面代码的报错。



express  的 moment 时间模块   深入了解一下





#### module.exports和exports

NodeJs开发者建议导出对象用module.exports,导出多个方法和变量用exports

后续补充代码eg





# 服务器



## Nginx

什么是nginx？

​		Nginx是一个 轻量级/高性能的反向代理**Web服务器**，他实现非常高效的反向代理、负载平衡，他可以处理2-3万并发连接数，官方监测能支持5万并发，现在中国使用nginx网站用户有很多，例如：新浪、网易、 腾讯等。

Nginx的应用场景

1. http服务器：Nginx是一个http服务可以独立提供http服务。可以做网页静态服务器。
2. 虚拟主机：可以实现在一台服务器虚拟出多个网站。例如个人网站使用的虚拟主机。基于端口的，不同的端口；基于域名的，不同域名
3. 反向代理，负载均衡：当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用nginx做反向代理。并且多台服务器可以平均分担负载，不会因为某台服务器负载高宕机而某台服务器闲置的情况。

### 正向代理



在如今的网络环境下，如果我们需要**梯子**到外网查资料，那么这个梯子就是正向代理的意思。

**正向代理最大的特点是客户端明确要访问的服务器地址**；服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端；**正向代理模式屏蔽或者隐藏了真实客户端信息。**

**正向代理就是客户端明确知道需要访问的目标网站，通过正向代理让正向代理的服务器去访问这个网站，再把服务器请求转交给客户端。**

**正向代理，"它代理的是客户端，代客户端发出请求"**

正向代理的用途：
（1）访问原来无法访问的资源，如Google
（2） 可以做缓存，加速访问资源
（3）对客户端访问授权，上网进行认证
（4）代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

### 反向代理

这一个图片就可以明白什么叫做反向代理了。

![img](https://images2018.cnblogs.com/blog/1202586/201804/1202586-20180406175939873-925019958.png)

多个客户端给服务器发送的请求，Nginx服务器接收到之后，按照一定的规则分发给了后端的业务处理服务器进行处理了。此时~请求的来源也就是客户端是明确的，但是请求具体由哪台服务器处理的并不明确了，Nginx扮演的就是一个反向代理角色。

反向代理，"**它代理的是服务端，代服务端接收请求**"，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。

反向代理的作用：
（1）保证内网的安全，通常将反向代理作为公网访问地址，Web服务器是内网
（2）负载均衡，通过反向代理服务器来优化网站的负载





参考链接：https://www.cnblogs.com/wcwnina/p/8728391.html



## pm2

### pm2 常用命令行

```
以下是pm2常用的命令行

$ pm2 start app.js              # 启动app.js应用程序

$ pm2 start app.js -i 4         # cluster mode 模式启动4个app.js的应用实例     # 4个应用程序会自动进行负载均衡

$ pm2 start app.js --name="api" # 启动应用程序并命名为 "api"

$ pm2 start app.js --watch      # 当文件变化时自动重启应用

$ pm2 start script.sh           # 启动 bash 脚本


$ pm2 list                      # 列表 PM2 启动的所有的应用程序

$ pm2 monit                     # 显示每个应用程序的CPU和内存占用情况

$ pm2 show [app-name]           # 显示应用程序的所有信息


$ pm2 logs                      # 显示所有应用程序的日志

$ pm2 logs [app-name]           # 显示指定应用程序的日志

$ pm2 flush


$ pm2 stop all                  # 停止所有的应用程序

$ pm2 stop 0                    # 停止 id为 0的指定应用程序

$ pm2 restart all               # 重启所有应用

$ pm2 reload all                # 重启 cluster mode下的所有应用

$ pm2 gracefulReload all        # Graceful reload all apps in cluster mode

$ pm2 delete all                # 关闭并删除所有应用

$ pm2 delete 0                  # 删除指定应用 id 0

$ pm2 scale api 10              # 把名字叫api的应用扩展到10个实例

$ pm2 reset [app-name]          # 重置重启数量


$ pm2 startup                   # 创建开机自启动命令

$ pm2 save                      # 保存当前应用列表

$ pm2 resurrect                 # 重新加载保存的应用列表

$ pm2 update                    # Save processes, kill PM2 and restore processes

$ pm2 generate                  # Generate a sample json configuration file


$ pm2 deploy app.json prod setup    # Setup "prod" remote server

$ pm2 deploy app.json prod          # Update "prod" remote server

$ pm2 deploy app.json prod revert 2 # Revert "prod" remote server by 2


$ pm2 module:generate [name]    # Generate sample module with name [name]

$ pm2 install pm2-logrotate     # Install module (here a log rotation system)

$ pm2 uninstall pm2-logrotate   # Uninstall module

$ pm2 publish                   # Increment version, git push and npm publish
```



## redis db pool 

负载均衡 

https://www.csdn.net/tags/NtTaUg4sNzI1Ny1ibG9n.html

redis 还可以优化数据线的查询数据的效率



hai没有完全理解 再看一下  只是收录进来





## aksk身份验证

https://www.cnblogs.com/xiaomengniu/p/16174330.html







# 工作经验

## -S  -D

- **`-S`** 与 **`--save`** 的简写，安装包信息会写入 **`dependencies`** 中    生产需要的依赖
- **`-D`** 与 **`--save-dev`** 的简写，安装包写入 **`devDependencies`** 中      开发中需要的依赖



## 旧项目安装依赖问题

下载的项目 下下来之后 安装依赖有问题就直接把package 、package。lock 对应的下载失败的删掉 直接按需按版本单独下载



npm cache clean --force  清除npm缓存



跑项目可能出错的问题，node版本的兼容问题，你先用node10跑这个项目，换了node12就可能跑不了这个项目了。



css-loader 经常下载错误版本  出现 512xxxxxxxxxxx  直接复制对应的package、package-lock再npm i



npm 淘宝镜像 可能会使得下载一些依赖有问题，所以安装cnpm 选择性用淘宝还是用官方

淘宝镜像设置与官方配置

https://www.jianshu.com/p/d763e88e15fe



## npm  yarn  使用说明

https://blog.csdn.net/qq_37974755/article/details/124475338





## registry不到服务器

```
registry Unexpected warning for https://registry.npmjs.org/: Miscellaneous Warning ETIMEDOUT: request to https://registry.npmjs.org/vue-loader failed, reason: connect ETIMEDOUT 104.16.23.35:443        
npm WARN registry Using stale data from https://registry.npmjs.org/ due to a request error during revalidation.
npm ERR! code ETIMEDOUT
npm ERR! errno ETIMEDOUT
npm ERR! network request to https://registry.npmjs.org/@babel%2fplugin-transform-property-literals failed, reason: connect ETIMEDOUT 104.16.21.35:443
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'
```

```
在 npm@5 中，npm 缓存通过将完整性不匹配视为缓存丢失来自修复损坏问题。
因此，从缓存中提取的数据保证是有效的。如果你想确保一切都是一致的，那就用 npm cache verify 吧。
删除缓存只会让 npm 运行得更慢，而且不太可能纠正你可能遇到的任何问题!

另一方面，如果你在调试安装程序的问题，或者依赖于写入空缓存的时间的竞争条件，
你可以使用 'npm install --cache /tmp/empty-cache' 来使用一个临时缓存，而不是使用实际的缓存。
如果您确定要删除整个缓存，请使用 '--force' 重新运行此命令。
```



就是网络连接不上，就是超时太慢了

换成淘宝镜像或者yarn

```
npm config set registry https://registry.npm.taobao.org
```





## sass-node      sass-loader    的undeifend、或者一些文件找不到。

sass-node    

sass-loader

如果报错this.options not defined 那就是版本的问题 版本过高了

还有的是注意sass-node     sass-loader   less-loader node sass 这些版本要对应一下，这个百度吧。



## 图片 返回200 但是不显示 ，可以下载该图片 ，用记事本打开。







## vscode设置git记住账号和密码

在Gitbash中第一次拉取代码之后,输入git config --global credential.helper store即可
会记住你的用户名和密码



## git 经验    

git 如果直接拉去发现不可以直接，说明本地有修改，那么这个时候就可以先提交代码到本地 ，不要push上去，然后再拉去代码，再去解决冲突，没有冲突就可以直接push了。



解决冲突

https://blog.csdn.net/weixin_44694660/article/details/115600275

注意第一次pull发现有冲突，那就先提交代码，不push（推送上去）

再次pull（拉取）下来，如果有冲突，那么就会提示请解决冲突再提交。

等你解决完冲突了，再提交对应的代码（这里注意，可能会有一些你完全没改过的代码，又出现在这里），

提交代码，然后再push。







## vue模板中图片应用

在vue 模板里面写 三目 src 的话 ，引用到图片是需要用require（）进行引入图片的

![http://114.132.239.118:3003/getpic/1662552779346.jpg ](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1662552779346.jpg)





## vant 图片预览 不显示 

 因为有可能 他默认挂载在html上  zindex是2001  



## 页面回到顶部方法，一些滚动实际业务

**vue的指令**

```
import Vue from 'vue'

// 滚动到顶部(指令实现)
const ScrollTopByInstruct = Vue.directive('ScrollTopByInstruct', {
    update(el, binding) {
        console.log('binding.value',binding.value);
        console.log('binding.oldValue',binding.oldValue);
        if(binding.value != binding.oldValue){
            el.scrollTop = 0
        }
    }
})

export default {
    ScrollTopByInstruct
}

```



use

```
import ScrollTopByInstruct from '@/utils/scrollTop.js'
Vue.use(ScrollTopByInstruct);
```



tmp

```
    <div class="page-content" ref="flagToScrollTop0" v-ScrollTopByInstruct = flagTest>
```

只需要v-ScrollTopByInstruct = flagTest  绑定一个data的值，改变这个值的值就可以触发了。我是使用ture false触发的。





**element ui table 添加数据行后滚动条滚动到对应的行头或行尾问题**

滚动到第一行：

this.$refs.table.bodyWrapper.scrollTop =0;

滚动到最后一行：

this.$refs.table.bodyWrapper.scrollTop =this.$refs.table.bodyWrapper.scrollHeight;



**vue router 可以设置路由跳转，自动滚动到具体xy坐标**

```
const router = new VueRouter({
  routes:[...],
  //scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
  scrollBehavior: (to, from, savePosition) => {
    // return 期望滚动到哪个的位置
    //  to：跳转到哪个页面
    // from：来自哪个页面
     //savePosition:位置
    // 常规的一种做法，直接禁止滚动行为，每个页面切换时自动回到顶部
    return {x: 0, y:0}
  }
})

```







## vant组件（以及其他组件）注意一些组件的绑定些事件

一个小注意的点，使用vant组件（以及其他组件），如果有进行vue的双向绑定的，就要注意了，组件如果你使用了一些事件是判断v-model的改变了就触发的事件，例如：radio的change事件，一旦v-model改变了就会触发change事件。但是这个change事件绑定的方法里面有一些操作是我不想它触发的。例如：新建的页面和修改的页面是同一个页面，通过api接口返回数据然后在初始化到页面，这个时候触发了change事件 导致我返回需要初始化的数据被change事件给覆盖了。



## vant 的日历  使用的是Date()类型

vant 的日历  使用的是Date()类型     所以可以直接用new Date()进行时间转换  再date的方法就可以拿到年月日

注意new Date().getTime()返回的是时间戳  是毫秒



## **inclues**多用用法

传进后端的一些参数 有一些是需要做校验的  [ 1 ,  2 , 3 ].inclues(valuse)  规定了values只可以传 1 2 3这三个参数，就不给它传其他可能进行攻击的注入了。

也可以使用inclues做一些权限判断之类的  

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1667978083738.png)



## parentInt

使用parentInt（）出来可能是NaN  所以也要判断这种 情况



## normalize.css在vue中使用

http://t.zoukankan.com/javahr-p-13608232.html

normalize.css可以理解为别人写的一个框架兼容各种浏览器的css不一致问题，相当于引入base.css文件重置一些样式。但是这个框架做了一些处理，更好的兼容和效果之类的吧。



## 有一些项目会现在图片大小的，图片的引用如果大于xxkb，会被拦截调的。

可以放到assets下面，assets文件夹下的东西是不被webpack这样的打包框架处理的。
可以设置为背景图，在css设置url
可以用svg图片格式



## Vue中使用e-icon-picker 图标选择组件

https://blog.csdn.net/qq_40323256/article/details/116834901



## Vue 父组件中触发子组件的方法

https://blog.csdn.net/guokexiaohao/article/details/113730983
1.通过$refs.xxx.xxxx()   推荐使用这一种，比较方便

2.发射this.$emit()出去



## 主题换颜色，直接给body根标签修改不同的theme，然后使用sass写好不同主题的样式，

```
.theme1 {


  .ovo {
    background: $theme1MainColor;
  }

  .el-button--text {
    color: $theme1MainColor;
  }

  .el-button--primary {
    background: $theme1MainColor;
    border-color: $theme1MainColor;
  }

  .el-button--default {
    border-color: $theme1MainColor;
    color: $theme1MainColor;
  }

  .el-pagination.is-background .el-pager li:not(.disabled).active {
    background: $theme1MainColor;
  }

  .el-menu {
    background: $theme1MainColor;
  }


  .sidebar-container {

    background-color: $theme1MainColor;

    & .nest-menu .el-submenu > .el-submenu__title,
    & .el-submenu .el-menu-item {
      background-color: $theme1MainColor !important;

      &:hover {
        background-color: $subMenuHover !important;
        color: $menuActiveText !important;

        i {
          color: $menuActiveText !important;
        }
      }
    }

    .el-submenu__title {
      background: $theme1MainColor; //侧栏菜单颜色
    }

    .menu-wrapper {
      .el-submenu {

        &.is-opened {
          .el-submenu__title {
            background: rgba(255, 255, 255, .3);
            border-bottom: rgba(255, 255, 255, .3);

            span, i {
              color: #fff !important; //选中的文字
            }
          }
        }
      }
    }

    .el-menu-item.is-active {
      background: rgba(255, 255, 255, .5) !important;
    }


    .submenu-title-noDropdown,
    .el-submenu__title {

      &:hover {
        background-color: rgba(255, 255, 255, .2);
        color: $menuActiveText;

        i {
          color: $menuActiveText;
        }
      }
    }


    .nest-menu {
      .el-menu-item {
        &:hover {
          background-color: rgba(255, 255, 255, .2) !important;
          color: $menuActiveText;

          i {
            color: $menuActiveText;
          }
        }
      }
    }

    .el-submenu.is-active .el-submenu__title {
      border-bottom-color: rgba(255, 255, 255, .5) !important;
    }

    .el-scrollbar {
      background-color: $theme1MainColor;
    }
  }

  .sidebar-logo-container {
    background: $theme1MainColor !important;
  }
}
```



## 懒加载组件

除了在vue router里面写路由懒加载之外，还可以在vue文件引入组件的时候也可以使用require懒加载组件。

![http://114.132.239.118:3003/getpic/1667978083183.png](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1667978083183.png)



## Vue重新设置data中对象的根属性后，为什么内部的对象还是响应式？

https://juejin.cn/post/6854573208310415374

原因是vue在给data中已经初始化好的对象进行set赋值的时候，还会通过observe方法去判断这个新值是否是对象，而且是否已经有响应式初始化。如果没有，重新对其进行响应式初始化。



## 在axios接口参数url写../路径

```
#VUE_APP_BASE_URL="//192.168.0.108:3504/sms/api"
VUE_APP_BASE_URL="//studentdemo.gz-bc.cn/sms/api/"
```

![http://114.132.239.118:3003/getpic/1668155436039.png](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1668155436039.png)

![http://114.132.239.118:3003/getpic/1668155435603.png](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1668155435603.png)



```
拼起来就是这样的=> http://studentdemo.gz-bc.cn/sms/api/../upload_file/upload
实际../就会找上一级 ==> 所以变成 http://studentdemo.gz-bc.cn/sms/upload_file/upload

也可以这么理解，计算机在遇到这个代码的时候'http://studentdemo.gz-bc.cn/sms/api/../upload_file/upload'，它就会去处理../ ，所以最后返回出来的是http://studentdemo.gz-bc.cn/sms/upload_file/upload
```







## 为什么不使用keep-alive  router-link   

https://zhuanlan.zhihu.com/p/513641725

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1668073955298.png)



1. 注意页面很多/很重的情况，可能会引起内存泄漏，可设置max大小。
2. 注意数据需要更新情况，则mounted和activated都需要调用。思考下游的流程是否影响已缓存页面的数据展示，忽略容易产生bug。**在刷新数据时，避免不要清空数据而导致页面的抖动。**（一个是回流性能不好，用户视觉上更不好）
3. keep-alive只解决是否需要留活，如果一个页面留活，以后操作跳转到同一路由的页面则使用缓存，不想使用都不行。这个问题就严重了。比如用户点击导航或面包削等类似场景，都需要一个全新的页面。

没发现好的解决方案，那只能需要先找到keep-alive内部缓存对象做清理操作（vue2: $options.parent.cache-沈锋贡献，vue3：方法之一是可获取keep-alive的ref后取到cache）



## 直接this.xxxx = 赋值方法

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\1667978083458.png)

因为最后method也是挂载到this上面的，也是this上面的一个属性。



## 组件

http://114.132.239.118:3003/getpic/1668155435933.png

![img](http://114.132.239.118:3003/getpic/1668155435933.png)

组件的通信技巧

https://www.cnblogs.com/aimee2004/p/15776454.html

[https://v2.cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6](https://v2.cn.vuejs.org/v2/guide/components-custom-events.html#sync-修饰符)

```
 <component v-for="(item, i) in group.propertys" :is="transformComponent(item.control)" v-bind.sync="item" :key="i"/>
v-bind.sync="item"  快速的把整个对象以props的形式传进去。
然后子组件如果需要传出去
在子组件中，props 中 使用 title，然后修改 title 为新的值，并通知父组件：
this.$emit('update:title', newTitle)
这里只需要this.$emit('update:xxx', newValue) , xxx就是传进来的props对应的某一个。这里是title。
```





## 需求可能是一个文字，点击后查看大图，但又不想引入其他npm插件

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\image-20221123171209711.png)





```
<template>
    <div>
        <el-button @click="onPreview">预览</el-button>
        <!-- 图片查看器(兼容ie) -->
        <el-image
          ref="preview"
          class="hideImgDiv"
          :src="url"
          :preview-src-list="[url]"
          z-index="9999"
        ></el-image>
    </div>
</template>
<script>
    export default {
      name: 'Index',
      data() {
        return {
          url:'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg'
        }
      },
      methods: {
        onPreview() {
          //调用image组件中的大图浏览图片方法
          this.$refs.preview.clickHandler();
        },
      }
</script>
css:

/*隐藏el-image图片组件中不需要展示的的img标签*/
.hideImgDiv {
  /deep/ .el-image__inner {
    display: none;
  }
}
```



## 表单的请求数据类型 处理方法：

```
      三种都可以处理表单数据，一般就直接用后两者其一
      
      // const param = new URLSearchParams();
      // Object.entries(config.data).forEach(item => {
      //   param.append(item[0], item[1])
      // })
      // config.data = param

      // config.transformRequest = [data => {
      //   const formData = new FormData()
      //   if (data) {
      //     Object.entries(data).forEach(item => {
      //       formData.append(item[0], item[1])
      //     })
      //   }
      //   return formData
      // }]

      //const formData = new FormData()
      //Object.entries(config.data).forEach(item => {
      //  formData.append(item[0], item[1])
      //})
      //config.data = formData
```



```
ransformRequest：
表示允许在向服务器发送前，修改请求数据
使用要求：
1、只能用在 ‘PUT’, ‘POST’ 和 ‘PATCH’ 这几个请求方法
2、后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

transformResponse：
在传递给 then/catch 前，允许修改响应数据
transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

```









## qrcode组件     转义出来二维码图片

https://blog.csdn.net/weixin_43840289/article/details/122595249



## v-model小技巧

v-model绑定的值，会自动补充到对象中，如果对象没有先写好这个变量。例如：

```
<el-input v-model="model.signTitle" placeholder="请输入签到主题" clearable></el-input>
data里面已经定义了 model：{
    //但是没有定义signTitle
}
这个时候可以这样写，只要input输入了xxxx，那么model：{
    signTitle：xxxx
}
但是如果v-model=“signTitle” ，data有没有定义signTitle，按这样是会报错的。
所以清除表单form的时候可以直接清除了赋值{}空对象
```



## let a = [{a:1},{a:2}]  变成  [1, 2]

```
[{a:1},{a:2}].map(({a})=>a)   这里使用map循环加上{}解构、箭头函数省略return关键字
```



## 使用vscode 1. 报在签出前,请清理储存库工作树. 2.拉取代码报错

http://t.zoukankan.com/niluiquhz-p-14734281.html



## h5调用微信api

https://blog.csdn.net/nannanWEB/article/details/103493898



## js捕获到除200之外的错误 怎么拿到返回的提示?

接口返回的状态码不是200，就是除200之外的状态码，有一些提示他是返回200，但是有一些错误，所以就需要有逻辑判断，reject出去

axios处理错误，默认行为是拒绝返回状态码不在2xx范围内的所有响应，并将其视为错误。

直接在响应拦截拦截

```
axios.interceptors.response.use(
  response => {//2xx走这
    if (一些逻辑判断) {
      return response
    } else {
      return Promise.reject(response.data.msg)
    }
  },
  error => {//除了2xx之外的走这
    if (error.response) {//逻辑判断
      // 请求已发出，但服务器响应的状态码不在 2xx 范围内
      return Promise.reject(error.response.data.msg)
    } else {
      return Promise.reject(error)
    }
  }
)

```



## Prefix string too short报错 解决办法

formData()对象去append文件类型的时候（file/blob），要注意如果出现上传的文件名字是很短的，那么后端使用了一些类可能会报Prefix string too short，这个时候你就需要在append进去的时候，给append写第三个参数，这个参数是文件的名字。

```
data.append("myfile", myBlob, "filename.txt");
使用 append() 方法时，可以通过第三个可选参数设置发送请求的头 Content-Disposition 指定文件名。如果不指定文件名（或者不支持该参数时），将使用名字“blob”。
```



## html渲染富文本的文字

```
<div class="text" style="white-space: pre-wrap;" v-if="!isSoloOrAllCommont && commentAllText" v-html="commentAllText.replace(/\n/g,'<br/>')"></div>

style="white-space: pre-wrap;" 让浏览器保留空格
.replace(/\n/g,'<br/>')"  把换行变成br

<pre class="text" style="white-space: pre-wrap;" v-if="!isSoloOrAllCommont && commentAllText" >{{commentAllText}}</pre>

<pre>  替换v-html，v-html容易被注入。
```



## application/json-常见的post提交数据的方式

```
在http协议中规定了GET、HEAD、POST、PUT、DELETE、CONNECT 等请求方式,其中比较常用的就是post和get，其中post用来向服务器提交数据，post只规定了提交的数据必须放在请求的主体中，但是并没有规定传输数据的编码方式。比较主流的有如下的几种编码方式。

1.application/x-www-form-urlencoded
最常见的请求方式，特别是自己在测试后端接口时，经常在前端url中直接以键值对的形式写入参数的值。但是该方式默认采用URLencode编码会导致消息包大，form表单默认以该方式提交，请求一般是如下的方式：

POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8
 
title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3

2.multipart/form-data
也是比较常用的提交表单的方式，既可以上传键值对也可以上传文件，因为有boundary的隔离可以上传多个文件，举例如下：

POST http://www.example.com HTTP/1.1
Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryrGKCBY7qhFd3TrwA
 
------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="text"
 
title
------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="file"; filename="chrome.png"
Content-Type: image/png
 
PNG ... content of chrome.png ...
------WebKitFormBoundaryrGKCBY7qhFd3TrwA--

3.application/json
application/json 这个 Content-Type 也是非常常见的，越来越多的人使用该方式传递，该方式传递的是序列化后的字符串，因为采用的是JSON格式的数据，因此支持更多复杂的类型。请求体一般如下：

{
    "username":"admin",
    "password":"admin",
}

4.text/xml
基于XML—PRC的编码方式，协议简单，功能页足够日常的使用JS也有类库使用，但是XML的格式还是过于臃肿，一般场景用JSON更为方便。
```



# vscode快捷键

折叠所有区域代码的快捷键：

先按下ctrl和K，再按下ctrl和 0; (注意这个是零，不是欧)  不是九宫格的0



展开所有折叠区域代码的快捷键： 用的少

先按下ctrl和K，再按下ctrl和 J



ctrl + shift + [    折叠区域代码

ctrl + shift + ]    展开区域代码

ctrl +  k     shift + [    折叠所有子区域代码

ctrl + k      shift + ]    展开所有子区域代码





ALT +  上下   移动当前行到上下，整行移动

shitf +  上下   选中  相当与鼠标左键长按拖拽选中

ctrl  + 上下  就是页面的上下移动



## vscode  debug 操作

https://blog.csdn.net/shentian885/article/details/123896536



debug配置

```
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "vuejs: chrome",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceFolder}/m/src",
            "breakOnLoad": true,
            "sourceMapPathOverrides": {
            "webpack:///src/*": "${webRoot}/*"
             }
        }
    ]
}


```







# 规范



后端在接受一些参数（有规定只可以传某一些值的参数）需要做限制

```
if ([undefined, null, ""].includes(value)) {
  //  业务
}

if ([1, 2, 3].includes(value)) {
  //  业务
}
```





代码规范

https://github.com/airbnb/javascript

https://tgideas.qq.com/doc/index.html







# 工具代码

## 格式化时间格式

```
把 yyyy-m-d  、 yyyy/m/d 转化为  yyyy-mm-dd
formatTime(date) {
      //date 格式一般进来都是 yyyy-m-d   yyyy/m/d
      // 是 yyyy-mm-dd 不要传进来
      let dateArry = date.split("-");
      let returnDate = "";

      for (const item of dateArry) {
        if (item.length == 1) {
          returnDate += "-0" + item;
        } else if (item.length == 2) {
          returnDate += "-" + item;
        } else {
          returnDate += item;
        }
      }
      return returnDate;
    },
```

 

 获取yyyy-mm-dd格式的时间func  

```
												加八小时是因为toJSON（）方法
var myDate = new Date((new Date).getTime() + 8*60*60*1000);

 var time = myDate.toJSON().split('T').join(' ').substr(0,19);
 
 //time   '2022-10-16 23:03:47'
```



获取yyyy-mm-dd格式的时间前后xx时间的时间格式 

```
当前时间的前一个月  之类的
											  + - x个月/日/年的时间戳
var myDate = new Date((new Date).getTime() + 8*60*60*1000);
```



```js
获取  2022年10月16日 23:19:18

export function getYMDHMS(timestamp) {

    let innerTimestamp = parseInt(timestamp)
    
    !isNaN(timestamp) && (return '格式错误')

    let time = null
    //有传值
    timestamp && (time = new Date(innerTimestamp));
    //没有传值，就用当前时间搓
    !timestamp && (time = new Date());

    let year = time.getFullYear();
    const month = (time.getMonth() + 1).toString().padStart(2, "0");
    const date = time
        .getDate()
        .toString()
        .padStart(2, "0");
    const hours = time
        .getHours()
        .toString()
        .padStart(2, "0");
    const minute = time
        .getMinutes()
        .toString()
        .padStart(2, "0");
    const second = time
        .getSeconds()
        .toString()
        .padStart(2, "0");

    return (
        year +
        "年" +
        month +
        "月" +
        date +
        "日 " +
        hours +
        ":" +
        minute +
        ":" +
        second
    );
}


获取 2022年10月16日 

export function getYMD(timestamp) {

    let innerTimestamp = parseInt(timestamp)
    
    !isNaN(timestamp) && (return '格式错误')

    let time = null
    //有传值
    timestamp && (time = new Date(innerTimestamp));
    //没有传值，就用当前时间搓
    !timestamp && (time = new Date());

    let year = time.getFullYear();
    const month = (time.getMonth() + 1).toString().padStart(2, "0");
    const date = time
        .getDate()
        .toString()
        .padStart(2, "0");
    return year + "年" + month + "月" + date + "日 ";
}


获取  今天23:19:18

export function getYestaryToday(timestamp) {
    
   !isNaN(timestamp) && (return '格式错误')
    
  //判断是昨天的还是今天的消息  通过 日 来比较
  let Time = new Date(parseInt(timestamp));
  //当前日
  const nowDate = new Date()
    .getDate()
    .toString()
    .padStart(2, "0");
  //数据日
  const date = Time.getDate()
    .toString()
    .padStart(2, "0");

  let values;
  if (nowDate == date) {
    values = "今天";
  } else {
    values = "昨天";
  }
  const hours = Time.getHours()
    .toString()
    .padStart(2, "0");
  const minute = Time.getMinutes()
    .toString()
    .padStart(2, "0");
  const second = Time.getSeconds()
    .toString()
    .padStart(2, "0");
  return values + hours + ":" + minute + ":" + second;
}
```



//将时间格式为 ‘2022 - 07 - 21T06: 00: 22.000 + 0000’转化为’yyyy - MM - dd HH: mm: ss’或者’yyyy / MM / dd HH: mm: ss’

```
formatDate(oldDate) {

  // 方式1 转换为'yyyy-MM-dd HH:mm:ss'

  function add0(num) { return num < 10 ? '0' + num : num } // 个位数的值在前面补0

  const date = new Date(oldDate)

  const Y = date.getFullYear()

  const M = date.getMonth() + 1

  const D = date.getDate()

  const h = date.getHours()

  const m = date.getMinutes()

  const s = date.getSeconds()


  const dateString = Y + '-' + add0(M) + '-' + add0(D) + '  ' + add0(h) + ':' + add0(m) + ':' + add0(s)

  return dateString

  // 方式2 转换为'yyyy/MM/dd HH:mm:ss'
  // return new Date(oldDate).toLocaleString()

},
```



## 工具安装

git 安装流程
https://blog.csdn.net/weixin_47638941/article/details/120632890

nvm切换nodejs版本
https://blog.csdn.net/weixin_44132277/article/details/124345575

git可视化界面 小乌龟
https://blog.csdn.net/qq_52719788/article/details/124766040

GitLens
可以把git修改的记录直接显示在行后面





# 正则表达式

match与exec区别  https://juejin.cn/post/6844903828374945799

 

.*    匹配除了换行以外的任意字符   这个很容易造成性能的问题  回溯地狱了



```
验证手机号：^1[3456789]\d{9}$
```



```
汉字 至少2个字：^[\u4e00-\u9fa5]{2,}$
```



```
Email地址：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
```

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\a1b551b3ede4afc701780da4811bb37.png)



```
18位身份证号码(数字、字母x结尾)：^((\d{18})|([0-9x]{18})|([0-9X]{18}))$
```

![](C:\Users\lovelife_xu\Desktop\笔记总结\笔记图片\385616ce518d96c46bdca9dfdd6925c.png)

```
帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$
```



**注意：**

```
var regExp = new RegExp('(^\\d{15}$)|(^\\d{18}$)|(^\\d{17}(\\d|X|x)$)');
```

使用RegExp方法创建正则校验对象的时候，里面的正则是需要转义的，例如：\d  是要 \\\d

但是如果你直接使用 /     /   包着正则的话，就不需要了

```
/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/.test(440182200005071210)
```

**一、校验数字的表达式**

```
 1 数字：^[0-9]*$
 2 n位的数字：^\d{n}$
 3 至少n位的数字：^\d{n,}$
 4 m-n位的数字：^\d{m,n}$
 5 零和非零开头的数字：^(0|[1-9][0-9]*)$
 6 非零开头的最多带两位小数的数字：^([1-9][0-9]*)+(.[0-9]{1,2})?$
 7 带1-2位小数的正数或负数：^(\-)?\d+(\.\d{1,2})?$
 8 正数、负数、和小数：^(\-|\+)?\d+(\.\d+)?$
 9 有两位小数的正实数：^[0-9]+(.[0-9]{2})?$
10 有1~3位小数的正实数：^[0-9]+(.[0-9]{1,3})?$
11 非零的正整数：^[1-9]\d*$ 或 ^([1-9][0-9]*){1,3}$ 或 ^\+?[1-9][0-9]*$
12 非零的负整数：^\-[1-9][]0-9"*$ 或 ^-[1-9]\d*$
13 非负整数：^\d+$ 或 ^[1-9]\d*|0$
14 非正整数：^-[1-9]\d*|0$ 或 ^((-\d+)|(0+))$
15 非负浮点数：^\d+(\.\d+)?$ 或 ^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$
16 非正浮点数：^((-\d+(\.\d+)?)|(0+(\.0+)?))$ 或 ^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$
17 正浮点数：^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$ 或 ^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$
18 负浮点数：^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$ 或 ^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$
19 浮点数：^(-?\d+)(\.\d+)?$ 或 ^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$
```

**二、校验字符的表达式**

```
 1 汉字：^[\u4e00-\u9fa5]{0,}$
 2 英文和数字：^[A-Za-z0-9]+$ 或 ^[A-Za-z0-9]{4,40}$
 3 长度为3-20的所有字符：^.{3,20}$
 4 由26个英文字母组成的字符串：^[A-Za-z]+$
 5 由26个大写英文字母组成的字符串：^[A-Z]+$
 6 由26个小写英文字母组成的字符串：^[a-z]+$
 7 由数字和26个英文字母组成的字符串：^[A-Za-z0-9]+$
 8 由数字、26个英文字母或者下划线组成的字符串：^\w+$ 或 ^\w{3,20}$
 9 中文、英文、数字包括下划线：^[\u4E00-\u9FA5A-Za-z0-9_]+$
10 中文、英文、数字但不包括下划线等符号：^[\u4E00-\u9FA5A-Za-z0-9]+$ 或 ^[\u4E00-\u9FA5A-Za-z0-9]{2,20}$
11 可以输入含有^%&',;=?$\"等字符：[^%&',;=?$\x22]+
12 禁止输入含有~的字符：[^~\x22]+
```

**三、特殊需求表达式**

```
 1 Email地址：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
 2 域名：[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(/.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+/.?
 3 InternetURL：[a-zA-z]+://[^\s]* 或 ^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$
 4 手机号码：^(13[0-9]|14[0-9]|15[0-9]|16[0-9]|17[0-9]|18[0-9]|19[0-9])\d{8}$ (由于工信部放号段不定时，所以建议使用泛解析 ^([1][3,4,5,6,7,8,9])\d{9}$) 5 电话号码("XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX)：^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$ 
 6 国内电话号码(0511-4405222、021-87888822)：\d{3}-\d{8}|\d{4}-\d{7}  7 18位身份证号码(数字、字母x结尾)：^((\d{18})|([0-9x]{18})|([0-9X]{18}))$ 8 帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$
 9 密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：^[a-zA-Z]\w{5,17}$
10 强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$  
11 日期格式：^\d{4}-\d{1,2}-\d{1,2}
12 一年的12个月(01～09和1～12)：^(0?[1-9]|1[0-2])$
13 一个月的31天(01～09和1～31)：^((0?[1-9])|((1|2)[0-9])|30|31)$ 
14 钱的输入格式：
15    1.有四种钱的表示形式我们可以接受:"10000.00" 和 "10,000.00", 和没有 "分" 的 "10000" 和 "10,000"：^[1-9][0-9]*$ 
16    2.这表示任意一个不以0开头的数字,但是,这也意味着一个字符"0"不通过,所以我们采用下面的形式：^(0|[1-9][0-9]*)$ 
17    3.一个0或者一个不以0开头的数字.我们还可以允许开头有一个负号：^(0|-?[1-9][0-9]*)$ 
18    4.这表示一个0或者一个可能为负的开头不为0的数字.让用户以0开头好了.把负号的也去掉,因为钱总不能是负的吧.下面我们要加的是说明可能的小数部分：^[0-9]+(.[0-9]+)?$ 
19    5.必须说明的是,小数点后面至少应该有1位数,所以"10."是不通过的,但是 "10" 和 "10.2" 是通过的：^[0-9]+(.[0-9]{2})?$ 
20    6.这样我们规定小数点后面必须有两位,如果你认为太苛刻了,可以这样：^[0-9]+(.[0-9]{1,2})?$ 
21    7.这样就允许用户只写一位小数.下面我们该考虑数字中的逗号了,我们可以这样：^[0-9]{1,3}(,[0-9]{3})*(.[0-9]{1,2})?$ 
22    8.1到3个数字,后面跟着任意个 逗号+3个数字,逗号成为可选,而不是必须：^([0-9]+|[0-9]{1,3}(,[0-9]{3})*)(.[0-9]{1,2})?$ 
23    备注：这就是最终结果了,别忘了"+"可以用"*"替代如果你觉得空字符串也可以接受的话(奇怪,为什么?)最后,别忘了在用函数时去掉去掉那个反斜杠,一般的错误都在这里
24 xml文件：^([a-zA-Z]+-?)+[a-zA-Z0-9]+\\.[x|X][m|M][l|L]$
25 中文字符的正则表达式：[\u4e00-\u9fa5]
26 双字节字符：[^\x00-\xff]    (包括汉字在内，可以用来计算字符串的长度(一个双字节字符长度计2，ASCII字符计1))27 空白行的正则表达式：\n\s*\r    (可以用来删除空白行)
28 HTML标记的正则表达式：<(\S*?)[^>]*>.*?</\1>|<.*? />    (网上流传的版本太糟糕，上面这个也仅仅能部分，对于复杂的嵌套标记依旧无能为力)29 首尾空白字符的正则表达式：^\s*|\s*$或(^\s*)|(\s*$)    (可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式)
30 腾讯QQ号：[1-9][0-9]{4,}    (腾讯QQ号从10000开始)
31 中国邮政编码：[1-9]\d{5}(?!\d)    (中国邮政编码为6位数字)
32 IP地址：\d+\.\d+\.\d+\.\d+    (提取IP地址时有用)
33 IP地址：((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))    
```



正则匹配后一个是数字，并且倒数第二个位置是汉字，移除匹配到的随后的数字。

```
function clearLastNumber(value){
    let chinaStr = value.substring(value.length - 2, value.length - 1)
    let regExp = new RegExp('[\u4E00-\u9FA5]');
    if (regExp.test(chinaStr)) {
        return value.replace(/\d(?=\d?$)/g, "")
    }else{
        return value
    }
}


console.log(clearLastNumber('EMI体育1'));
console.log(clearLastNumber('化学B'));
console.log(clearLastNumber('体育2（3-4）'));
console.log(clearLastNumber('EMI体育3-4'));
console.log(clearLastNumber('EMI体育'));

/**
 * 
 *  EMI体育
    化学B
    体育2（3-4）
    EMI体育3-4
    EMI体育
 * 
 */

```



```
function clearLastNumber(value){
    //白名单
    let white = {
        '3D打印（校队）':'3D打印（校队）',
        '棒球（周末集训班）':'棒球（周末集训班）',
        '武术（长拳）':'武术（长拳）',
        '综合实践（思维课）3-4':'综合实践（思维课）',
        '3D打印与设计':'3D打印与设计',
        'U10男子足球':'U10男子足球',
        '乒乓球（校队）':'乒乓球（校队）',
        '武术队（校队）':'武术队（校队）',
        '活力啦啦操（校队）':'活力啦啦操（校队）',
        '篮球队（校队）':'篮球队（校队）',
        '美术创作工作室（校队）':'美术创作工作室（校队）',
        '美术畅想（校队）':'美术畅想（校队）',
        '英语加油站G3':'英语加油站G3',
        '语文加油站G3':'语文加油站G3',
        '语文训练营G3':'语文训练营G3',
        '轮滑冰球（校队）':'轮滑冰球（校队）',
    }

    if(white[value]){
        return white[value]
    }else{
        return value.replace(/[0-9_\s\+\-\(\)\（\）\&\/]/g, "")
    }

    // return value.replace(/[0-9_\s\+\-\(\)\（\）\&\/]/g, "").replace(/校队/g, "").replace(/周末集训班/g, "").replace(/长拳/g, "").replace(/思维课/g, "")

    // let chinaStr = value.substring(value.length - 2, value.length - 1)
    // let regExp = new RegExp('[\u4E00-\u9FA5]');
    // if (regExp.test(chinaStr)) {
    //     return value.replace(/\d(?=\d?$)/g, "")
    // }else{
    //     return value
    // }
}


console.log(clearLastNumber('EMI体育1'));
console.log(clearLastNumber('化学B'));
console.log(clearLastNumber('体育2（3-4）'));
console.log(clearLastNumber('体育2(3-4)'));
console.log(clearLastNumber('EMI体育3-4'));
console.log(clearLastNumber('EMI体育-6'));
console.log(clearLastNumber('EMI体育-6&'));
console.log(clearLastNumber('棒球（周末集训班）'));
console.log(clearLastNumber('EMI体育  '));
console.log(clearLastNumber('3D打印（校队）'));
console.log(clearLastNumber('少儿篮球（1&2）'));
console.log(clearLastNumber('棒球（周末集训班）'));
console.log(clearLastNumber('武术（长拳）'));
console.log(clearLastNumber('综合实践（思维课）3-4'));


console.log(clearLastNumber('3D打印（校队）'));
console.log(clearLastNumber('棒球（周末集训班）'));
console.log(clearLastNumber('武术（长拳）'));
console.log(clearLastNumber('综合实践（思维课）3-4'));
console.log(clearLastNumber('3D打印与设计'));
console.log(clearLastNumber('U10男子足球'));
console.log(clearLastNumber('乒乓球（校队）'));
console.log(clearLastNumber('武术队（校队）'));
console.log(clearLastNumber('活力啦啦操（校队）'));
console.log(clearLastNumber('篮球队（校队）'));
console.log(clearLastNumber('美术创作工作室（校队）'));
console.log(clearLastNumber('美术畅想（校队）'));
console.log(clearLastNumber('英语加油站G3'));
console.log(clearLastNumber('语文加油站G3'));
console.log(clearLastNumber('语文训练营G3'));
console.log(clearLastNumber('轮滑冰球（校队）'));
```





# 性能优化篇

## CDN

cdn 内容分发网络

使用cdn把一些静态的资源放到cdn上，一些第三方的包也可以，这样就可以减少服务器的负荷。

https://blog.csdn.net/qq_38143787/article/details/109197628



## 延迟加载

延迟加载有助于进一步缩短前端加载时间，使用延迟加载，首先确保主要的内容先加载，如页面框架、文本内容、首屏内容等。在实际应用中可以对 JavaScript 进行延迟加载，HTML 中可以有两个相关属性 `async` 和 `defer` ， 这个两个属性使得 `script` 都不会阻塞DOM 的渲染。：

- `defer` 属性规定是否对脚本执行进行延迟，直到页面加载为止。
- `async`：会使得 `script` 脚本异步的加载并在允许的情况下执行，并且不会按着 `script` 在页面中的顺序来执行，而是谁先加载完谁执行。

```xml
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-157283337-2"></script>
<script defer src="https://www.googletagmanager.com/gtag/js?id=UA-157283337-2"></script>
```



## 预加载

预加载就是通过设置相应的资源属性告诉浏览器是否需要预取，包括 CSS文件、JavaScript文件、DNS接下，主要是在HTML页面的 `<head></head>` 间加入 `<meta />`：

```ini
<link rel="dns-prefetch" href="//www.devpoint.cn" />
<link rel="preload" as="style" href="/devpoint/public/css/site.css"/>
<link as="script"  rel="preload" href="/devpoint/public/scripts/site_min.js" />
```



## 缓存

为静态资源增加缓存是常见的处理方式，通常在项目开发中建议采用动静分离，即所谓的静态资源与应用本身分离，方便对静态资源进行优化，增加缓存或者增加CDN都可以方便的实现。下面是以Nginx的配置为例，为静态资源增加缓存：

```
 location ~* \.(gif|webp|txt|jpg|jpeg|png|swf|flv|ico|mp4|js|css|eot|ttf|woff|woff2|svg|bmp|doc|zip|docx|rar)$ {
    proxy_cache cache_one;
    proxy_cache_valid 200 302 24h;
    proxy_cache_valid 301 30d;
    proxy_cache_valid any 5m;
    expires 90d;
}
```



## 图片懒加载 、 路由懒加载

https://segmentfault.com/a/1190000038413073

```
<div class="container">
<img src="loading.gif" data-src="pic.png">
<img src="loading.gif" data-src="pic.png">
<img src="loading.gif" data-src="pic.png">
<img src="loading.gif" data-src="pic.png">
<img src="loading.gif" data-src="pic.png">
<img src="loading.gif" data-src="pic.png">
</div>
<script>
var imgs = document.querySelectorAll('img');
function lozyLoad(){
var scrollTop = document.body.scrollTop ||
document.documentElement.scrollTop;
var winHeight= window.innerHeight;
for(var i=0;i < imgs.length;i++){
if(imgs[i].offsetTop < scrollTop + winHeight ){
imgs[i].src = imgs[i].getAttribute('data-src');
}
}
}
window.onscroll = lozyLoad();
</script>
```

**IntersectionObserver **新方法可以不用监听器做懒加载，缺点：兼容不好

```
const imgList = [...document.querySelectorAll('img')]

var io = new IntersectionObserver((entries) =>{
  entries.forEach(item => {
    // isIntersecting是一个Boolean值，判断目标元素当前是否可见
    if (item.isIntersecting) {
      item.target.src = item.target.dataset.src
      // 图片加载后即停止监听该元素
      io.unobserve(item.target)
    }
  })
}, {
  root: document.querySelector('.root')
})

// observe遍历监听所有img节点
imgList.forEach(img => io.observe(img))
```

vue**中可以使用路由懒加载**

```
component: () =>
      import(
        /* webpackChunkName: "findmusci" */ "../views/find-music/find-music.vue"
      ),
```



## 回流与重绘

https://juejin.cn/post/6844903734951018504#heading-15

**回流**：当**渲染树中**部分或者全部元素的尺⼨、结构或者属性发⽣变化时，浏览器会重新渲染部分或者全部⽂档 的过程就称为回流。

触发回流的操作：

- 页⾯的⾸次渲染 
- 浏览器的窗⼝⼤⼩发⽣变化 
- 元素的内容发⽣变化 
- 元素的尺⼨或者位置发⽣变化 
- 元素的字体⼤⼩发⽣变化 
- 激活CSS伪类 
- 查询某些属性或者调⽤某些⽅法
- 添加或者删除可⻅的DOM元素



**重绘**：当⻚⾯中某些元素的样式发⽣变化，但是不会影响其在⽂档流中的位置时，浏览器就会对元素进⾏重新 绘制，这个过程就是重绘。

color、background 相关属性：background-color、background-image 等

outline 相关属性：outline-color、outline-width 、text-decoration

border-radius、visibility、box-shadow



**减少回流与重绘的措施：**

操作DOM时，尽量在低层级的DOM节点进⾏操作 

不要使⽤ table 布局， ⼀个⼩的改动可能会使整个 table 进⾏重新布局 

使⽤CSS的表达式 不要频繁操作元素的样式，对于静态⻚⾯，可以修改类名，⽽不是样式。 

使⽤absolute或者fixed，使元素脱离⽂档流，这样他们发⽣变化就不会影响其他元素 

 将元素先设置 display: none ，操作结束后再把它显示出来。因为在display属性为none的元素 上进⾏的DOM操作不会引发回流和重绘。

 将DOM的多个读操作（或者写操作）放在⼀起，⽽不是读写操作穿插着写。这得益于浏览器的渲染 队列机制。



防抖和节流都是为了解决短时间内大量触发某函数而导致的性能问题，比如触发频率过高导致的响应速度跟不上触发频率，出现延迟，假死或卡顿的现象

## 节流 

```
function _throttle(fn, delay = 300) {
    var prev = Date.now();
    return (...args) => {
        var now = Date.now();
        if (now - prev > delay) {
            prev = Date.now();
            fn.apply(this, args);
        }
    };
}


function _throttle(fn, delay = 300) {
    let timer = null
    return (...args) => {
        if (!timer) {
            timer = setTimeout(() => {
                timer = null
                fn.apply(this, args)
            }, delay)
        }
    }
}
```

## 防抖

```
export function _debounce(fn, delay) {
    let timer = null;
    return (...args) => {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            fn.apply(this, args);
        }, delay);
    };
}


 	   lllllllllllllllllllllll   可能这个时间段内你一直触发某个函数
       |                     |
       |（它到这里就触发了一个settimeout）到时候下一个函数执行的时候timer就有值了，return里面就清楚上一个，重新计时了


使用vue指令实现防抖效果 
这个指令是需要使用在button标签下的，因为只有button才有disabled属性
import Vue from 'vue'
// 防重复点击(指令实现)
const preventReClick = Vue.directive('preventReClick', {
    inserted(el, binding) {
        el.addEventListener('click', () => {
            if (!el.disabled) {
                el.disabled = true
                setTimeout(() => {
                    el.disabled = false
                }, binding.value || 3000)
            }
        })
    }
})
export default {
    preventReClick
}
```

**防抖:触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间。**
**节流:高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率。**

```
<!DOCTYPE html>

<body>
    <style>
        div {
            color: aqua;
            height: 100000px;
        }
    </style>
    <div class="wrap">
        21312312312
    </div>
    <script>
        let div = document.getElementsByClassName('wrap')[0]
        div.style.color = 'red'
        div.style.color = 'black'
        function a() {
            console.log(1);
        }
        function _throttle(fn, delay = 300) {
            var prev = Date.now();
            console.log(prev);
            return (...args) => {
                var now = Date.now();
                if (now - prev > delay) {
                    prev = Date.now();
                    fn.apply(this, args);
                }
            };
        }
        addEventListener('scroll', _throttle(a, 1000))
    </script>
</body>


                     
       lllllllllllllllllllllll   可能这个时间段内你一直触发某个函数（这个时间段就是delay）
       |                     |
                             |（它到这里才会触发一次）
```

```
就是说每一次addEventListener调用的_throttle函数是同一个函数，不是新的函数。所以prev的值是不会变的，如果不重新赋值。
所以才有 prev = Date.now();这个说法

```



## 图片优化

1.不⽤图⽚。很多时候会使⽤到很多修饰类图⽚，其实这类修饰图⽚完全可以⽤ CSS 去代替。 

2.对于移动端来说，屏幕宽度就那么点，完全没有必要去加载原图浪费带宽。⼀般图⽚都⽤ CDN 加 载，可以计算出适配屏幕的宽度，然后去请求相应裁剪好的图⽚。

3.⼩图使⽤ base64 格式 

4.将多个图标⽂件整合到⼀张图⽚中（雪碧图） 

5.选择正确的图⽚格式： 对于能够显示 WebP 格式的浏览器尽量使⽤ WebP 格式。因为 WebP 格式具有更好的图像数 据压缩算法，能带来更⼩的图⽚体积，⽽且拥有⾁眼识别⽆差异的图像质量，缺点就是兼容性并不好 ⼩图使⽤ PNG，其实对于⼤部分图标这类图⽚，完全可以使⽤ SVG 代替 照⽚使⽤ JPEG。



## Webpack优化⭐

### 如何⽤webpack来优化前端性能？

 **压缩代码**：删除多余的代码、注释、简化代码的写法等等⽅式。可以利⽤webpack的 UglifyJsPlugin 和 ParallelUglifyPlugin 来压缩JS⽂件， 利⽤ cssnano （css-loader?minimize） 来压缩css 

**利⽤CDN加速**: 在构建过程中，将引⽤的静态资源路径修改为CDN上对应的路径。可以利⽤ webpack对于 output 参数和各loader的 publicPath 参数来修改资源路径 

**Tree Shaking**: 将代码中永远不会⾛到的⽚段删除掉。可以通过在启动webpack时追加参数 -- optimize-minimize 来实现 

**Code Splitting**: 将代码按路由维度或者组件分块(chunk),这样做到按需加载,同时可以充分利⽤浏览 器缓存 

**提取公共第三⽅库**: SplitChunksPlugin插件来进⾏公共模块抽取,利⽤浏览器缓存可以⻓期缓存这些 ⽆需频繁变动的公共代码 

### 如何提⾼webpack的构建速度？

1. 多⼊⼝情况下，使⽤ CommonsChunkPlugin 来提取公共代码 
2. 通过 externals 配置来提取常⽤库 
3. 利⽤ DllPlugin 和 DllReferencePlugin 预编译资源模块 通过 DllPlugin 来对那些我们引⽤但是绝对 不会修改的npm包来进⾏预编译，再通过 DllReferencePlugin 将预编译的模块加载进来。 
4. 使⽤ Happypack 实现多线程加速编译 
5. 使⽤ webpack-uglify-parallel 来提升 uglifyPlugin 的压缩速度。 原理上 webpack-uglifyparallel 采⽤了多核并⾏压缩来提升压缩速度 
6. 使⽤ Tree-shaking 和 Scope Hoisting 来剔除多余代码







# File、Blob、FileReader、ArrayBuffer、base64之间的关系

JavaScript 提供了一些 API 来处理文件或原始文件数据，例如：File、Blob、FileReader、ArrayBuffer、base64 

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBcQECI5aicL7byicfumiaGXWLSFCfHPkalFiccSHWWKOOSFHPpaJ88ic4K2w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## 1. Blob

Blob 全称为 binary large object ，即二进制大对象，它是 JavaScript 中的一个对象，表示原始的类似文件的数据。下面是 MDN 中对 Blob 的解释：

> Blob 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 ReadableStream 来用于数据操作。

实际上，Blob 对象是包含有只读原始数据的类文件对象。简单来说，Blob 对象就是一个不可修改的二进制文件。

### （1）Blob 创建

可以使用 Blob() 构造函数来创建一个 Blob：

```
new Blob(array, options);
```

其有两个参数：

- `array`：由 `ArrayBuffer`、`ArrayBufferView`、`Blob`、`DOMString` 等对象构成的，将会被放进 `Blob`；

- `options`：可选的 `BlobPropertyBag` 字典，它可能会指定如下两个属性

- - `type`：默认值为 ""，表示将会被放入到 `blob` 中的数组内容的 MIME 类型。
  - `endings`：默认值为"`transparent`"，用于指定包含行结束符`\n`的字符串如何被写入，不常用。

常见的 MIME 类型如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjB9DibsGicIP99Q90ric28KNxfRsG6yV4bzia5Lpiclq7Ja3eriaFrdnwMpZsw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面来看一个简单的例子：

```
const blob = new Blob(["Hello World"], {type: "text/plain"});
```

这里可以成为动态文件创建，其正在创建一个类似文件的对象。这个 blob 对象上有两个属性：

- `size`：Blob对象中所包含数据的大小（字节）；
- `type`：字符串，认为该Blob对象所包含的 MIME 类型。如果类型未知，则为空字符串。

下面来看打印结果：

```
const blob = new Blob(["Hello World"], {type: "text/plain"});

console.log(blob.size); // 11
console.log(blob.type); // "text/plain"
```

注意，字符串"Hello World"是 UTF-8 编码的，因此它的每个字符占用 1 个字节。

到现在，Blob 对象看起来似乎我们还是没有啥用。那该如何使用 Blob 对象呢？可以使用 URL.createObjectURL() 方法将将其转化为一个 URL，并在 Iframe 中加载：

```
<iframe></iframe>

const iframe = document.getElementsByTagName("iframe")[0];

const blob = new Blob(["Hello World"], {type: "text/plain"});

iframe.src = URL.createObjectURL(blob);
```

### （2）Blob 分片

除了使用`Blob()`构造函数来创建blob 对象之外，还可以从 blob 对象中创建blob，也就是将 blob 对象切片。Blob 对象内置了 slice() 方法用来将 blob 对象分片，其语法如下：

```
const blob = instanceOfBlob.slice([start [, end [, contentType]]]};
```

其有三个参数：

- `start`：设置切片的起点，即切片开始位置。默认值为 0，这意味着切片应该从第一个字节开始；
- `end`：设置切片的结束点，会对该位置之前的数据进行切片。默认值为`blob.size`；
- `contentType`：设置新 blob 的 MIME 类型。如果省略 type，则默认为 blob 的原始值。

下面来看例子：

```
const iframe = document.getElementsByTagName("iframe")[0];

const blob = new Blob(["Hello World"], {type: "text/plain"});

const subBlob = blob.slice(0, 5);

iframe.src = URL.createObjectURL(subBlob);
```

此时页面会显示"Hello"。

## 2. File

文件（File）接口提供有关文件的信息，并允许网页中的 JavaScript 访问其内容。实际上，File 对象是特殊类型的 Blob，且可以用在任意的 Blob 类型的 context 中。Blob 的属性和方法都可以用于 File 对象。

> 注意：File 对象中只存在于浏览器环境中，在 Node.js 环境中不存在。

在 JavaScript 中，主要有两种方法来获取 File 对象：

- `<input>` 元素上选择文件后返回的 FileList 对象；
- 文件拖放操作生成的 `DataTransfer` 对象；

### （1）input

首先定义一个输入类型为 file 的 `input` 标签：

```
<input type="file" id="fileInput" multiple="multiple">
```

这里给 `input` 标签添加了三个属性：

- `type="file"`：指定 `input` 的输入类型为文件；
- `id="fileInput"`：指定 `input` 的唯一 id；
- `multiple="multiple"`：指定 `input` 可以同时上传多个文件；

下面来给 `input` 标签添加 `onchange` 事件，当选择文件并上传之后触发：

```
const fileInput = document.getElementById("fileInput");

fileInput.onchange = (e) => {
    console.log(e.target.files);
}
```

当点击上传文件时，控制台就会输出一个 FileList 数组，这个数组的每个元素都是一个 File 对象，一个上传的文件就对应一个 File 对象：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBx2B7dlBnCJm4hLulndbtFJV7ksEmxZ8LqgoPoYnTkVzSVbiaHicLXksQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

每个 `File` 对象都包含文件的一些属性，这些属性都继承自 Blob 对象：

- `lastModified`：引用文件最后修改日期，为自1970年1月1日0:00以来的毫秒数；
- `lastModifiedDate`：引用文件的最后修改日期；
- `name`：引用文件的文件名；
- `size`：引用文件的文件大小；
- `type`：文件的媒体类型（MIME）；
- `webkitRelativePath`：文件的路径或 URL。

通常，我们在上传文件时，可以通过对比 size 属性来限制文件大小，通过对比 type 来限制上传文件的格式等。

### （2）文件拖放

另一种获取 File 对象的方式就是拖放 API，这个 API 很简单，就是将浏览器之外的文件拖到浏览器窗口中，并将它放在一个成为拖放区域的特殊区域中。拖放区域用于响应放置操作并从放置的项目中提取信息。这些是通过 `ondrop` 和 `ondragover` 两个 API 实现的。

下面来看一个简单的例子，首先定义一个拖放区域：

```
<div id="drop-zone"></div>
```

然后给这个元素添加 `ondragover` 和 `ondrop` 事件处理程序：

```
const dropZone = document.getElementById("drop-zone");

dropZone.ondragover = (e) => {
    e.preventDefault();
}

dropZone.ondrop = (e) => {
    e.preventDefault();
    const files = e.dataTransfer.files;
    console.log(files)
}
```

**注意**：这里给两个 API 都添加了 `e.preventDefault()`，用来阻止默认事件。它是非常重要的，可以用来阻止浏览器的一些默认行为，比如放置文件将显示在浏览器窗口中。

当拖放文件到拖放区域时，控制台就会输出一个  FileList 数组，该数组的每一个元素都是一个 `File` 对象。这个 FileList 数组是从事件参数的 `dataTransfer` 属性的 `files` 获取的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBMZVVUgo40UaMyPKFZOrrHLqgTSMHYFssib4YFLFXzIia8ztEofPetcdg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到，这里得到的 `File` 对象和通过 `input` 标签获得的 `File` 对象是完全一样的。

## 3. FileReader

FileReader 是一个异步 API，用于读取文件并提取其内容以供进一步使用。FileReader 可以将 Blob 读取为不同的格式。

> 注意：FileReader 仅用于以安全的方式从用户（远程）系统读取文件内容，不能用于从文件系统中按路径名简单地读取文件。

### （1）基本使用

可以使用 FileReader 构造函数来创建一个 FileReader 对象：

```
const reader = new FileReader();
```

这个对象常用属性如下：

- `error`：表示在读取文件时发生的错误；
- `result`：文件内容。该属性仅在读取操作完成后才有效，数据的格式取决于使用哪个方法来启动读取操作。
- `readyState`：表示`FileReader`状态的数字。取值如下：



FileReader 对象提供了以下方法来加载文件：

- `readAsArrayBuffer()`：读取指定 Blob 中的内容，完成之后，`result` 属性中保存的将是被读取文件的 `ArrayBuffer` 数据对象；
- `FileReader.readAsBinaryString()`：读取指定 Blob 中的内容，完成之后，`result` 属性中将包含所读取文件的原始二进制数据；
- `FileReader.readAsDataURL()`：读取指定 Blob 中的内容，完成之后，`result` 属性中将包含一个`data: URL` 格式的 Base64 字符串以表示所读取文件的内容。
- `FileReader.readAsText()`：读取指定 Blob 中的内容，完成之后，`result` 属性中将包含一个字符串以表示所读取的文件内容。

可以看到，上面这些方法都接受一个要读取的 blob 对象作为参数，读取完之后会将读取的结果放入对象的 `result` 属性中。

### （2）事件处理

FileReader 对象常用的事件如下：

- `abort`：该事件在读取操作被中断时触发；
- `error`：该事件在读取操作发生错误时触发；
- `load`：该事件在读取操作完成时触发；
- `progress`：该事件在读取 Blob 时触发。

当然，这些方法可以加上前置 on 后在HTML元素上使用，比如`onload`、`onerror`、`onabort`、`onprogress`。除此之外，由于`FileReader`对象继承自`EventTarget`，因此还可以使用 `addEventListener()` 监听上述事件。

下面来看一个简单的例子，首先定义一个 `input` 输入框用于上传文件：

```
<input type="file" id="fileInput">
```

接下来定义 `input` 标签的 `onchange` 事件处理函数和`FileReader`对象的`onload`事件处理函数：

```
const fileInput = document.getElementById("fileInput");

const reader = new FileReader();

fileInput.onchange = (e) => {
    reader.readAsText(e.target.files[0]);
}

reader.onload = (e) => {
    console.log(e.target.result);
}
```

这里，首先创建了一个 `FileReader` 对象，当文件上传成功时，使用 `readAsText()` 方法读取 `File` 对象，当读取操作完成时打印读取结果。

使用上述例子读取文本文件时，就是比较正常的。如果读取二进制文件，比如png格式的图片，往往会产生乱码，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBiaE2mnicVvcib36FADoJuqbgE4rWd7KNnAkU90XIKKyw1kyxVw6VibojHQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

那该如何处理这种二进制数据呢？`readAsDataURL()` 是一个不错的选择，它可以将读取的文件的内容转换为 base64 数据的 URL 表示。这样，就可以直接将 URL 用在需要源链接的地方，比如 img 标签的 src 属性。

对于上述例子，将 readAsText 方法改为 `readAsDataURL()`：

```
const fileInput = document.getElementById("fileInput");

const reader = new FileReader();

fileInput.onchange = (e) => {
    reader.readAsDataURL(e.target.files[0]);
}

reader.onload = (e) => {
    console.log(e.target.result);
}
```

这时，再次上传二进制图片时，就会在控制台打印一个 base64 编码的 URL，如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBwp075sRvcDbUEBARehWia3XFevFtrrYyMI7NrVVag4cCv9Ex7B0ibVZg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面来修改一下这个例子，将上传的图片通过以上方式显示在页面上：

```
<input type="file" id="fileInput" />

<img id="preview" />
const fileInput = document.getElementById("fileInput");
const preview = document.getElementById("preview");
const reader = new FileReader();

fileInput.onchange = (e) => {
  reader.readAsDataURL(e.target.files[0]);
};

reader.onload = (e) => {
  preview.src = e.target.result;
  console.log(e.target.result);
};
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBpibytqWcTodGCHmMamLOiaoZFwibZEfUPp2XGVbS2WRC8aOgP7bqK7aAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

当上传大文件时，可以通过 `progress` 事件来监控文件的读取进度：

```
const reader = new FileReader();

reader.onprogress = (e) => {
  if (e.loaded && e.total) {
    const percent = (event.loaded / event.total) * 100;
    console.log(`上传进度: ${Math.round(percent)} %`);
  }
});
```

`progress` 事件提供了两个属性：`loaded`（已读取量）和`total`（需读取总量）。

## 4. ArrayBuffer

### （1）ArrayBuffer

ArrayBuffer 对象用来表示通用的、固定长度的**原始二进制数据缓冲区**。ArrayBuffer 的内容不能直接操作，只能通过 DataView 对象或 TypedArrray 对象来访问。这些对象用于读取和写入缓冲区内容。

ArrayBuffer 本身就是一个黑盒，不能直接读写所存储的数据，需要借助以下视图对象来读写：

- **TypedArray**：用来生成内存的视图，通过9个构造函数，可以生成9种数据格式的视图。
- **DataViews**：用来生成内存的视图，可以自定义格式和字节序。

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBBT93872GFpMId0yL0wkERib8DQJjZICRn3ljrib8ObXao2ibOVPVtTxibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

TypedArray视图和 DataView视图的区别主要是**字节序**，前者的数组成员都是同一个数据类型，后者的数组成员可以是不同的数据类型。

那 ArrayBuffer 与 Blob 有啥区别呢？根据 ArrayBuffer 和 Blob 的特性，Blob 作为一个整体文件，适合用于传输；当需要对二进制数据进行操作时（比如要修改某一段数据时），就可以使用 ArrayBuffer。

下面来看看 ArrayBuffer 有哪些常用的方法和属性。

#### ① new ArrayBuffer()

ArrayBuffer 可以通过以下方式生成：

```
new ArrayBuffer(bytelength)
```

`ArrayBuffer()`构造函数可以分配指定字节数量的缓冲区，其参数和返回值如下：

- **参数**：它接受一个参数，即 bytelength，表示要创建数组缓冲区的大小（以字节为单位。）；
- **返回值**：返回一个新的指定大小的ArrayBuffer对象，内容初始化为0。

#### ② ArrayBuffer.prototype.byteLength

ArrayBuffer 实例上有一个 byteLength 属性，它是一个只读属性，表示 ArrayBuffer 的 byte 的大小，在 ArrayBuffer 构造完成时生成，不可改变。来看例子：

```
const buffer = new ArrayBuffer(16); 
console.log(buffer.byteLength);  // 16
```

#### ③ ArrayBuffer.prototype.slice()

ArrayBuffer 实例上还有一个 slice 方法，该方法可以用来截取 ArrayBuffer 实例，它返回一个新的 ArrayBuffer ，它的内容是这个 ArrayBuffer 的字节副本，从 begin（包括），到 end（不包括）。来看例子：

```
const buffer = new ArrayBuffer(16); 
console.log(buffer.slice(0, 8));  // 16
```

这里会从 buffer 对象上将前8个字节生成一个新的ArrayBuffer对象。这个方法实际上有两步操作，首先会分配一段指定长度的内存，然后拷贝原来ArrayBuffer对象的置顶部分。

#### ④ ArrayBuffer.isView()

ArrayBuffer 上有一个 isView()方法，它的返回值是一个布尔值，如果参数是 ArrayBuffer 的视图实例则返回 true，例如类型数组对象或 DataView 对象；否则返回 false。简单来说，这个方法就是用来判断参数是否是 TypedArray 实例或者 DataView 实例：

```
const buffer = new ArrayBuffer(16);
ArrayBuffer.isView(buffer)   // false

const view = new Uint32Array(buffer);
ArrayBuffer.isView(view)     // true
```

### （2）TypedArray

TypedArray 对象一共提供 9 种类型的视图，每一种视图都是一种构造函数。如下：

| **元素** | **类型化数组**    | **字节** | **描述**        |
| :------- | :---------------- | :------- | :-------------- |
| Int8     | Int8Array         | 1        | 8 位有符号整数  |
| Uint8    | Uint8Array        | 1        | 8 位无符号整数  |
| Uint8C   | Uint8ClampedArray | 1        | 8 位无符号整数  |
| Int16    | Int16Array        | 2        | 16 位有符号整数 |
| Uint16   | Uint16Array       | 2        | 16 位无符号整数 |
| Int32    | Int32Array        | 4        | 32 位有符号整数 |
| Uint32   | Uint32Array       | 4        | 32 位无符号整数 |
| Float32  | Float32Array      | 4        | 32 位浮点       |
| Float64  | Float64Array      | 8        | 64 位浮点       |

来看看这些都是什么意思：

- **Uint8Array：** 将 ArrayBuffer 中的每个字节视为一个整数，可能的值从 0 到 255 （一个字节等于 8 位）。 这样的值称为“8 位无符号整数”。
- **Uint16Array**：将 ArrayBuffer 中任意两个字节视为一个整数，可能的值从 0 到 65535。 这样的值称为“16 位无符号整数”。
- **Uint32Array：**将 ArrayBuffer 中任何四个字节视为一个整数，可能值从 0 到 4294967295，这样的值称为“32 位无符号整数”。

这些构造函数生成的对象统称为 TypedArray 对象。它们和正常的数组很类似，都有`length` 属性，都能用索引获取数组元素，所有数组的方法都可以在类型化数组上面使用。

**那类型化数组和数组有什么区别呢？**

- 类型化数组的元素都是连续的，不会为空；
- 类型化数组的所有成员的类型和格式相同；
- 类型化数组元素默认值为 0；
- 类型化数组本质上只是一个视图层，不会存储数据，数据都存储在更底层的 ArrayBuffer 对象中。

下面来看看 TypedArray 都有哪些常用的方法和属性。

#### ① new TypedArray()

TypedArray 的语法如下（TypedArray只是一个概念，实际使用的是那9个对象）：

```
new Int8Array(length);
new Int8Array(typedArray);
new Int8Array(object);
new Int8Array(buffer [, byteOffset [, length]]);
```

可以看到，TypedArray 有多种用法，下面来分别看一下。

- **TypedArray(length)** ：通过分配指定长度内容进行分配

```
let view = new Int8Array(16);
view[0] = 10;
view[10] = 6;
console.log(view);
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBjvbhdePuMQRqP6Utd6LC6GvSotoErhqtkjYKkzdkP5DXafIHqUkc1w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里就生成了一个 16个元素的 Int8Array 数组，除了手动赋值的元素，其他元素的初始值都是 0。

- **TypedArray(typeArray)** ：接收一个视图实例作为参数

```
const view = new Int8Array(new Uint8Array(6));
view[0] = 10;
view[3] = 6;
console.log(view);
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBTJ7n72mWia8vicclFrDlG5h1iccSNElNkRfdiaJvaic5FrVA1dMpZhJk4Sg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- **TypedArray(object)** ：参数可以是一个普通数组

```
const view = new Int8Array([1, 2, 3, 4, 5]);
view[0] = 10;
view[3] = 6;
console.log(view);
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBQvwyeSQtbOzJpoQr7wr1buJUflke5QOMV87mVgEmtLGhS6KY6g4XEA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

需要注意，TypedArray视图会开辟一段新的内存，不会在原数组上建立内存。当然，这里创建的类型化数组也能转换回普通数组：

```
Array.prototype.slice.call(view); // [10, 2, 3, 6, 5]
```

- **TypeArray(buffer [, byteOffset [, length]])** ：

这种方式有三个参数，其中第一个参数是一个ArrayBuffer对象；第二个参数是视图开始的字节序号，默认从0开始，可选；第三个参数是视图包含的数据个数，默认直到本段内存区域结束。

```
const buffer = new ArrayBuffer(8);
const view1 = new Int32Array(buffer); 
const view2 = new Int32Array(buffer, 4); 
console.log(view1, view2);
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjB7p2VKK6MNIZAsDsRtFX8v4VdWydZic3locxHRzohOhqbgzMZm36p8sQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### ② BYTES_PER_ELEMENT

每种视图的构造函数都有一个 `BYTES_PER_ELEMENT` 属性，表示这种数据类型占据的字节数：

```
Int8Array.BYTES_PER_ELEMENT // 1
Uint8Array.BYTES_PER_ELEMENT // 1
Int16Array.BYTES_PER_ELEMENT // 2
Uint16Array.BYTES_PER_ELEMENT // 2
Int32Array.BYTES_PER_ELEMENT // 4
Uint32Array.BYTES_PER_ELEMENT // 4
Float32Array.BYTES_PER_ELEMENT // 4
Float64Array.BYTES_PER_ELEMENT // 8
```

`BYTES_PER_ELEMENT` 属性也可以在类型化数组的实例上获取：

```
const buffer = new ArrayBuffer(16); 
const view = new Uint32Array(buffer); 
console.log(Uint32Array.BYTES_PER_ELEMENT); // 4
```

#### ③ **TypedArray.prototype.buffer**

TypedArray 实例的 buffer 属性会返回内存中对应的 ArrayBuffer对象，只读属性。

```
const a = new Uint32Array(8);
const b = new Int32Array(a.buffer); 
console.log(a, b);
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBcQ1icc88FA4sRfCAeKjP6khiaqWufjw3mRFTibKcpb6fyOwZsGyhaA7lw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### ④ TypedArray.prototype.slice()

TypeArray 实例的 slice方法可以返回一个指定位置的新的 TypedArray实例。

```
const view = new Int16Array(8);
console.log(view.slice(0 ,5));
```

输出结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBl8pv1icHKibWF5hxKS6a9RecUo0qqu2BuF8n3juwtWRTk3TPKKdjQic0g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### ⑤ byteLength 和 length

- `byteLength`：返回 TypedArray 占据的内存长度，单位为字节；
- `length`：返回 TypedArray 元素个数；

```
const view = new Int16Array(8);
view.length;      // 8
view.byteLength;  // 16
```

### （3）DataView

说完 ArrayBuffer，下面来看看另一种操作 ArrayBuffer 的方式：DataView。**DataView** 视图是一个可以从 二进制 ArrayBuffer 对象中读写多种数值类型的底层接口，使用它时，不用考虑不同平台的字节序问题。

DataView视图提供更多操作选项，而且支持设定字节序。本来，在设计目的上，ArrayBuffer对象的各种TypedArray视图，是用来向网卡、声卡之类的本机设备传送数据，所以使用本机的字节序就可以了；而DataView视图的设计目的，是用来处理网络设备传来的数据，所以大端字节序或小端字节序是可以自行设定的。

#### ① new DataView()

DataView视图可以通过构造函数来创建，它的参数是一个ArrayBuffer对象，生成视图。其语法如下：

```
new DataView(buffer [, byteOffset [, byteLength]])
```

其有三个参数：

- `buffer`：一个已经存在的 ArrayBuffer 对象，DataView 对象的数据源。
- `byteOffset`：可选，此 DataView 对象的第一个字节在 buffer 中的字节偏移。如果未指定，则默认从第一个字节开始。
- `byteLength`：可选，此 DataView 对象的字节长度。如果未指定，这个视图的长度将匹配 buffer 的长度。

来看一个例子：

```
const buffer = new ArrayBuffer(16);
const view = new DataView(buffer);
console.log(view);
```

打印结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBAQgcSIZ83CaQVmnFC5jj8P2B9bLdnsvd9ic24IAecF01pD8UdnLqHGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### ② buffer、byteLength、byteOffset

DataView 实例有以下常用属性：

- `buffer`：返回对应的ArrayBuffer对象；
- `byteLength`：返回占据的内存字节长度；
- `byteOffset`：返回当前视图从对应的ArrayBuffer对象的哪个字节开始。

```
const buffer = new ArrayBuffer(16);
const view = new DataView(buffer);
view.buffer;
view.byteLength;
view.byteOffset;
```

打印结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBmbzW69HZianteiafuia1gEico1cNj5Iib4TTwF8dJswWW7k5EIF23VcxldQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### ③ 读取内存

DataView 实例提供了以下方法来读取内存，它们的参数都是一个字节序号，表示开始读取的字节位置：

- getInt8：读取1个字节，返回一个8位整数。
- getUint8：读取1个字节，返回一个无符号的8位整数。
- getInt16：读取2个字节，返回一个16位整数。
- getUint16：读取2个字节，返回一个无符号的16位整数。
- getInt32：读取4个字节，返回一个32位整数。
- getUint32：读取4个字节，返回一个无符号的32位整数。
- getFloat32：读取4个字节，返回一个32位浮点数。
- getFloat64：读取8个字节，返回一个64位浮点数。

下面来看一个例子：

```
const buffer = new ArrayBuffer(24);
const view = new DataView(buffer);

// 从第1个字节读取一个8位无符号整数
const view1 = view.getUint8(0);

// 从第2个字节读取一个16位无符号整数
const view2 = view.getUint16(1);

// 从第4个字节读取一个16位无符号整数
const view3 = view.getUint16(3);
```

#### ④ 写入内存

DataView 实例提供了以下方法来写入内存，它们都接受两个参数，第一个参数表示开始写入数据的字节序号，第二个参数为写入的数据：

- setInt8：写入1个字节的8位整数。
- setUint8：写入1个字节的8位无符号整数。
- setInt16：写入2个字节的16位整数。
- setUint16：写入2个字节的16位无符号整数。
- setInt32：写入4个字节的32位整数。
- setUint32：写入4个字节的32位无符号整数。
- setFloat32：写入4个字节的32位浮点数。
- setFloat64：写入8个字节的64位浮点数。

## 5. Object URL

Object URL（MDN定义名称）又称Blob URL（W3C定义名称），是HTML5中的新标准。它是一个用来表示File Object 或Blob Object 的URL。在网页中，我们可能会看到过这种形式的 Blob URL：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjBmvaNyW9DO3dpELEDT9LNKJSPvC6OeakDWv6s8fI7Cfhn3QkxX93TKA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

其实 Blob URL/Object URL 是一种伪协议，允许将 Blob 和 File 对象用作图像、二进制数据下载链接等的 URL 源。

对于 Blob/File 对象，可以使用 URL构造函数的 `createObjectURL()` 方法创建将给出的对象的 URL。这个 URL 对象表示指定的 File 对象或 Blob 对象。我们可以在`<img>`、`<script>` 标签中或者 `<a>` 和 `<link>` 标签的 `href` 属性中使用这个 URL。

来看一个简单的例子，首先定义一个文件上传的 input 和一个 图片预览的 img：

```
<input type="file" id="fileInput" />

<img id="preview" />
```

再来使用 `URL.createObjectURL()` 将File 对象转化为一个 URL：

```
const fileInput = document.getElementById("fileInput");
const preview = document.getElementById("preview");

fileInput.onchange = (e) => {
  preview.src = URL.createObjectURL(e.target.files[0]);
  console.log(preview.src);
};
```

可以看到，上传的图片转化成了一个 URL，并显示在了屏幕上：



那这个 API 有什么意义呢？可以将Blob/File对象转化为URL，通过这个URL 就可以实现文件下载或者图片显示等。

当我们使用`createObjectURL()`方法创建一个data URL 时，就需要使用`revokeObjectURL()`方法从内存中清除它来释放内存。虽然浏览器会在文档卸载时自动释放 Data URL，但为了提高性能，我们应该使用`revokeObjectURL()`来手动释放它。`revokeObjectURL()`方法接受一个Data URL 作为其参数，返回`undefined`。下面来看一个例子：

```
const objUrl = URL.createObjectURL(new File([""], "filename"));
console.log(objUrl);
URL.revokeObjectURL(objUrl);
```

## 6. Base64

Base64 是一种基于64个可打印字符来表示二进制数据的表示方法。Base64 编码普遍应用于需要通过被设计为处理文本数据的媒介上储存和传输二进制数据而需要编码该二进制数据的场景。这样是为了保证数据的完整并且不用在传输过程中修改这些数据。

在 JavaScript 中，有两个函数被分别用来处理解码和编码 *base64* 字符串：

- `atob()`：解码，解码一个 Base64 字符串；
- `btoa()`：编码，从一个字符串或者二进制数据编码一个 Base64 字符串。

```
btoa("JavaScript")       // 'SmF2YVNjcmlwdA=='
atob('SmF2YVNjcmlwdA==') // 'JavaScript'
```

那 base64 的实际应用场景有哪些呢？其实多数场景就是基于Data URL的。比如，使用`toDataURL()`方法把 canvas 画布内容生成 base64 编码格式的图片：

```
const canvas = document.getElementById('canvas'); 
const ctx = canvas.getContext("2d");
const dataUrl = canvas.toDataURL();
```

除此之外，还可以使用`readAsDataURL()`方法把上传的文件转为base64格式的data URI，比如上传头像展示或者编辑：

```
<input type="file" id="fileInput" />

<img id="preview" />
const fileInput = document.getElementById("fileInput");
const preview = document.getElementById("preview");
const reader = new FileReader();

fileInput.onchange = (e) => {
  reader.readAsDataURL(e.target.files[0]);
};

reader.onload = (e) => {
  preview.src = e.target.result;
  console.log(e.target.result);
};
```

效果如下，将图片（二进制数据）转化为可打印的字符，也便于数据的传输：

![图片](https://mmbiz.qpic.cn/mmbiz_png/EO58xpw5UMOaLRWgbtfiafOM84RoleicjB79D4jerx6hW3q3EeWR2lC79s1XqRmdfxwutzkv2V3upd9dGRGFtvMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

另外，一些小的图片都可以使用 base64 格式进行展示，`img`标签和`background`的 `url` 属性都支持使用base64 格式的图片，这样做也可以减少 HTTP 请求。

## 7. 格式转化

看完这些基本的概念，下面就来看看常用格式之间是如何转换的。

### （1）ArrayBuffer → blob

```
const blob = new Blob([new Uint8Array(buffer, byteOffset, length)]);
```

### （2）ArrayBuffer → base64

```
const base64 = btoa(String.fromCharCode.apply(null, new Uint8Array(arrayBuffer)));
```

### （3）base64 → blob

```
const base64toBlob = (base64Data, contentType, sliceSize) => {
  const byteCharacters = atob(base64Data);
  const byteArrays = [];

  for (let offset = 0; offset < byteCharacters.length; offset += sliceSize) {
    const slice = byteCharacters.slice(offset, offset + sliceSize);

    const byteNumbers = new Array(slice.length);
    for (let i = 0; i < slice.length; i++) {
      byteNumbers[i] = slice.charCodeAt(i);
    }

    const byteArray = new Uint8Array(byteNumbers);
    byteArrays.push(byteArray);
  }

  const blob = new Blob(byteArrays, {type: contentType});
  return blob;
}
```

### （4）blob → ArrayBuffer

```
function blobToArrayBuffer(blob) { 
  return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result);
      reader.onerror = () => reject;
      reader.readAsArrayBuffer(blob);
  });
}
```

### （5）blob → base64

```
function blobToBase64(blob) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onloadend = () => resolve(reader.result);
    reader.readAsDataURL(blob);
  });
}
```

### （6）blob → Object URL

```
const objectUrl = URL.createObjectURL(blob);
```

















# WEBPACK

什么是webpack？

webpack可以理解为打包工具 代码兼容 代码优化 附属功能。



webpack做了什么？

webpack有什么用？

热更新原理？  https://juejin.cn/post/6844904008432222215#heading-9

loader 、 plugins  、 入口  、 出口

plugins  插件 这就是做一些loader不能做的  

"eslint-plugin-import": "^2.20.2",

  "eslint-plugin-node": "^11.1.0",

  "eslint-plugin-prettier": "^3.1.3",

  "eslint-plugin-promise": "^4.2.1",

  "eslint-plugin-standard": "^4.0.0",

  "eslint-plugin-vue": "^6.2.2",

loader 也分很多loader  sass-loader 、 css-loader 、 



简单来说就是 在webpack.config.js 里面设置配置，例如：入口 可以设置自定义的入口 入口名字  ，也可以多个入口 。



loader 

test 是目标文件  就是对哪一些文件是使用（use）哪一些loader

```js
 rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
```



 plugins   直接new     插件是需要new出实例的  因为他是以发布监听这样的模式  或者说 广播吧  所以需要监听 。

```javascript
 plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
```





# 坑

写移动端就不要使用elementui   会有一些兼容的问题，ios兼容是真的麻烦。selector就是其一