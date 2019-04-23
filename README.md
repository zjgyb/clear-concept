# clear-concept

## HTML
`object`标签用于在HTML加入Flash文件

## CSS
- `direction:rtl;`文本方向, 在块级元素起作用, 在设置`inline-block`, 同时文字不在一行排列时也会起作用
- `text-indent`规定了文本块中首行文本的缩进，一般进行首行缩进使用`text-indent: 2em;`
- 'height: 100vh;'一般用于高度设置为一屏幕的高度

使文本在一行显示，超出父元素隐藏，并且加`...`

```css
.element {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```
