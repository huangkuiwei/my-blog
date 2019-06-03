---
title: DOM - Element 节点
date: 2018-08-05 20:15:52
author: huangkuiwei
img: https://www.justplay.work/medias/postimages/09.png
tags: 
    - JavaScript
    - HTML
---
Element节点对象对应网页的 HTML 元素。每一个 HTML 元素，在 DOM 树上都会转化成一个Element节点对象（以下简称元素节点）。
```javascript
let div = document.querySelector('div');
console.log(div.nodeType, div.nodeName);     // 1 DIV
```
>注意：元素节点的 nodeType 都是1，元素节点的 nodeName 都是大写。

Element 对象继承了 Node 接口，因此 Node 的属性和方法在 Element 对象都存在。此外，不同的 HTML 元素对应的元素节点是不一样的，浏览器使用不同的构造函数，生成不同的元素节点，比如 a 元素的节点对象由 HTMLAnchorElement 构造函数生成，button 元素的节点对象由 HTMLButtonElement 构造函数生成。因此，元素节点不是一种对象，而是一组对象，这些对象除了继承 Element 的属性和方法，还有各自构造函数的属性和方法。
### (一) 实例属性
#### 1.1 元素特性的相关属性
##### (1) Element.id
>Element.id属性返回指定元素的id属性，该属性可读写。

```javascript
// 注意，id属性的值是大小写敏感，即浏览器能正确识别<div id="app">和<div id="APP">这两个元素的id属性，但是最好不要这样命名。
let app = document.getElementById('app');
console.log(app.id)     // app
```
##### (2) Element.tagName
>Element.tagName属性返回指定元素的大写标签名，与nodeName属性的值相等。

```javascript
let app = document.getElementById('app');
console.log(app.tagName)     // DIV
```
##### (3) Element.lang
>Element.lang属性返回当前元素的语言设置。该属性可读写。

```javascript
// HTML 如下
// <html lang="en">
console.log(document.documentElement.lang)      // en
```

##### (4) Element.title
>Element.title属性用来读写当前元素的 HTML 属性title。该属性通常用来指定，鼠标悬浮时弹出的文字提示框。

#### 1.2 元素状态的相关属性
##### (1) Element.contentEditable，Element.isContentEditable
>HTML 元素可以设置contentEditable属性，使得元素的内容可以编辑。

```javascript
// HTML
// <div contenteditable>hello world</div>
let div = document.querySelector('div');
console.log(div.contentEditable)      // true
```

上面代码中，div 元素有 contenteditable 属性，因此用户可以在网页上编辑这个区块的内容。
Element.contentEditable 属性返回一个字符串，表示是否设置了 contenteditable 属性，有三种可能的值。该属性可写。
1. "true"：元素内容可编辑
2. "false"：元素内容不可编辑
3. "inherit"：元素是否可编辑，继承了父元素的设置

Element.isContentEditable属性返回一个布尔值，同样表示是否设置了contenteditable属性。该属性只读。
#### 1.3 Element.attributes
>Element.attributes属性返回一个类似数组的对象，成员是当前元素节点的所有属性节点。

```javascript
// HTML
// <div id="app"></div>
let div = document.querySelector('div');
let attrs = div.attributes;
for (let i = 0; i < attrs.length; i++) {
  console.log(attrs[i].name + '->' + attrs[i].value)     // id->app
}
```
上面代码遍历 div 元素的所有属性。
#### 1.4 Element.className，Element.classList
>1. className属性用来读写当前元素节点的class属性。它的值是一个字符串，每个class之间用空格分割。
>2. classList属性返回一个类似数组的对象，当前元素节点的每个class就是这个对象的一个成员。

```javascript
// HTML
// <div class="div1 div2 div3"></div>
let div = document.querySelector('div');
console.log(div.className);       // div1 div2 div3
console.log(div.classList)        // {0: 'div1', 1: 'div2', 2: 'div3', length: 3}
```
上面代码中，className属性返回一个空格分隔的字符串，而classList属性指向一个类似数组的对象，该对象的length属性（只读）返回当前元素的class数量。
classList对象有下列方法。
>1. add()：增加一个 class。
>2. remove()：移除一个 class。
>3. contains()：检查当前元素是否包含某个 class。
>4. toggle()：将某个 class 移入或移出当前元素。
>5. item()：返回指定索引位置的 class。
>6. toString()：将 class 的列表转为字符串。

```javascript
div.classList.add('div4');
div.classList.remove('div4');
div.classList.contain('div4');    // false
div.classList.toggle('div4');     // true
div.classList.item(3);            // div4
div.classList.toString()          // div1 div2 div3 div4
```

下面比较一下，className和classList在添加和删除某个 class 时的写法。

```javascript
let app = document.getElementById('app');

// 添加class
app.className += 'bold';
app.classList.add('bold');

// 删除class
app.classList.remove('bold');
app.className = foo.className.replace(/^bold$/, '');
```

>toggle方法可以接受一个布尔值，作为第二个参数。如果为true，则添加该属性；如果为false，则去除该属性。

#### 1.5 Element.dataset
网页元素可以自定义data-属性，用来添加数据。
```html
<div data-timestamp="1522907809292"></div>
```
上面代码中，div 元素有一个自定义的 data-timestamp 属性，用来为该元素添加一个时间戳。
Element.dataset 属性返回一个对象，可以从这个对象读写 data- 属性。
```javascript
// HTML
// <div id="app" data-timestamp="1522907809292" data-index-number="123"></div>
let app = document.getElementById('app');
console.log(app.dataset)    // {indexNumber: '123', timestamp: '1522907809292'}
```

>1. dataset上面的各个属性返回都是字符串。
>2. 开头的data-会省略。
>3. 如果连词线后面跟了一个英文字母，那么连词线会取消，该字母变成大写。
>4. 其他字符不变。

因此，data-abc-def 对应 dataset.abcDef ，data-abc-1 对应 dataset["abc-1"]。
除了使用 dataset 读写 data- 属性，也可以使用 Element.getAttribute() 和 Element.setAttribute()，通过完整的属性名读写这些属性。
```javascript
let myDiv = document.getElementById('mydiv');
myDiv.dataset.foo = 'bar';
myDiv.getAttribute('data-foo')    // bar
```

#### 1.6 Element.innerHTML
Element.innerHTML 属性返回一个字符串，等同于该元素包含的所有 HTML 代码。该属性可读写，常用来设置某个节点的内容。它能改写所有元素节点的内容，包括 HTML 和 body 元素。
如果将innerHTML属性设为空，等于删除所有它包含的所有节点。
```javascript
el.innerHTML = ''
```
上面代码等于将el节点变成了一个空节点，el原来包含的节点被全部删除。
注意，读取属性值的时候，如果文本节点包含&、小于号（<）和大于号（>），innerHTML属性会将它们转为实体形式&amp;、&lt;、&gt;。如果想得到原文，建议使用element.textContent属性。
```javascript
// HTML
// <div>5 > 3</div>
console.log(document.querySelector('div').innerHTML)      // 5 &gt; 3
```
写入的时候，如果插入的文本包含 HTML 标签，会被解析成为节点对象插入 DOM。注意，如果文本之中含有 script 标签，虽然可以生成 script 节点，但是插入的代码不会执行。
#### 1.7 Element.outerHTML
Element.outerHTML属性返回一个字符串，表示当前元素节点的所有 HTML 代码，包括该元素本身和所有子元素。
```javascript
// HTML
// <div id="app"><p>Hello</p></div>
let app = document.getElementById('app');
console.log(app.outerHTML)     // '<div id="app"><p>Hello</p></div>'
```
outerHTML属性是可读写的，对它进行赋值，等于替换掉当前元素。
注意，如果一个节点没有父节点，设置outerHTML属性会报错。
```javascript
// div 是刚刚创建出来的元素，还没有插入到 DOM 中
let div = document.createElement('div');
div.outerHTML = '<div>Hello World</div>'    // DOMException: This element has no parent node.
```
#### 1.8 Element.clientHeight，Element.clientWidth
>1. Element.clientHeight 属性返回一个整数值，表示元素节点的 CSS 高度（单位像素），只对块级元素生效，对于行内元素返回0。如果块级元素没有设置 CSS 高度，则返回实际高度。
>2. 除了元素本身的高度，它还包括padding部分，但是不包括 border、margin。如果有水平滚动条，还要减去水平滚动条的高度。注意，这个值始终是整数，如果是小数会被四舍五入。
>3. Element.clientWidth属性返回元素节点的 CSS 宽度，同样只对块级元素有效，也是只包括元素本身的宽度和padding，如果有垂直滚动条，还要减去垂直滚动条的宽度。
>4. document.documentElement 的 clientHeight 属性，返回当前视口的高度（即浏览器窗口的高度），等同于 window.innerHeight 属性减去水平滚动条的高度（如果有的话）。
>5. document.documentElement 的 clientHeight 属性，返回当前视口的高度（即浏览器窗口的高度），等同于 window.innerHeight 属性减去水平滚动条的高度（如果有的话）。document.body 的高度则是网页的实际高度。一般来说，document.body.clientHeight 大于 document.documentElement.clientHeight。

```javascript
// 视口高度
document.documentElement.clientHeight;

// 网页总高度
document.body.clientHeight
```

#### 1.9 Element.clientLeft，Element.clientTop
Element.clientLeft属性等于元素节点左边框（left border）的宽度（单位像素），不包括左侧的padding和margin。如果没有设置左边框，或者是行内元素（display: inline），该属性返回0。该属性总是返回整数值，如果是小数，会四舍五入。
#### 1.10 Element.scrollHeight，Element.scrollWidth
Element.scrollHeight属性返回一个整数值（小数会四舍五入），表示当前元素的总高度（单位像素），包括溢出容器、当前不可见的部分。它包括padding，但是不包括border、margin以及水平滚动条的高度（如果有水平滚动条的话），还包括伪元素（::before或::after）的高度。
Element.scrollWidth属性表示当前元素的总宽度（单位像素），其他地方都与scrollHeight属性类似。这两个属性只读。
整张网页的总高度可以从document.documentElement或document.body上读取。
```javascript
// 返回网页的总高度
document.documentElement.scrollHeight;
document.body.scrollHeight
```
注意，如果元素节点的内容出现溢出，即使溢出的内容是隐藏的，scrollHeight属性仍然返回元素的总高度。
#### 1.11 Element.scrollLeft，Element.scrollTop
Element.scrollLeft属性表示当前元素的水平滚动条向右侧滚动的像素数量，Element.scrollTop属性表示当前元素的垂直滚动条向下滚动的像素数量。对于那些没有滚动条的网页元素，这两个属性总是等于0。
如果要查看整张网页的水平的和垂直的滚动距离，要从document.documentElement元素上读取。
```javascript
document.documentElement.scrollLeft;
document.documentElement.scrollTop
```
这两个属性都可读写，设置该属性的值，会导致浏览器将当前元素自动滚动到相应的位置。
#### 1.12 Element.offsetParent
Element.offsetParent属性返回最靠近当前元素的、并且 CSS 的position属性不等于static的上层元素。
```html
<div style="position: absolute;">
  <p>
    <span>Hello</span>
  </p>
</div>
```
上面代码中，span元素的offsetParent属性就是div元素。
该属性主要用于确定子元素位置偏移的计算基准，Element.offsetTop和Element.offsetLeft就是offsetParent元素计算的。
如果该元素是不可见的（display属性为none），或者位置是固定的（position属性为fixed），则offsetParent属性返回null。
```html
<div style="position: absolute;">
  <p>
    <span style="display: none;">Hello</span>
  </p>
</div>
```
上面代码中，span元素的offsetParent属性是null。
如果某个元素的所有上层节点的position属性都是static，则Element.offsetParent属性指向<body>元素。
#### 1.13 Element.offsetHeight，Element.offsetWidth
Element.offsetHeight属性返回一个整数，表示元素的 CSS 垂直高度（单位像素），包括元素本身的高度、padding 和 border，以及水平滚动条的高度（如果存在滚动条）。
Element.offsetWidth属性表示元素的 CSS 水平宽度（单位像素），其他都与Element.offsetHeight一致。
这两个属性都是只读属性，只比Element.clientHeight和Element.clientWidth多了边框的高度或宽度。如果元素的 CSS 设为不可见（比如display: none;），则返回0。
#### 1.14 Element.offsetLeft，Element.offsetTop
Element.offsetLeft返回当前元素左上角相对于Element.offsetParent节点的水平位移，Element.offsetTop返回垂直位移，单位为像素。通常，这两个值是指相对于父节点的位移。
下面的代码可以算出元素左上角相对于整张网页的坐标。
```javascript
function getElementPosition(e) {
  let x = 0;
  let y = 0;
  // document.body.offsetParent 为 null
  while (e !== null)  {
    x += e.offsetLeft;
    y += e.offsetTop;
    e = e.offsetParent
  }
  return {x: x, y: y};
}
```
#### 1.15 Element.style
每个元素节点都有style用来读写该元素的行内样式信息。
#### 1.16 Element.children，Element.childElementCount
Element.children属性返回一个类似数组的对象（HTMLCollection实例），包括当前元素节点的所有子元素。如果当前元素没有子元素，则返回的对象包含零个成员。
```javascript
if (para.children.length) {
  let children = para.children;
    for (let i = 0; i < children.length; i++) {
      // ...
    }
}
```
上面代码遍历了para元素的所有子元素。
Element.childElementCount属性返回当前元素节点包含的子元素节点的个数，与Element.children.length的值相同。
#### 1.17 Element.firstElementChild，Element.lastElementChild
Element.firstElementChild属性返回当前元素的第一个元素子节点，Element.lastElementChild返回最后一个元素子节点。
#### 1.18 Element.nextElementSibling，Element.previousElementSibling
Element.nextElementSibling属性返回当前元素节点的后一个同级元素节点，如果没有则返回null。
Element.previousElementSibling属性返回当前元素节点的前一个同级元素节点，如果没有则返回null。
### (二) 实例方法
#### 2.1 属性相关方法
元素节点提供六个方法，用来操作属性。
>1. getAttribute()：读取某个属性的值，返回字符串
>2. getAttributeNames()：返回当前元素的所有属性名，返回数组
>3. setAttribute()：写入属性值，传入要设置的属性和属性值，无返回值
>4. hasAttribute()：某个属性是否存在，传入要检测的属性，返回 true 或 false
>5. hasAttributes()：当前元素是否有属性，不需要传参，返回 true 或 false
>6. removeAttribute()：删除属性，无返回值

#### 2.2 Element.querySelector()
Element.querySelector方法接受**CSS 选择器**作为参数，返回父元素的**第一个匹配的子元素**。如果没有找到匹配的子元素，就返回null。
>1. Element.querySelector方法可以接受任何复杂的 CSS 选择器。
>2. 这个方法无法选中伪元素。
>3. 它可以接受多个选择器，它们之间使用逗号分隔。

```javascript
let el1 = document.body.querySelector('p');
let el2 = document.body.querySelector('.box');
let el3 = document.body.querySelector('#box');
// 返回第一个div或p元素。（注意：选择多个也只返回一个）
let el4 = document.body.querySelector('div, p');
let el5 = document.body.querySelector("style[type='text/css'], style:not([type])")
```
#### 2.3 Element.querySelectorAll()
Element.querySelectorAll方法接受 CSS 选择器作为参数，返回一个NodeList实例，包含所有匹配的子元素。
```javascript
let el = document.querySelector('#test');
let matches = el.querySelectorAll('div.highlighted > p')
```
该方法的执行机制与querySelector方法相同，也是先在全局范围内查找，再过滤出当前元素的子元素。因此，选择器实际上针对整个文档的。
它也可以接受多个 CSS 选择器，它们之间使用逗号分隔。如果选择器里面有伪元素的选择器，则总是返回一个空的NodeList实例。
#### 2.4 Element.getElementsByClassName()
Element.getElementsByClassName方法返回一个HTMLCollection实例，成员是当前元素节点的所有具有指定 class 的子元素节点。该方法与document.getElementsByClassName方法的用法类似，只是搜索范围不是整个文档，而是当前元素节点。
```javascript
element.getElementsByClassName('red test')
```
注意，该方法的参数大小写敏感。
#### 2.5 Element.getElementsByTagName()
Element.getElementsByTagName方法返回一个HTMLCollection实例，成员是当前节点的所有匹配指定标签名的子元素节点。该方法与document.getElementsByClassName方法的用法类似，只是搜索范围不是整个文档，而是当前元素节点。
```javascript
let table = document.getElementById('forecast-table');
let cells = table.getElementsByTagName('td')
```
#### 2.6 事件相关方法
以下三个方法与Element节点的事件相关。这些方法都继承自EventTarget接口。
>1. Element.addEventListener()：添加事件的回调函数
>2. Element.removeEventListener()：移除事件监听函数
>3. Element.dispatchEvent()：触发事件
#### 2.7 Element.getBoundingClientRect()
Element.getBoundingClientRect方法返回一个对象，提供当前元素节点的大小、位置等信息，基本上就是 CSS 盒状模型的所有信息。
```javascript
let rect = obj.getBoundingClientRect();
```
上面代码中，getBoundingClientRect方法返回的rect对象，具有以下属性（全部为只读）。
>1. x：元素左上角相对于视口的横坐标
>2. y：元素左上角相对于视口的纵坐标
>3. height：元素高度
>4. width：元素宽度
>5. left：元素左上角相对于视口的横坐标，与x属性相等
>6. right：元素右边界相对于视口的横坐标（等于x + width）
>7. top：元素顶部相对于视口的纵坐标，与y属性相等
>8. bottom：元素底部相对于视口的纵坐标（等于y + height）

由于元素相对于视口（viewport）的位置，会随着页面滚动变化，因此表示位置的四个属性值，都不是固定不变的。如果想得到绝对位置，可以将left属性加上window.scrollX，top属性加上window.scrollY。
注意，getBoundingClientRect方法的所有属性，都把边框（border属性）算作元素的一部分。也就是说，都是从边框外缘的各个点来计算。因此，width和height包括了元素本身 + padding + border。
另外，上面的这些属性，都是继承自原型的属性，Object.keys会返回一个空数组，这一点也需要注意。
```javascript
let rect = document.body.getBoundingClientRect();
Object.keys(rect) // []
```
上面代码中，rect对象没有自身属性，而Object.keys方法只返回对象自身的属性，所以返回了一个空数组。
#### 2.8 Element.remove()
Element.remove方法继承自 ChildNode 接口，用于将当前元素节点从它的父节点移除。
```javascript
let el = document.getElementById('mydiv');
el.remove();
```
#### 2.9 Element.focus()，Element.blur()
Element.focus方法用于将当前页面的焦点，转移到指定元素上。
Element.blur方法用于将焦点从当前元素移除。
#### 2.10 Element.click()
Element.click方法用于在当前元素上模拟一次鼠标点击，相当于触发了click事件。