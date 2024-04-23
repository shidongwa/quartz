---
parent: "[[Fleeting MOC]]"
tags:
  - 🪴weedy
  - dailyNotes
  - "#camelCase"
  - "#kebab-cased"
  - "#PascalCase"
date: 
draft: false
---
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
camelCase属性key

```html
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

 kebab-cased属性 key
 
```html
<div :style="{ 'font-size': fontSize + 'px' }"></div>
```

PascalCase

`<PascalCase />` 在模板中更明显地表明了这是一个 Vue 组件，而不是原生 HTML 元素。同时也能够将 Vue 组件和自定义元素 (web components) 区分开来。[^1]

[^1]: https://cn.vuejs.org/guide/components/registration.html


>[!Tips]
> 不同属性名声明方式比较重要。原因是 Javascrpt 和 Html 中对变量大小写，声明方式不同。