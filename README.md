# clear-concept

## HTML
`object`标签用于在HTML加入Flash文件

## CSS
- `direction:rtl;`文本方向, 在块级元素起作用, 在设置`inline-block`, 同时文字不在一行排列时也会起作用
- `text-indent`规定了文本块中首行文本的缩进，一般进行首行缩进使用`text-indent: 2em;`
- `height: 100vh;`一般用于高度设置为一屏幕的高度
- `width: 100vw;`一般用于宽度设置为一屏幕的宽度

使文本在一行显示，超出父元素隐藏，并且加`...`

```css
.element {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

### 关于文字之间的间距问题
1. letter-spacing
定义了关于字母之间的间距
```css
letter-spacing: normal | 2px | 0.1em;
```

2. word-spacing
定义了关于单词之间的间距
```css
word-spacing: normal | 2px | 0.1em;
```

3. white-space
定义了关于标签内空白处表现方式
```css
white-space: normal; // 不超出宽度，自动换行，如果单词不能一行显示，那么该单词就会整个换行
white-space: nowrap; // 单词不换行，一行显示
white-space: pre; // 保留空格和换行符，但不能自动换行
white-space: pre-wrap; // 保留空格和换行符，并且自动换行
white-space: pre-line; // 保留换行符和自动换行，但一行内的空格会被忽略
```

4. word-break
定义了关于单词在换行时是否拆分
```css
word-break: normal; // 就是一个单词超出后不换行，但接着会换行
word-break: keep-all; // 该行一律不换行
word-braak: break-all; // 超出后一律拆分
```

5. word-wrap
定义了关于单词超过一行时是否拆分
```css
word-wrap: normal; // 不拆分
word-wrap: break-word; // 这个区别于break-all，这个是之前超出不会换行，但是后面单词超出就会换行
```
