#wip #frontend #css #布局 
# Block box， Inline box

1. Block和inline改变的是盒子的外部显示类型，也就是其和布局中其他元素在布局上的显示方式。
2. 好比说flex则是改变盒子的内部显示类型，也就是会接管内部元素的一些布局，盒子类型等。
3. Block box 会占据父容器在内联方向上的所有宽度，而且对外部显示的控制能力较高（width, height可以发挥作用，内外边距和边框会推开别人）
4. inline box不会占据所有宽度，不会换行，width, height不起作用，边距和边框只能推开水平方向上的别人

# BFC（Block Formatting Context)

# 常见问题

## 外边距(margin)折叠

## 内联盒子无法撑开高度

# 参考
https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model
[BFC](https://juejin.cn/post/7031065166317879310)
[\*FC](https://github.com/chokcoco/iCSS/issues/56)
