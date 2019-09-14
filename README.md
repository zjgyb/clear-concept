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

#### key

在大部分的情况下是与`v-for`中配合使用的，目的是为了标识插入的数据在一条数据变更的时候不至于全部更新，然而有时候你需要重新渲染某一个组价，那就需要用到key了

我喜欢使用`Math.random()`去改变key值

#### watch

一开始是不会去查看用户的改变的，只有数据改变时才查看数据的改变，这里是想要介绍如何某些值如何监听

Props
+ 如果是String类型的，则直接和一般的监听一样
+ 如果是Object类型的，则需要加引号的方式去监听，例如：
```js
export default {
  watch: {
    'test.num'(newValue, oldValue) {
      ...
    }
  }
}
```

#### model
绑定元素，这里讲的用途是用于组件之间的通信
```
<parent-element 
  v-model="val"
/>

<!--
  等同于
<parent-element 
  v-on:value="val"
  @input="e => val = e.target.value"
/>
 -->
export default {
  data() {
    return {
      val: ''
    }
  }
}
```

```
<!-- child-element -->
<div>
  <input :value="value" @input="handleVal" />
</div>

export default {
  props: {
    value: String
  },
  methods: {
    handleVal(e) {
      this.$emit('input', e.target.value);
    }
  }
}
```

Vuex
+ 如果想监听Vuex也是可以的，例如：
```js
export default {
  watch: {
    '$store.state.num'() {
      ...
    }
  }
}
```

## Vue CLI 3

**Q:** CSS中scoped作用

**A:** scoped 其实就加上一个 hash，使 css 变量能够只对所在文件起作用

---
**Q:** Preset的作用

**A:** 保存一个项目的目录结构，它的文件格式是`~/.vuerc`，方便他人开发时使用这个目录结构

---
**Q:** 如何进行单个vue文件的开发预览

**A:** 
1. 先使用命令`npm install -g @vue/cli-service-global`
2. 浏览器预览`vue serve [filename]`
---
**Q:** 如何查看vue-cli中webpack配置

**A:** 使用命令`vue inspect`

---

[官网地址](https://cli.vuejs.org/zh/guide/)

## 各种工具包的配置

### marked在vue中的使用——与markdown相关

先`yarn add marked`或者`npm install marked`

然后使用，单纯的装换
```html
<template>
  <div v-html="compiledMarkdown"></div>
</template>
<script>
  import marked from 'marked';

  export default {
    data() {
      return {
        input: '## hello'
      }
    },
    computed: {
      compiledMarkdown() {
        return marked(this.input, { sanitize: true });
      }
    },
  }
</script>
```

输入转换

```html
<template>
  <div class="editor">
    <textarea :value="input"></textarea>
    <div v-html="compiledMarkdown"></div>
  </div>
</template>
<script>
  import marked from 'marked';

  export default {
    data() {
      return {
        input: '## hello\n ## world'
      }
    },
    computed: {
      compiledMarkdown() {
        return marked(this.input, { sanitize: true });
      }
    },
  }
</script>
```
[传送门](https://github.com/markedjs/marked)

## tips

### preview image files
```html
<template>
  <div class="box">
    <div class="img-preview" v-if="imageData.length > 0">
      <img class="preview" :src="imageData">
    </div>
    <input type="file" @change="getImgData" accept="image/*">
  </div>
</template>
export default {
  data() {
    return {
      imageData: ''
    };
  },
  methods: {
    getImgData(event) {
      const input = event.target;
      if (input.files && input.files[0]) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.imageData = e.target.result;
        };
        reader.readAsDataURL(input.files[0]);
      }
    }
  }
};
```
[参考来源](https://gist.github.com/zmts/9cfdba90897db1615b6e9fda6069a124)

### 动态改变Vue Component(动态组件)
app.vue
```html
<template>
  <div id="app">
    <component :is="view"></component>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
      view: this.$store.state.page
    }
  },
  mounted() {
    this.$root.$on('update', this.update);
  },
  methods: {
    update() {
      this.view = this.$store.state.page;
    }
  }
}
</script>
```
其他文件
```html
<script>
export default {
  methods: {
    changeComponent() {
      // 这里使用了vuex进行状态管理
      // this.$store.state.page = '...'; 直接改变Vuex不是很好，应该交给Vuex里面的mutations的函数
      this.$store.commit({
        type: 'changePage', // 函数名
        page: 'hello-world' // 改变的页面名称，注意名字的书写方式
      });
      this.$root.$emit('update');
    }
  }
}
</script>
```

### 父子元素通信

子组件

```html
<template>
  <div @click="changeSomething">some message</div>
</template>
<script>
  // childComponent
  export default {
    data() {
      return {
        param1: 'test'
      }
    },
    methods: {
      changeSomething() {
        this.$emit('changeSomethingInParent', this.param1);
      }
    }
  }
</script>
```

父组件
```html
<template>
  <div>
    <child-component @changeSomethingInParent="changeData" />
  </div>
</template>
<script>
  // parentComponent
  import ChildComponent from './documentToChildComponent.vue';
  
  export default {
    components: {
      ChildComponent
    },
    data() {
      return {
        data: ''
      }
    },
    methods: {
      changeData(param1) {
        this.data = param1;
      }
    }
  }
</script>
```

### 在vue中引入全局自定义CSS
```js
// vue.config.js
module.exports = {
  ...
  css: {
    loaderOptions: {
      sass: {
        data: `@import "@/style/_global.scss";`
      }
    }
   }
  ...
}
```
[参考地址](https://css-tricks.com/how-to-import-a-sass-file-into-every-vue-component-in-an-app/#article-header-id-1)

## Vue I18n问题记录

复数问题: 只能取到前三个参数，不能取到第四个(three apples)及以后参数，在Github里的issue里有讨论

```js
const messages = {
  en: {
    car: 'car | cars',
    apple: 'no apples | one apple | two apples | three apples'
  }
}
```
