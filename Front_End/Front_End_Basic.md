# Front_End

## HTML

1. 显示代码原有格式 类似模板字符串功能

   - 使用 `<pre></pre>` 标签

2. link 与 script 的区别

   - link 标签：链入一个文档，通过 rel 属性申明链入的文档与当前文档之间的关系
   - script 标签:
     1. `innerHTML` 永远不会显示在界面上
     2. 如果 `type` 不等于 `text/javascript` 的话，内部的内容不会作为 `Javascript` 执行

3. 自定义 radio CheckBox file...

```html
<style>
  #checkbox {
    display: none;
  }
  i::after {
    content: '☆';
  }
  #checkbox:checked + i::after {
    content: '★';
  }
</style>
<body>
  <label>
    <input type="checkbox" name="checkbox" id="checkbox" />
    <i style="font-size: normal"></i>
  </label>
</body>
```

4. 行内块级元素出现空格问题的解决 `font-size: 0`

### FORM 额外属性

1. novalidate

   - 取消 h5 自动校验功能（form 属性）因为浏览器自带的校验功能不友好而且与界面不兼容

2. autocomplete

   - 表单自动完成功能开启与禁止

   - on / off

3. autofocus

   - 页面加载即获取焦点

4. accept

   - text 属性带有 file 的标签的限制文件属性

   - 有两种值:

     1. MIME Type => ex: `audio/*`
     2. 文件扩展名 => ex : `.lrc`

5. multiple

   - 让一个文件域多选。

6. 文件域设置

   - `method='post'`

   - `enctype='multipart/form-data'`

7. input 表单内文字样式

```css
input::-webkit-input-placeholder {
  color: #182f62;
}
input::-moz-placeholder {
  color: #182f62;
}
input:-moz-placeholder {
  color: #182f62;
}
input:-ms-input-placeholder {
  color: #182f62;
}
```

8. 复选框：checkbox

   - 默认值： 'on'

   - 设置 value 提交，value 属性；

   - 多选提交的话是在 name 属性后加[]

9. 单选框：radio

   - 将多个 radio 的 name 属性设置成相同的，形成相同的组

   - 将 value 设置成不同的，在服务端辨别选则的是哪个

10. SELECT

    - 如果 option 有 value 提交 value 如果没 value 提交 innerhtml、innertext

#### 表单提交 ajax

```js
$('form').on('submit', function(e) {
  $.ajax({
    url: $(this).attr('action'),
    type: $(this).attr('method'),
    data: $(this).serialize(),
    success: function(data) {
      console.log(data)
    }
  })
  return false // 阻止本身提交行为
})
```

## FLEX

### 方向：

- flex-direction

  - 默认值： row 水平向左

  - 常用值： column 垂直向下

  - 其他值：
    - row-reverse 水平向左
    - column-reverse 垂直向下

### 宽度均分

- flex: 1;

### 换行：

- flex-wrap

  - 默认值： nowrap 不换行，若超出则等比缩小（ flex-shrink=1 ）
  - 常用值： wrap 换行，第一行在上方。
  - 其他值： wrap-reverse 换行，第一行在下方。

### 对齐：

- justify-content

  - 默认值： flex-start 左对齐

  - 常用值：

    - flex-end 右对齐
    - center 居中
    - space-between 两端对齐

  - 其他值： space-around 两侧等距

- align-items

  - 默认值： flex-start 上对齐

  - 常用值：

    - flex-end 下对齐
    - center 居中

  - 其他值：
    - baseline 文字基线对其
    - stretch 占满容器

## JavaScript

### JS Basic

1. `return` or `break` or `continue`

   - `return`: 直接跳出函数 => 程度最大, 不执行后续

   - `break`: 直接跳出循环 => 程度一般, 不执行后续(若有循环嵌套，只能跳出一层，需要逐层 break)

   - `continue`: 终止单次循环，不能跳出循环 => 程度最小, 执行后续循环

2. `attr` or `prop`

   - `attr`: 获取的是 html 元素上的属性（即实际写在元素上的属性---elements 所监视到的属性）

   - `prop`: 获取的是 DOM 属性（获取的是 DOM 抽象出来的属性值，与 attr 的值部分有区别）

> DOM 会将 html 元素进行封装，而且会抽象出来做一些额外功能

3. 访问自定义属性 `data-...`

   - DOM: `.dataset['id']`

   - jQuery:
     - `.attr('data-id')`
     - `.data('id')`

4. 逻辑运算符优先级： `! > && > ||`

5. 逻辑运算符说明：

   - `||` 或的关系，如果前面的值等于 false 就会自动执行第二个值

   - `&&` 且的关系，如果前面的值等于 true 就会自动执行第二个值

6. 正则

   - 创建

     - `var rec = /^[a-zA-Z]@\.$/`

   - 验证

     - rec.test('str') => 找到返回 `boolean`

### API

1. typeof operand

   - operand 一个表示对象或原始值的表达式
   - 返回 其类型

2. eval(String)

   - 将传入的字符串当做 JavaScript 代码进行执行

3. charAt(0)

   - 返回指定位置的字符

### JQuery

1. 入口函数

   - 文档结构加载完成（不包含图片等非文字媒体文件）后，对 DOM 进行操作

   - 即 ：在“加载 js 和 css”和“加载图片等其他信息”之间，就可以操作 Dom

   - 一个页面响应加载的顺序是：域名解析-加载 html-加载 js 和 css-加载图片等其他信息。

   - onload : 页面包含图片等文件在内的所有元素都加载完成。

```javascript
// 入口函数 （jq ready()的简写）
$(function($) {
  // 作用
  // 1. 单独作用域
  // 2. 确保页面加载过后执行
})

// 即等价于（ jQuery的默认参数是：“document” ）
$().ready(function() {
  //do something
})
```

2. contains :用来查找的一个文本字符串。这是区分大小写的。

```html
<!-- `$( ":contains(text)" )`  -->
<div>John Resig</div>
<div>George Martin</div>
<div>Malcom John Sinclair</div>
<div>J. Ohn</div>
<script>
  $("div:contains('John')").css('text-decoration', 'underline')
</script>
```

3. 动画相关

```js
// fadeIn() / fadeOut()有function（）参数
// 指的是在动画完成时执行的函数。
// fadeIn() / fadeOut 先检测样式，所以可以先设置display：flex属性，行内隐藏。

// Example:
$('.avatar').fadeOut(function() {
  $(this)
    .on('load', function() {
      $(this).fadeIn()
    })
    .attr('src', res)
})

// img的onload事件指的是图片完全加载完以后执行 （异步操作）先注册后设置 src
```

### JSON

```js
es5中有JSON对象（内置对象）

	JSON.parse(str)    		==>  将json字符串转为数组

	JSON.stringify(arr)		==>  将数组转换为json格式的字符串

```

### 网页中的跳转方式：

1. 标签跳转

   - 使用 `a` 标签的形式进行跳转

2. 编程式导航

   使用 `window.location.href` 的形式跳转

## 备忘工具

1. 富文本编辑器

   - ueditor

   - CKEditor

   - tinymce

   - markdown
