# 1. Vue框架
> 2020-2-11 指令学习笔记
> 2020-2-12 生命周期、组件化开发学习笔记
> 2020-2-13 前端常用的模块化开发总结笔记
## 1.1. 指令

- 本质就是自定义属性
- Vue中指定都是以 v- 开头 

### 1.1.1. v-cloak

- 防止页面加载时出现闪烁问题

  ```html
   <style type="text/css">
    /* 
      1、通过属性选择器 选择到 带有属性 v-cloak的标签  让他隐藏
   */
    [v-cloak]{
      /* 元素隐藏    */
      display: none;
    }
    </style>
  <body>
    <div id="app">
      <!-- 2、 让带有插值 语法的   添加 v-cloak 属性 
           在 数据渲染完场之后，v-cloak 属性会被自动去除，
           v-cloak一旦移除也就是没有这个属性了  属性选择器就选择不到该标签
  		 也就是对应的标签会变为可见
      -->
      <div  v-cloak  >{{msg}}</div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
      var vm = new Vue({
        //  el   指定元素 id 是 app 的元素  
        el: '#app',
        //  data  里面存储的是数据
        data: {
          msg: 'Hello Vue'
        }
      });
  </script>
  </body>
  </html>
  ```
  

### 1.1.2. v-text

- v-text指令用于将数据填充到标签中，作用于插值表达式类似，但是没有闪动问题
- 如果数据中有HTML标签会将html标签一并输出
- 注意：此处为单向绑定，数据对象上的值改变，插值会发生变化；但是当插值发生变化并不会影响数据对象的值

```html
<div id="app">
    <!--  
		注意:在指令中不要写插值语法  直接写对应的变量名称 
        在 v-text 中 赋值的时候不要在写 插值语法
		一般属性中不加 {{}}  直接写 对应 的数据名 
	-->
    <p v-text="msg"></p>
    <p>
        <!-- Vue  中只有在标签的 内容中 才用插值语法 -->
        {{msg}}
    </p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });

</script>
```

### 1.1.3. v-html

- 用法和v-text 相似  但是他可以将HTML片段填充到标签中

- 可能有安全问题, 一般只在可信任内容上使用 `v-html`，**永不**用在用户提交的内容上

- 它与v-text区别在于v-text输出的是纯文本，浏览器不会对其再进行html解析，但v-html会将其当html标签解析后输出。

  ```html
  <div id="app">
  　　<p v-html="html"></p> <!-- 输出：html标签在渲染的时候被解析 -->
      
      <p>{{message}}</p> <!-- 输出：<span>通过双括号绑定</span> -->
      
  　　<p v-text="text"></p> <!-- 输出：<span>html标签在渲染的时候被源码输出</span> -->
  </div>
  <script>
  　　let app = new Vue({
  　　el: "#app",
  　　data: {
  　　　　message: "<span>通过双括号绑定</span>",
  　　　　html: "<span>html标签在渲染的时候被解析</span>",
  　　　　text: "<span>html标签在渲染的时候被源码输出</span>",
  　　}
   });
  </script>
  ```

### 1.1.4. v-pre

- 显示原始信息跳过编译过程
- 跳过这个元素和它的子元素的编译过程。
- **一些静态的内容不需要编译加这个指令可以加快渲染**

```html
    <span v-pre>{{ this will not be compiled }}</span>    
	<!--  显示的是{{ this will not be compiled }}  -->
	<span v-pre>{{msg}}</span>  
     <!--   即使data里面定义了msg这里仍然是显示的{{msg}}  -->
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });

</script>
```

### 1.1.5. v-once

- 执行一次性的插值【当数据改变时，插值处的内容不会继续更新】

```html
  <!-- 即使data里面定义了msg 后期我们修改了 仍然显示的是第一次data里面存储的数据即 Hello Vue.js  -->
     <span v-once>{{ msg}}</span>    
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Vue.js'
        }
    });
</script>
```



### 1.1.6. 双向数据绑定

- 当数据发生变化的时候，视图也就发生变化
- 当视图发生变化的时候，数据也会跟着同步变化

#### 1.1.6.1. v-model

- **v-model**是一个指令，限制在 `<input>、<select>、<textarea>、components`中使用

```html
 <div id="app">
      <div>{{msg}}</div>
      <div>
          当输入框中内容改变的时候，  页面上的msg  会自动更新
        <input type="text" v-model='msg'>
      </div>
  </div>
```

### 1.1.7. mvvm

- MVC 是后端的分层开发概念； MVVM是前端视图层的概念，主要关注于 视图层分离，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel
- m  [model]  
  - 数据层   Vue  中 数据层 都放在 data 里面
- v   [view]     视图   
  - Vue  中  view      即 我们的HTML页面  
- vm   [view-model]     控制器     将数据和视图层建立联系      
  - vm 即  Vue 的实例  就是 vm  

### 1.1.8. v-on

- 用来绑定事件的
-  形式如：v-on:click  缩写为 @click;

![@click](_v_images/20200211170641210_21396.png =577x)

#### 1.1.8.1. v-on事件函数中传入参数

```html

<body>
    <div id="app">
        <div>{{num}}</div>
        <div>
            <!-- 如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数 -->
            <button v-on:click='handle1'>点击1</button>
            <!-- 2、如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，
                 并且事件对象的名称必须是$event 
            -->
            <button v-on:click='handle2(123, 456, $event)'>点击2</button>
        </div>
    </div>
    <script type="text/javascript" src="js/vue.js"></script>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#app',
            data: {
                num: 0
            },
            methods: {
                handle1: function(event) {
                    console.log(event.target.innerHTML)
                },
                handle2: function(p, p1, event) {
                    console.log(p, p1)
                    console.log(event.target.innerHTML)
                    this.num++;
                }
            }
        });
    </script>
```

#### 1.1.8.2. 事件修饰符

- 在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。
- Vue 不推荐我们操作DOM    为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**
- 修饰符是由点开头的指令后缀来表示的

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联   即阻止冒泡也阻止默认事件 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。
```

#### 1.1.8.3. 按键修饰符

- 在做项目中有时会用到键盘事件，在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">

<!-- -当点击enter 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<!--当点击enter或者space时  时调用 `vm.alertMe()`   -->
<input type="text" v-on:keyup.enter.space="alertMe" >

常用的按键修饰符
.enter =>    enter键
.tab => tab键
.delete (捕获“删除”和“退格”按键) =>  删除键
.esc => 取消键
.space =>  空格键
.up =>  上
.down =>  下
.left =>  左
.right =>  右

<script>
	var vm = new Vue({
        el:"#app",
        methods: {
              submit:function(){},
              alertMe:function(){},
        }
    })

</script>
```

#### 1.1.8.4. 自定义按键修饰符别名

- 在Vue中可以通过`config.keyCodes`自定义按键修饰符别名

```html
<div id="app">
    预先定义了keycode 116（即F5）的别名为f5，因此在文字输入框中按下F5，会触发prompt方法
    <input type="text" v-on:keydown.f5="prompt()">
</div>

<script>
	
    Vue.config.keyCodes.f5 = 116;

    let app = new Vue({
        el: '#app',
        methods: {
            prompt: function() {
                alert('我是 F5！');
            }
        }
    });
</script>
```

### 1.1.9. v-bind

- v-bind 指令被用来响应地更新 HTML 属性
- v-bind:href    可以缩写为    :href;

```html
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">

<!-- 缩写 -->
<img :src="imageSrc">
```

#### 1.1.9.1. 绑定对象

- 我们可以给v-bind:class 一个对象，以动态地切换class。
- 注意：v-bind:class指令可以与普通的class特性共存

```html
1、 v-bind 中支持绑定一个对象 
	如果绑定的是一个对象 则 键为 对应的类名  值 为对应data中的数据 
<!-- 
	HTML最终渲染为 <ul class="box textColor textSize"></ul>
	注意：
		textColor，textSize  对应的渲染到页面上的CSS类名	
		isColor，isSize  对应vue data中的数据  如果为true 则对应的类名 渲染到页面上 


		当 isColor 和 isSize 变化时，class列表将相应的更新，
		例如，将isSize改成false，
		class列表将变为 <ul class="box textColor"></ul>
-->

<ul class="box" v-bind:class="{textColor:isColor, textSize:isSize}">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
  <div v-bind:style="{color:activeColor,fontSize:activeSize}">对象语法</div>

<sript>
var vm= new Vue({
    el:'.box',
    data:{
        isColor:true,
        isSize:true，
    	activeColor:"red",
        activeSize:"25px",
    }
})
</sript>
<style>

    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```

#### 1.1.9.2. 绑定class

```html
2、  v-bind 中支持绑定一个数组    数组中classA和 classB 对应为data中的数据

这里的classA  对用data 中的  classA
这里的classB  对用data 中的  classB
<ul class="box" :class="[classA, classB]">
    <li>学习Vue</li>
    <li>学习Node</li>
    <li>学习React</li>
</ul>
<script>
var vm= new Vue({
    el:'.box',
    data:{
        classA:‘textColor‘,
        classB:‘textSize‘
    }
})
</script>
<style>
    .box{
        border:1px dashed #f0f;
    }
    .textColor{
        color:#f00;
        background-color:#eef;
    }
    .textSize{
        font-size:30px;
        font-weight:bold;
    }
</style>
```

#### 1.1.9.3. 绑定对象和绑定数组的区别

- 绑定对象的时候 对象的属性 即要渲染的类名 对象的属性值对应的是 data 中的数据 
- 绑定数组的时候数组里面存的是data 中的数据 

#### 1.1.9.4. 绑定style

```html
 <div v-bind:style="styleObject">绑定样式对象</div>'
 
<!-- CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来)    -->
 <div v-bind:style="{ color: activeColor, fontSize: fontSize,background:'red' }">内联样式</div>

<!--组语法可以将多个样式对象应用到同一个元素 -->
<div v-bind:style="[styleObj1, styleObj2]"></div>


<script>
	new Vue({
      el: '#app',
      data: {
        styleObject: {
          color: 'green',
          fontSize: '30px',
          background:'red'
        }，
        activeColor: 'green',
   		fontSize: "30px"
      },
      styleObj1: {
             color: 'red'
       },
       styleObj2: {
            fontSize: '30px'
       }

</script>
```

### 1.1.10. 分支结构

#### 1.1.10.1. v-if 使用场景

- 1- 多个元素 通过条件判断展示或者隐藏某个元素。或者多个元素
- 2- 进行两个视图之间的切换

```html
<div id="app">
        <!--  判断是否加载，如果为真，就加载，否则不加载-->
        <span v-if="flag">
           如果flag为true则显示,false不显示!
        </span>
</div>

<script>
    var vm = new Vue({
        el:"#app",
        data:{
            flag:true
        }
    })
</script>

----------------------------------------------------------

    <div v-if="type === 'A'">
       A
    </div>
  <!-- v-else-if紧跟在v-if或v-else-if之后   表示v-if条件不成立时执行-->
    <div v-else-if="type === 'B'">
       B
    </div>
    <div v-else-if="type === 'C'">
       C
    </div>
  <!-- v-else紧跟在v-if或v-else-if之后-->
    <div v-else>
       Not A/B/C
    </div>

<script>
    new Vue({
      el: '#app',
      data: {
        type: 'C'
      }
    })
</script>
```

#### 1.1.10.2. v-show 和 v-if的区别

- v-show本质就是标签display设置为none，控制隐藏
  - v-show只编译一次，后面其实就是控制css，而v-if不停的销毁和创建，故v-show性能更好一点。
- v-if是动态的向DOM树内添加或者删除DOM元素
  - v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件

### 1.1.11. 循环结构

#### 1.1.11.1. v-for

- 用于循环的数组里面的值可以是对象，也可以是普通元素  

```html
<ul id="example-1">
   <!-- 循环结构-遍历数组  
	item 是我们自己定义的一个名字  代表数组里面的每一项  
	items对应的是 data中的数组-->
  <li v-for="item in items">
    {{ item.message }}
  </li> 

</ul>
<script>
 new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]，
   
  }
})
</script>
```

- **不推荐**同时使用 `v-if` 和 `v-for`
- 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。

```html
   <!--  循环结构-遍历对象
		v 代表   对象的value
		k  代表对象的 键 
		i  代表索引	
	---> 
     <div v-if='v==13' v-for='(v,k,i) in obj'>{{v + '---' + k + '---' + i}}</div>

<script>
 new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]，
    obj: {
        uname: 'zhangsan',
        age: 13,
        gender: 'female'
    }
  }
})
</script>
```

- key 的作用
  - **key来给每个节点做一个唯一标识**
  - **key的作用主要是为了高效的更新虚拟DOM**

```html
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>

```
## 1.2. 生命周期
>生命周期：Vue实例从创建到销毁的过程。Vue在这些过程中会调用一些被称为钩子的函数。
### 1.2.1. Vue框架生命周期图示
![Vue生命周期图示](_v_images/20200212141729948_31877.png =720x)
## 1.3. Vue组件
> 组件是可复用的Vue实例
### 1.3.1. 组件构造器
`var MyComponent = Vue.extend({...})`
组件名的命名方式有两种：
* 使用kebab-case(短横线分隔命名)
    * 例：`<my-component-name>`
* 使用PascalCase(首字母大写命名)
    * 例：`<MyComponentName>`
* 直接在Dom(即非字符串的模板)中使用只用第一种(kebab-case----短横线分隔命名法)有效
### 1.3.2. 组件注册
在使用组件之前需要注册才能使用。Vue提供了**全局注册**和**局部注册**两种方式
#### 1.3.2.1. 全局注册
>全局注册需要确保在根实例初始化之前注册，这样才能使用组件在任意实例中使用
注册方式 `Vue.component('组件名称', MyComponent)`

**使用方法：**
```html
<!-- 组件使用 --> 
<div id="app">
    <my-component></my-component>
</div>

<!-- 构造组件 --> 
var MyComponent = Vue.extend({
    template:'<p>This is a component</p>'
})

<!-- 组件注册 --> 
Vue.component('my-component', MyComponent)
<!-- Vue实例 -->
var vm = new Vue({
    el:'#app'
})
```
#### 1.3.2.2. 局部注册
>局部注册：只能在被注册的组件中使用

**使用方法：**
```html
<!-- 构造组件 -->
var Child = Vue.extend({
    template: '<p>This is a child component</p>'
})

<!-- 组件局部注册 -->
var Parent = Vue.extend({
    template: "<div>
        <p>This is a parent component</p>
        <my-child></my-child>
        </div>",
        components:{
            'my-child': Child
        }
})
```
#### 1.3.2.3. 注册语法糖
>Vue.js在注册方式上提供了简化方法，可以直接在注册的时候定义组件构造器选项

**使用方法：**
```html
<!-- 全局注册 -->
Vue.component('my-component', {
    template: '<p>This is a component</p>'
})

<!-- 局部注册 -->
var Parent = Vue.extend({
    template: '<div>
        <p>This is a parent component</p>
        <my-child></my-child>
    </div>',
    components: {
        'my-child': {
            template: '<p>This is a child component</p>'
        }
    }
})
```
#### 1.3.2.4. 在模板系统中局部注册
* 官方推荐创建一个 components 目录，并将每个组件放置在其各自的文件中
* 在局部注册之前导入每个想使用的组件

**使用方法：**
```javascripts
//例如，在一个假设的 ComponentB.js 或 ComponentB.vue文件中使用ComponentA、ComponentB
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
    components: {
        ComponentA,
        ComponentC
    },
    //...
}
```
### 1.3.3. 组件选项
组件选项与Vue选项的区别：
* Vue构造器：
```javascript
var vm = new Vue({
    el: '#app',
    data: {
        name: 'Vue'
    }
})
```
* 组件定义：
```javascripts
var MyComponent = Vue.extend({
        data: function() {
            return {
                name: 'component'
            }
        }
})
```
#### 1.3.3.1. 组件Props
>props是组件中非常重要的一个选项，起到父组件与子组件通讯的作用

* 组件实例的作用域是孤立的，子组件是无法直接使用父组件的数据的
* 需要使用props将父组件的数据传递给子组件，子组件在接受数据时需要显示声明props

**使用方法：**
```html
<!-- 组件构造 -->
Vue.component('my-child', {
    props: ['parent'],
    template: '<p>{{parent}} is from parent</p>'
})

<!-- 组件实例使用 -->
<my-child parent="this data"></my-child>
```
**props命名：**
* 标签中使用`<my-child  myParam=""></my-child>` 对应props选项为：`props: ['myparam']`
* 标签中使用`<my-child  my-param=""></my-child>` 对应props选项为：`props: ['myParam']`

##### 1.3.3.1.1. 动态使用Props
>可以通过v-bind指令的方式将父组件的data数据传递给子组件

**使用方法：**
```html
<div id="app">
    <input type="text" v-model="message"/>
    <my-component v-bind:message="message"></my-component>
</div>

var MyComponent = Vue.extend({
    props: ['message'],
    template: "<p>{{'From parent:' + message}}</p>"
})

Vue.component('my-component', MyComponent)

var vm = new Vue({
    el: '#app',
    data: {
        message: 'default'
    }
})
```
~~###### 1.3.3.1.1.1. v-bind使用修饰符 .sync和.once进行不同方式的绑定~~
默认v-bind绑定为数据单项绑定
* .sync 声明为双向绑定
* .once 声明为单次绑定

###### 1.3.3.1.1.1. Props数据类型验证
`props:{a: Number}` 即验证参数a  需为Number类型
* 基础类型检测： prop: Number，接受的参数为原生构造器，任何类型均可
* 参数可以为多种类型： `prop: [Number,String]`
* 参数必须： `prop:{type: Number,required: true}`,即参数必须有值且为Number类型
* 默认参数：`prop:{type: Number,default: 10}`,参数具有默认值
    * 如果默认值为对象或数组，需要通过函数返回值的形式赋值。
     
     ```javascripts
     prop: {
         type: Object,
         default: function() {
             return { a: 'a'}
         }
     }
     ```
* 绑定类型：`prop:{twoWay: ture}`, 校验绑定类型，验证失败会抛出一条警告
* 自定义验证函数： `prop: {validator:function(value) { return value > 0}}`,验证值必须大于0
### 1.3.4. 组件间通信
#### 1.3.4.1. 直接访问
* `this.$parent`：访问父组件实例
* `this.$children`: 访问子组件实例
* `this.$root`: 组件所在的根实例
#### 1.3.4.2. 通过定义ref属性，访问组件实例
**使用方法：**
定义ref：`<base-input ref='usernameInput'></base-input>`
通过ref访问：`this.$refs.usernameInput`
#### 1.3.4.3. 子组件向父组件传递数据
* 如何做？
    * 需要自定义事件完成子组件向父组件的数据传递
* 自定义事件流程
    * 在子组件中，通过$emit()来触发事件
    * 在父组件中，通过v-on来监听子组件事件
 * **使用方法：**
 ![](_v_images/20200213154954297_903.png =716x)
 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<body>
<div id="app">
  <child-cpn @increment="changeTotal" @decrement="changeTotal"></child-cpn>
    <h2>点击次数：{{ total }}</h2>
</div>
</body>
<template id="childCpn">
    <div>
        <button @click="increment">+1</button>
        <button @click="decrement">-1</button>
    </div>
</template>
<script>
    var app1 = new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue!',
        total: 0
      },
        methods: {
            changeTotal(counter) {
                this.total = counter
            }
        },
        components: {
          'child-cpn': {
              template: '#childCpn',
              data() {
                  return {
                      counter: 0
                  }
              },
              methods: {
                  increment() {
                      this.counter++;
                      this.$emit('increment', this.counter)
                  },
                  decrement() {
                      this.counter--;
                      this.$emit('decrement', this.counter)
                  }
              }
          }
        }
    })
</script>
</html>
```
### 1.3.5. 组件插槽使用
这里主要记录了作用域插槽使用
**使用方法：**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<body>
<div id="app">
    <my-cpn>
        <template v-slot="slotProps">
            <ul>
                <li v-for="info in slotProps.data">{{ info }}</li>
            </ul>
            <span v-for="info in slotProps.data">{{ info }}</span>
        </template>
    </my-cpn>
</div>
</body>
<template id="myCpn">
    <div>
        <slot :data="planguages"></slot>
    </div>
</template>
<script>
    var app1 = new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue!',
        total: 0
      },
        methods: {
            changeTotal(counter) {
                this.total = counter
            }
        },
        components: {
          'my-cpn': {
              template: '#myCpn',
              data() {
                  return {
                      planguages: [
                          'Javascripts', 'Python', 'Swift'
                      ]
                  }
              }
          }
        }
    })
</script>
</html>
```
## 1.4. 常用模块化规范
* CommonJS
* AMD
* CMD
* ES6的Module
### 1.4.1. CommonJS
* CommonJS的导出
```javascripts
moule.exports = {
    flag: true,
    test(a, b) {
        return a + b
    },
    demo(a, b) {
        return a * b
    }
}
```

* CommonJS的导入
```javascripts
// CommonJS模块
let { test, demo, flag } = require(moduleA);
//等同于
let _mA = require('moduleA');
let test = _mA.test;
let demo = _mA.demo;
let flag = _mA.flag;
```
* export基本使用
![](_v_images/20200213165235164_4555.png =307x)
//另一种写法
![](_v_images/20200213165314116_7187.png =323x)

* 导出函数或类
    * 方式一
    ![](_v_images/20200213165513596_27123.png =434x)
    * 方式二
    ![](_v_images/20200213165528508_22103.png =445x)

* export default
>export default在同一个模块中，不允许同时存在多个
**实例方法：**
![](_v_images/20200213170807857_31375.png =357x)
![](_v_images/20200213170822447_20332.png =357x)

* import使用
>使用import命令可以用加载对应的模块

**使用方法：**
首先，我们需要在HTML代码中引入两个js文件，并且类型需要设置为module
![](_v_images/20200213171503767_21304.png =537x)
然后使用import导入模块的内容
![](_v_images/20200213171602168_11231.png =466x)

通过`*`可以导入模块中所有的export变量
但是通常情况下我们需要给*起一个别名，方便后续的使用
![](_v_images/20200213171700645_29747.png =471x)
## 1.5. Vue CLI
>CLI 全称 Command-Line Interface ，命令行界面 俗称：脚手架
>Vue CLI是一个官方发布的 vue.js项目脚手架
>使用vue-cli可以快速搭建Vue开发环境以及对应的webpack配置
* Vue脚手架安装
    * npm install -g @vue/cli
### 1.5.1. npm
>npm 全称Node Package Manager
>是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node（包）模块的标准
### 1.5.2. webpack
>Vue.js官方脚手架工具使用了webpack模板
* 全局安装
    * npm install webpack -g
## 1.6. 异步编程解决方案-Promise
```javascripts
new Promise((resolve, reject) => {
    ...
}).then(data =>{
    ...
}).catch(error =>{
    ...
})
```
* 异步事件解析
    * new Promise很明显是创建一个Promise对象
    * 小括号中((resolve, reject) => {})也是一个箭头(=>)函数。
    * resolve和reject它们两个也是函数，通常情况下，我们会根据请求数据的成功和失败来决定调用哪一个。
        * 如果是成功的，那么通常我们会调用resolve(messsage)，这个时候，我们后续的then会被回调。
        * 如果是失败的，那么通常我们会调用reject(error)，这个时候，我们后续的catch会被回调。

![](_v_images/20200214154453185_26892.png =681x)
* Promise链式调用
![](_v_images/20200214154625546_4853.png =681x)
* Promise 链式调用简写
![](_v_images/20200214154738297_31686.png =681x)
## 1.7. axiox请求
* 支持的请求方式
    * axios(config)
    * axios.request(config)
    * axios.get(url[, config])
    * axios.delete(url[, config])
    * axios.head(url[, config])
    * axios.post(url[, data[, config]])
    * axios.put(url[, data[, config]])
    * axios.patch(url[, data[, config]])
### 1.7.1. 发送get请求
![](_v_images/20200214155538116_18771.png =681x)
### 1.7.2. 发送并发请求
* 使用axios.all，可以放入多个请求的数组
* axios.all([])返回的结果是一个数组，使用axios.spread可将数组[res1, res2]展开
![](_v_images/20200214155751338_16737.png =681x)
