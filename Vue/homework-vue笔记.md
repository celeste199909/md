# vue笔记

## 基础语法

- 模板语法

  在Vue中要想把数据渲染在页面上只需要把要渲染的数据放在两个大括号中即可（而不用操作DOM，Vue的一个思想就是数据驱动），里面可以放变量，也可以是表达式。

  ```javascript
  // ...
  <div>{{name}}</div>
  // ...
  export default{
    data(){
      return{
        name: "hgw"
      }
    }
  }
  ```

- 计算属性

  当一个数据需要经过一系列计算才能得到，并且可能依赖其他数据时，我们使用计算属性。

  ```js
  // ...
  <div>{{greeting}}</div>
  // ...
  export default{
    data(){
      return{
        name: "hgw"
      }
    },
    computed:{
      greeting(){
        return "hello" + this.name;
      }
    }
  }
  ```

  计算属性只计算一次就会将结果缓存起来，只有当它依赖的值改变时才会重新计算，这样就减少了性能的消耗，比如这里的`name`改变时它才会重新计算。

- 侦听器

  当有一个数据发生变化时，我们想要做让其他的数据随之发生变化（其实也不止如此我们还可以做其他的事情），这时我们就可以用`watch`

  ```js
  // ...
  <div>
    <div>{{a}}</div>
  	<div>{{b}}</div>
  </div>
  // ...
  export default{
    data(){
      return{
        a: "hehe"
        b: "b还没变"
      }
    },
    watch: {
      a(newValue, oldValue){
        this.b = "b随着a变了"
      }
    },
  }
  ```

  watch可以监听到某个数据的变化，当其变化时就会触发相应的函数（函数名是以该数据的名字命名）。

## 生命周期

[声明周期图示](https://cn.vuejs.org/images/lifecycle.png)

当我们想要在页面加载到某个时候或者数据更新又或者退出页面时做一些事情，那么我们就可以使用生命周期钩子函数。

比如我要在页面加载出来之前请求一个接口，以便获得数据来渲染页面：

```js
// ...
created(){
  axios.get("api")
}
// ...
```

一个组件钩子函数的执行顺序：

 created => mounted => updated => destroyed

其中`updated`（和`beforeUpdate`）在数据更新时触发。

> 注意：如果一个组件有一个子组件，那么这个子组件的声明周期会发生在父组件的beforeMount之后

实际上每个钩子之前还有一个before钩子函数

>  除此之外还有`activated`和`deactivated`，他们与动态组件相关。
>
> `errorCaptured`，和错误捕捉相关。



## Vue Router

在做单页面应用时，我们想要实现页面的跳转，由于是单页面应用，当我们要跳转到另一个页面时并不需要向后端请求相应的路由地址，我们只要切换相应的视图（或者说组件）即可。

Vue Router为我们提供了切换视图的方式。

一般来说，我们通过点击a链接来跳转页面，在Vue Router 中，我们使用`router-link`来切换视图。

```html
// ...
<nav>
  <router-link to="/">home</router-link>
	<router-link to="/about">about</router-link>
</nav>
<main>
	<router-view><router-view>    
</main>
// ...
```

当然了，我们必须使用`router-view`来说明切换的视图应该出现在哪里。

这样用起来好像不够灵活，跳转的页面是固定的。比如在登录的时候我们需要根据用户的登录信息正确与否来决定是否跳转，或者跳转到相应的页面，那么我们就可以使用编程式路由。

```js
// 当登录信息验证正确后，跳转到首页
router.push('home')
```



## 组件传值

- props

  当父组件要给子组件传值时可以使用`props`

  ```js
  // 父组件
  <Child name="hgw"></Child>
  // ...
  
  // 子组件
  ...
  <div>{{name}}</div>
  ...
  export default {
     props:["name"]
  }
  ```

  在子组件中使用接收到的值就像使用自己的data即可。

- emit

  自定义事件，当子组件给父组件传值时使用。

  ```js
  // 子组件发出自定义事件，并附带一个数据
  this.$emit("deliver","msg1","msg2")
  
  // 父组件监听该自定义事件
  <Child @deliver="handleDeliver"></Child>
  // ...
  methods:{
    handleDeliver(v1, v2){
      console.log(v1, v2);
    }
  }
  // ...
  ```

  在这里子组件自定义了一个事件名为`deliver`，并附带一个数据，当`this.$emit("deliver","give you a msg")`被执行时，父组件可以监听到该事件，`@自定义名称`即可监听到，等号后面是处理函数，该函数的参数就是子组件自定义事件附带过来的数据，可以传多个。


## Vuex 

组件之间传值有很多种方式，比如常用的props，$emit，此外还有很多。但是这些方式传递起来非常麻烦，尤其是在多级关系的组件之间传递的时候，或者当多个组件同时依赖一个数据的时候要修改数据让依赖该数据的组件都相应的发生改变操作起来会很不方便。这时候就得用到Vuex了。

Vuex就像一个数据仓库，每个组件都能访问和修改其中的数据。