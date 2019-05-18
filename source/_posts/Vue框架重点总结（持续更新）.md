---
title: Vue框架重点总结（持续更新）
date: 2018-08-15 19:55:14
author: huangkuiwei
img: http://www.manongjc.com/images/51jb/154648858215Q4G648Pl8582.png
tags: 
    - Vue
    - JavaScript
---
在这篇文章中，我会总结一些我在学习Vue中觉得很重要而且易错的知识点，方便以后查阅。
1. Vue响应式系统：当属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

2. 在Vue实例中，只有当实例被创建时 data 中存在的属性才是响应式的。如果视图中使用到了 data 中未定义的数据时，当此数据改变，视图不会同步更新。**（如果数据不需要应用在视图中，也可以选择不在 data 选项中进行定义）**,如果你知道你会在晚些时候视图需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。例如：
```javascript
  new Vue({
    el: '#app',
    data: {
      newTodoText: '',
      visitCount: 0,
      hideCompletedTodos: false,
      todos: [],
      error: null
    }
  })
```

3. Vue 实例暴露了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。例如：
```javascript
  let data = { a: 1 };
  let vm = new Vue({
    el: '#example',
    data: data
  });
  
  vm.$data === data; // => true
  vm.$el === document.getElementById('example'); // => true
  
  // $watch 是一个实例方法
  vm.$watch('a', function (newValue, oldValue) {
    // 这个回调将在 `vm.a` 改变后调用
  })
```

4. 指令 (Directives) 是带有 v- 前缀的特殊特性。指令特性的值预期是单个 JavaScript 表达式 (v-for 是例外情况)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

5. 动态参数（2.6.0新增）
```html
  <a v-bind:[attrHref] = "href"></a>
```
```javascript
  new Vue({
    data: {
      attrHref: 'href',
      href: 'https://www.baidu.com'
    }
  })
```
这里的 attrHref 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。
例如，如果你的 Vue 实例有一个 data 属性 attrHref，其值为 "href"，那么这个绑定将等价于 v-bind:href。<br>
同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：
```html
  <input v-on:[eventName] = "doSomething" type="text">
```
当 eventName 的值为 "focus" 时，v-on:[eventName] 将等价于 v-on:focus。
>1. 注意：动态参数预期会求出一个字符串，异常情况下值为 null。这个特殊的 null 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。
>2. 动态参数表达式有一些语法约束，因为某些字符，例如空格和引号，放在 HTML 特性名里是无效的。同样，在 DOM 中使用模板时你需要回避大写键名。

  ```html
    <a v-bind:['foo' + bar] = "url">链接</a>
  ```
  上面的代码是无效的：变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。
  
6. 我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。**相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。**

7. 计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter：
```html
  <div id="app">{{fullName}}</div>
```
```javascript
  // 默认情况只有getter
  let app = new Vue({
    el: '#app',
    data: {
      firstName: 'hello',
      lastName: 'world'
    },
    computed: {
      fullName() {
        return this.firstName + this.lastName
      }
    }
  })
```
此时 fullName 会随着 firstName 和 lastName 的值改变而改变，而且无法直接修改 fullName 的值，会报错。
```javascript
  // 设置setter
  let app = new Vue({
    el: '#app',
    data: {
      firstName: 'hello',
      lastName: 'world'
    },
    computed: {
      fullName: {
        get() {
          return this.firstName + this.lastName
        },
        set(value) {
          let name = value.split(' ');
          this.firstName = name[0];
          this.lastName = name[1]
        }
      }
    }
  })
```
现在再运行 app.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。

8. v-if 和 v-show 对比：
v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
>1. v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
>2. v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
>3. 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
>4. 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

9. 建议尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
>不要使用对象或数组之类的非原始类型值作为 v-for 的 key。用字符串或数类型的值取而代之。

10. Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：
>1. push()
>2. pop()
>3. shift()
>4. unshift()
>5. splice()
>6. sort()
>7. reverse()

11. 关于数组变动的注意事项：
由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
>1. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
>2. 当你修改数组的长度时，例如：vm.items.length = newLength

  ```javascript
    let vm = new Vue({
      data: {
        items: ['a', 'b', 'c']
      }
    });
    vm.items[1] = 'x';    // 不是响应性的
    vm.items.length = 2   // 不是响应性的
  ```
  对于上面的例子中的两个问题，我们可以用下面的方式解决。
  ```javascript
  vm.items.splice(1, 1, 'x');
  vm.items.splice(1)
  ```
  
12. 由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除：
```javascript
  let app = new Vue({
      data: {
        a: 1
      }
  });
  app.b = 2   // 不是响应式的
```
对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。

13. 使用 Vue.set 方法添加或者修改响应式属性：
>1. 数组：Vue.set(target, index, value)。
>2. 对象：Vue.set(target, propertyName, value)。

  你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名。
  ```html
    <div id="app">
      <button @click="modifyList">按钮</button>
      <p v-for="item in list">
        {{item.name}}
      </p>
    </div>
  ```
  ```javascript
    let app = new Vue({
      el: '#app',
      data: {
        list: [
          {name: '张三', age: '22', address: ['长沙市', '娄底市']},
          {name: '李四', age: '33', address: ['株洲市', '湘潭市']}
        ]
      },
      methods: {
        modifyList() {
          // 第一种修改方式
          this.$set(this.list, 1, {name: '王五', age: '33', address: ['株洲市', '湘潭市']});
          // 第二种修改方式（直接定位到要修改的值的上一级）
          this.$set(this.list[1], 'name', '王五')
        }
      }
    });
  ```
  有时你可能需要为已有对象赋予多个新属性，比如使用 Object.assign() 或 _.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：
  ```javascript
    Object.assign(vm.userProfile, {
      age: 27,
      favoriteColor: 'Vue Green'
    })
  ```
  你应该这样做：
  ```javascript
    vm.userProfile = Object.assign({}, vm.userProfile, {
      age: 27,
      favoriteColor: 'Vue Green'
    })
  ```
  
14. 不推荐同时使用 v-if 和 v-for。
当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
  ```html
    <ul>
      <!-- 如果 list 不存在或者为空数组的时候，后面紧跟的 v-else 没有起作用 -->
      <!-- 因为当它们处于同一节点，v-for 的优先级比 v-if 更高，没有循环，所以后面的 v-if 也就相当于没有 -->
      <li v-for="item in list" v-if="list && list.length > 0">{{item.name}}</li>
      <!--<li v-else>选项为空</li>-->
      <!-- 替代方法 -->
      <li v-if="list && !list.length>选项为空</li>
  </ul>
  ```
  
15. 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。

16. 事件修饰符：
>1. stop    // 阻止事件冒泡
>2. prevent // 阻止默认事件
>3. capture // 采用事件捕获模式
>4. self    // 当e.currentTarget === e.target时触发
>5. once    // 事件只触发一次
>6. passive // addEventListener 中的 passive 选项

  不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，.passive 会告诉浏览器你不想阻止事件的默认行为。
```html
  <!-- 阻止单击事件继续传播 -->
  <a v-on:click.stop="doThis"></a>
  
  <!-- 提交事件不再重载页面 -->
  <form v-on:submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a v-on:click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form v-on:submit.prevent></form>
  
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
  <div v-on:click.capture="doThis">...</div>
  
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div v-on:click.self="doThat">...</div>
  
  <!-- 点击事件将只会触发一次 -->
  <a v-on:click.once="doThis"></a>
```

17. 关于Vue响应式的一点实践总结：
  1. Vue 不能检测对象属性的添加或删除（修改已经存在的属性是可以检测的）。
  ```html
      <!-- 给 info 对象添加 age 属性，页面不是响应式的 -->
      <button @click="info.age=22">按钮</button>
      <p>{{info.age}}</p>
  ```
  ```html
      <!-- 删除 info 对象的 name 属性后，页面的 'huang' 还是会存在，不是响应式的 -->
      <button @click="deleteInfoName">按钮</button>
      <p>{{info.name}}</p>
  ```
  ```html
      <!-- 修改 info 对象的 name 属性，页面会跟着变化，是响应式的。 -->
      <button @click="info.name='zhang'">按钮</button>
      <p>{{info.name}}</p>
  ```
  ```javascript
      new Vue({
        el: '#app',
        data: {
          info: {
            name: 'huang'
          }
        },
        methods: {
          deleteInfoName() {
            delete this.info.name
          }
        }
      })
  ```
  2. 对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。
  ```javascript
      let vm = new Vue({
        data: {
          a: 1
        }
      });
      // `vm.a` 现在是响应式的
      vm.b = 2
      // `vm.b` 不是响应式的
  ```
  3. 使用 v-model 绑定属性问题，见实例： 
  ```html
      <!-- 会报错，因为 message 没有在 Vue 实例中进行定义 -->
      <input type="text" v-model="message">
  ```
  ```html
      <!-- 虽然 info 中没有 name 属性，不过并不会报错，而且 info.name 是响应式的。 -->
      <input type="text" v-model="info.name">
      <p>{{info.name}}</p>
  ```
  ```javascript
      new Vue({
        el: '#app',
        data: {
          info: {}  
        }
      })
  ```
  根据上面的例子我们可以知道，如果 v-model 绑定了未在 Vue 实例中定义的根属性，会报错，但是如果绑定了根属性下一级的对象里面的属性是不会报错的，而且该属性也会响应式。
  
  4. 其它例子：
  ```html
      <!-- 是响应式的 -->
      <button @click="info.list = ['1', '2']">按钮</button>
      <p>{{info.list}}</p>
  ```
  ```html
      <!-- 不是响应式的 -->
      <button @click="info.list[2] = '王五'">按钮</button>
      <p>{{info.list}}</p>
  ```
  ```javascript
      new Vue({
        el: '#app',
        data: {
          info: {
            list: ['张三', '李四']
          }
        }
      })
  ```
  >1. 直接修改某个值（不能是数组的某个索引），使它变为另外一个原始值或者指向另外一个指针，此时是响应式的。
  >2. 修改或者添加数组某个索引的值不能使用：vm.items[indexOfItem] = newValue。要使用 Vue.set()。
  >3. 新增或者删除对象的值也要使用 Vue.set()。
  
18. $refs 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 $refs。
19. 当 ref 和 v-for 一起使用的时候，你得到的引用将会是一个包含了对应数据源的这些子组件的数组。
