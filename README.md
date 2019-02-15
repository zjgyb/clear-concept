# clear-concept

## Vue(基础知识)

### 模板语法

#### 文本
使用`{{ }}`插入数据
```html
<span>Message {{ msg }}</span>
```

#### 参数
当属性内为变量时，则需要进行绑定

```html
<a v-bind:href="url">...</a>
<!-- 或者 -->
<a :href="url">...</a>
```

```html
<a v-on:click="doSomething">...</a>
<!-- 或者 -->
<a @click="doSomething">...</a>
```

#### 计算属性缓存VS方法
计算属性缓存：`computed`
方法：`method`
计算属性的值是缓存的，因此计算属性在直接需要渲染时比方法更加合适，计算属性不会影响其他渲染函数。

#### 对象语法
直接样式书写
```html html
<div :style="styleObject"></div>
```

```js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

#### slot

* 在父元素中定义的slot可以把里面的元素增加到子元素的最后部分
* 如果父元素没有在slot中增加内容，则显示子元素中slot的内容

[参考地址](https://alligator.io/vuejs/component-slots/)

## Vue CLI 3

**Q:** CSS中scoped作用
**A:** scoped 其实就加上一个 hash，使 css 变量能够只对所在文件起作用
