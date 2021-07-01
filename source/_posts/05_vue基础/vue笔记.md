---
title: 【vue基础_01】vue笔记
categories: vue笔记
tags:
  - vue
  - web前端
excerpt: vue笔记
date: 2021-07-01 20:33:36
img: '/images/vue/vue.png'
---
## 一、第一个Vue应用
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
		<title>创建一个 Vue 实例</title>
	</head>
	<body>
		<div id="app">{{message}}</div>
	</body>
	<script>
		var app = new Vue({
			el: "#app",
			data: {
				message: "hello Vue"
			}
		})
	</script>
</html>
```

## 二、生命周期
下图展示了实例的生命周期。你不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。
![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

## 三、Vue语法

### 3.1 条件渲染

#### 3.1.1 v-if 

```html
<div id="app">
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
</div>
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
    awesome: true
    }
})
```
#### 3.1.2 v-else-if 连续条件渲染

```html
<div id="app">
    <h1 v-if="type==='A'">A</h1>
    <h1 v-else-if="type==='A'">B</h1>
    <h1 v-else-if="type==='C'">C</h1>
    <h1 v-else> Not A/B/C</h1>
</div>
```

  ```javascript
  var app = new Vue({
      el: "#app",
          data: {
          	type: 'D'
      	}
  	})
  ```



### 3.2 列表渲染

#### 3.2.1  v-for 遍历数组

```html
<li v-for="item in items">
	{{item.message}}
</li>
```


```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo'},
      { message: 'Bar'}
    ]
  }
})
```

#### 3.2.2 v-for 获取数组索引值

```html
<ul id="example-2">
    <li v-for="(item,index) in items">
    	{{ parentMessage }} - {{ index }} - {{ item.message }}
    </li>
</ul>
```

```javascript
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',  
    items: [
      { message: 'Foo'},
      { message: 'Bar'}
    ]
  }
})
```

#### 3.2.3 v-for 里使用对象

```html
<ul id="example-3">
    <li v-for="value in object">
    	{{ value }}
    </li>
</ul>
```

```javascript
var example3 = new Vue({
  el: '#example-3',
  data: {
	object: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
	}
  }
})
```
#### 3.2.4 v-for 循环键名

```html
<ul id="example-4">
    <li v-for="(value, name) in object">
    {{ name }}: {{ value }}
    </li>
</ul>
```

```javascript
var example4 = new Vue({
  el: '#example-4',
  data: {
	object: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
	}
  }
})
```

#### 3.2.5 v-for 三个参数作为索引

```html
<ul id="example-5">
    <li v-for="(value, name, index) in object">
    	{{ index }}. {{ name }}: {{ value }}
    </li>
</ul>
```

```javascript
var example5 = new Vue({
  el: '#example-5',
  data: {
	object: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
	}
  }
})
```

### 3.3 事件处理

```html
<div id="app">
	<button v-on:click="greet">Greet</button>
</div>
```

```javascript
var app = new Vue({
	el: "#app",
	data: { message: "hello vue"},
	methods: {
		greet: function() {
			alert(this.message)
		}
	}
})
```

## 四、axios应用程序

### 4.1 安装方法

**使用 cdn:**

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

**使用 npm:**

```shell
$ npm install axios
```

**使用 bower:**

```shell
$ bower install axios
```

**使用 yarn:**

```shell
$ yarn add axios
```

### 4.2 传递参数

```javascript
axios.post('/user', {
    firstName: 'Fred',        // 参数 firstName
    lastName: 'Flintstone'    // 参数 lastName
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```



### 4.3 简单示例

json文件

```json
{
	"name": "百度",
	"url": "http://www.baicu.com",
	"page": 66,
	"links": [{
			"name": "Google",
			"url": "http://www.google.com"
		},
		{
			"name": "Baidu",
			"url": "http://www.baidu.com"
		},
		{
			"name": "Sougou",
			"url": "http://www.sougou.com"
		}
	]
}

```



```html
<div id="app">
    <div>
    	名称: {{info.name}}
    </div>
    <div>
    	链接: <a v-bind:href="info.url" target="_blank">{{info.url}}</a>
    </div>
    	<div v-for="item in info.links">
    		{{item.name}}----->
    	<a v-bind:href="item.url" target="_blank">{{item.url}}</a>
    </div>
</div>
```

```javascript
var app = new Vue({
	el: "#app",
	data() {
		return {
			info: {
				"name": '',
				"url": '',
				"links": []
			}
		}
	},
	mounted() {
		axios
			.get("links.json")
			.then(response => this.info = response.data)
	}
})
```

## 五、表单输入绑定

### 5.1 文本

```html
<div id="app">
    <input type="text" v-model="message" placeholder="edit me" />
    <p>Message is: {{ message }}</p>
</div>    
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
        message: ""
    }
})
```

### 5.2 多行文本
```html
<div id="app">
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
  <p>Message is: {{ message }}</p>
</div>    
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
        message: ""
    }
})
```
### 5.3 复选框

```html
<div id="app">
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
</div> 
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
        message: ""
    }
})
```
### 5.4 多个复选框
```html
<div id="app">
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
</div> 
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
       checkedNames: []
    }
})
```
### 5.5 单选按钮
```html
<div id="app">
    <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <br>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <br>
    <span>Picked: {{ picked }}</span>
</div> 
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
    	picked: "One"
    }
})
```
### 5.6 下拉框
```html
<div id="app">
    <select v-model="selected2" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
    </select>
    <br>
    <span>Selected: {{ selected2 }}</span>
</div> 
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
      selected: ""
    }
})
```
### 5.7 下拉框(多选绑定数组)
```html
<div id="app">
    <select v-model="selected2" multiple style="width: 50px;">
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select>
    <br>
    <span>Selected: {{ selected2 }}</span>
</div> 
```
```javascript
var app = new Vue({
    el: "#app",
    data: {
      selected2: []
    }
})
```

## 六、组件基础

```html
<div id="app">
    <ul>
    	<my-conponent-li v-for="item in items" v-bind:item="item"></my-conponent-li>
    </ul>
</div>
```

```javascript
Vue.component("my-conponent-li", {
			//参数
			props: ["item"],
			template: '<li>{{item}}</li>'
})
var app = new Vue({
    el: "#app",
    data: {
        items: ["古力娜扎", "迪丽热巴", "玛尔扎哈"]
	}
})
```

## 七、插槽内容

```html
<div id="app">
    <todo>
        <todo-title slot="todo-title" v-bind:title="title"></todo-title>
        <todo-items slot="todo-items" v-for="item in items" v-bind:item="item"></todo-items>
    </todo>
</div>
```

```javascript
Vue.component("todo", {
	template: '<div><slot name=\'todo-title\'></slot><ul><slot name=\'todo-items\'></slot></ul></div>'
})
Vue.component("todo-title", {
	props: ["title"],
	template: "<div>{{title}}</div>"
})
Vue.component("todo-items", {
	props: ["item"],
	template: "<li>{{item}}</li>"
})
var app = new Vue({
        el: "#app",
        data: {
        title: "我是标题",
        items: ["古力娜扎", "迪丽热巴", "玛尔扎哈"]
	}
})
```

## 八、自定义事件

```html
<div id="app">
    <todo>
        <todo-title slot="todo-title" v-bind:title="title"></todo-title>
        <todo-items slot="todo-items" 
                    v-for="(item,index) in items" v-bind:item="item" 
                    v-bind:index="index"
                    v-on:remove="removeItem"></todo-items>
    </todo>
</div>
```

```javascript
Vue.component("todo", {
    template: '<div><slot name=\'todo-title\'></slot><ul><slot name=\'todo-items\'></slot></ul></div>'
})
Vue.component("todo-title", {
    props: ["title"],
    template: "<div>{{title}}</div>"
})
Vue.component("todo-items", {
    props: ["item", "index"],
    template: "<li>{{index}}--{{item}}<button @click='remove'>删除</button></li>",
    methods: {
        remove: function() {
            this.$emit("remove")
        }
    }
})
var app = new Vue({
    el: "#app",
    data: {
        title: "我是标题",
        items: ["古力娜扎", "迪丽热巴", "玛尔扎哈"]
    },
    methods: {
        removeItem: function(index) {
            this.items.splice(index, 1)
        }
    }
})
```



## 九、计算属性

```html
<div id="app">
    <h1>当前时间方法{{getCurrentTime()}}</h1>
    <h1>当前时间属性{{getCurrentTime1}}</h1>
</div>
```

```javascript
var app = new Vue({
    el: "#app",
    //方法
    methods: {
        getCurrentTime: function() {
            return Date.now();
        }
    },
    computed: {
        getCurrentTime1: function() {
            return Date.now();
        }
    }
})
```

## 十、vue-cli

**安装node.js**

官方网站：[https://nodejs.org/en/](https://nodejs.org/en/)

 **安装npm淘宝镜像加速器**

```shell
 # 到node.js路径下运行管理员身份运行
 npm install -g cnpm --registry=https://registry.npm.taobao.org
 # 或者
 npm install --registry=https://registry.npm.taobao.org
```
 **安装vue-cli**

``` shell

npm install vue-cli -g

```
 **创建项目**

``` shell

vue init webpack demo

```
 **运行项目**
``` shell
`# To get started:
cd demo
npm install 
npm run dev
```

## 十 一、webPack安装及使用

### 11.1 安装

```shell
npm install webpack -g
npm install webpack-cli  -g
```

### 11.2 打包

```shell
webpack --watch
```

### 11.3 示例

文件目录

![](/images/vue/webpack_demo.png)

hello.js

```javascript
exports.sayHi=function(){
	document.write("<div>hello webpack!!!</div>")
}
```

main.js

```javascript
var hello=require("./hello");
hello.sayHi();
```
index.html
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<script src="dist/js/bundle.js"></script>
	</body>
</html>
```
webpack.config.js
```javascript
module.exports={
	entry: "./modules/main.js",
	output:{
		filename:"./js/bundle.js"
	}
}
```