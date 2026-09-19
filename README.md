## 个人主页

整体结构采用Bootstrap中的grid样式，并且为了减小页面大小没有使用对应的JS库和JQuery。其中，第一栏采用响应式布局的方式，根据窗口的大小动态调整布局位置。

### 页面制作中的问题记录
- 打印时：1）勾选背景图形，否则打不出背景色；2）使用另存为PDF，使用Acrobat PDF会导致无法复制；3）Acrobat PDF可将PDF转Word;

- Bootstrap隐藏元素：使用`.d-none`（全部隐藏）或 `.d-{sm,md,lg,xl}-none`。Bootstrap显示元素：使用`.d-block`（全部显示）或`.d-{sm,md,lg,xl}-block`。如`.d-none .d-md-block d-xl-none`将隐藏除了中型、大型设备以外的所有屏幕中的元素。

- 自适应布局在打印页面时会出现和`html`展示不同的排版，因为A4的大小需要填充满，目前考虑通过调整`.container`样式中的字体大小来调整排版，但很难调整到和`html`页布局完全一致。

- `<a href='#'></a>`在打印时会多出下划线，通过调整其对应的样式`a:link {text-decoration:none;}`、`a:active: text-decoration:none;}`、`a:visited {text-decoration:none;}`、`a:hover {text-decoration:none;}`均无果，因此采用`<span>`标签替换`<a>`标签，用CSS和JS模拟`<a>`标签的效果。
```css
.alink {
    color: #007bff;
    font-size: 10px;
    display: inline-block;
    vertical-align: middle;
}
.alink:hover {
    text-decoration: underline;
    color: #f60;
    cursor: pointer;
}
```
```javascript
<a id="a-node" style="display: none;" href="#" target="_blank">用于模拟a标签跳转</a>
//重写原生a标签的点击事件，因为原生a标签打印时带下划线
var els = document.getElementsByClassName('alink');// console.log(els);
Array.prototype.forEach.call(els, function (el) {
    el.onclick = function () {
        // window.open(el.getAttribute('data-href'), '_blank');
        var link = el.getAttribute('data-href');
        var ael = document.getElementById('a-node');
        ael.setAttribute("href", link);
        ael.click();
    };
});
```

- 打印时生效的CSS
```css
@media print {
    .no-print {
        /* 隐藏不需要在打印版本中显示的元素 */
        display: none !important;
    }
    .container {
        /* 自适应布局通过字号控制页面排版 */
        font-size: 16px;
    }
}
```
