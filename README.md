# clear-concept

## CSS

描述：`vertical-align`有时候不起作用

`vertical-align`主要起作用的是`inline-block`，而对`block`属性不起作用，但是受影响的元素涉及到`inline`属性，`inline`水平元素受`vertical-align`属性而位置改变不是因为其对`vertical-align`属性敏感或者起作用，而是受制于整个line box的变化而不得不变化的。

`vertical-align`默认的属性为`baseline`，这个可以想象一下英语书写时的四条线，从上到下第三条为`baseline`，而这些属性起作用是依靠父元素的，因此需要在子元素里面编写这些属性。

参考地址：[张鑫旭的blog](https://www.zhangxinxu.com/wordpress/2010/05/%E6%88%91%E5%AF%B9css-vertical-align%E7%9A%84%E4%B8%80%E4%BA%9B%E7%90%86%E8%A7%A3%E4%B8%8E%E8%AE%A4%E8%AF%86%EF%BC%88%E4%B8%80%EF%BC%89/)

## Vue

### 描述：Vue环境没报错，也能正常显示数据，但是浏览器控制台会报错。

**解决**：Vue里面的数据不能超过两级；如果超过两级需要在父元素判断一下，通过判断二级JSON是否存在数据解决。

举例：

```diff 
- <span v-if="seller.supports">{{ seller.supports[0].description }}</span>
+ <span v-if="seller.supports">{{ seller.supports[0].description }}</span>
```

### 描述：在最开始的文件内最外层`template`中使用`v-if`和`v-else`始终渲染`v-else`部分

举例：

```html
<template v-if="ok">
  <p>hello</p>
</template>
<template v-else>
  <p>world</p>
</template>

<script>
  export default {
    data() {
      return {
        ok: true
      }
    }
  }
</script>
```

### 描述：Vue传props时数据为JSON应该标记为Object

举例：

```html
<template v-if="ok">
  <p>hello</p>
</template>
<template v-else>
  <p>world</p>
</template>

<script>
  export default {
    props: {
      // error data: JSON
      data: Object
    }
  }
</script>
```

## VS Code

### 描述：解决 VS Code 中 vue 文件格式化时单引号变双引号的问题

**解决**：在`setting.json`中配置如下

```js
"vetur.format.defaultFormatterOptions": {
  "js-beautify-html": {
      "wrap_attributes": "force-expand-multiline"
  },
  "prettyhtml": {
      "printWidth": 100,
      "singleQuote": false,
      "wrapAttributes": false,
      "sortAttributes": false
  },
  // start here
  "prettier": {
      // "semi": false,   --semicolon
      "singleQuote": true
  }
  // end here
}
```
