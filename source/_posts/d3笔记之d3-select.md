---
title: d3笔记之d3-selection
date: 2016-07-08 16:16:22
tags:
  - javascript
  - d3
  - 数据可视化
categories: javascript
toc: true
---
Selections 允许强大的数据驱动文档对象模型 (DOM): 设置 attributes, styles, properties, HTML 或 text 内容等等。使用 data join 的 enter 和 exit 选择集可以用来根据具体的数据 add 或 remove 元素。

#### Selecting Elements
``` javascript
d3.select("body")
  .append("svg")
    .attr("width", 960)
    .attr("height", 500)
  .append("g")
    .attr("transform", "translate(20,20)")
  .append("rect")
    .attr("width", 920)
    .attr("height", 460);
```

扩展选择集原型链，比如为 checkbox 添加一个 check 方法
``` javascript
d3.selection.prototype.checked = function(value) {
  return arguments.length < 1
      ? this.property("checked")
      : this.property("checked", !!value);
};
// use
d3.selectAll("input[type=checkbox]").checked(true);
```
+ ##### d3.select(selector)
 `(selector?: String|NodeList|d3.selection|Function)`
```javascript
// element selector
d3.select("a");

// NodeList
d3.selectAll(document.links).style("color", "red");
```
 如果 selector 为函数，则会在选择前执行对应的函数，并且会传递当前元素的关联数据 (d)，当前的索引 (i) 以及当前分组 (nodes)，在函数中 this 指向当前 DOM 元素(nodes[i]). 为函数时必须返回一个元素，或者 null。例如选择每个 p 元素的前一个同胞节点:
```javascript
// function
var previous = d3.selectAll("p").select(function() {
  return this.previousElementSibling;
})
```
 如果 selector 为 null 则返回的选择集中的每个元素都为 null，也就是产生一个空选择集。
+ ##### selection.filter(filter) 
 `(filter: String|Function)`
 
 过滤选择集并返回一个新的过滤后的选择集
``` javascript
// 过滤出表格行中的偶数行
// 等价于 var even = d3.selectAll("tr:nth-child(even)");
var even = d3.selectAll("tr").filter(":nth-child(even)");

// 用函数实现
var even = d3.selectAll("tr").filter(function(d, i) { return i & 1; });

var even = d3.selectAll("tr").select(function(d, i) { return i & 1 ? this : null; });
```

+ ##### selection.merge(other) 
 返回一个将当前选择集和指定的 other 选择集合并之后的新的选择集。
 ```javascript
var circle = svg.selectAll("circle").data(data) // UPDATE
    .style("fill", "blue");

circle.exit().remove(); // EXIT

circle = circle.enter().append("circle") // ENTER
    .style("fill", "green")
  .merge(circle) // ENTER + UPDATE
    .style("stroke", "black");
```

****

#### Modifying Elements
在选中元素之后，使用选择集的转换方法来修改文档的内容。
+ ##### selection.attr(name[, value]) 
```javascript
d3.select("a").attr("name", "fred")
```

+ ##### selection.classed(names[, value]) 
如果指定了 value 则通过设置 class 属性或者修改 classList 来为选择集中的元素指定或取消指定名称为 names 的类名。
```javascript
selection.classed("foo bar", true);
```
 如果 value 为真，则表示选择集中的所有元素都会被添加指定的类名。否则则指定的类会被移除。如果 value 为函数，则会为选择集中的每个元素调用，并传递当前元素绑定的数据 d, 索引 i 以及当前的分组 nodes。
```javascript
selection.classed("foo", function() { return Math.random() > 0.5; });
```

+ ##### selection.property(name[, value]) 
有些 HTML 元素的属性比较特殊，不能直接使用 attr 和 style 操作，比如文本域的 value 属性以及 checkbox 的 checked 属性。使用本方法可以操作这些属性。
```javascript
d3.selectAll("input[type=checkbox]").property("checked", true);
```

+ ##### selection.text([value]) 
如果指定了 value 则将选中的元素的 text content 设置为指定的值，会替代任何现有的子元素。

+ ##### selection.html([value]) 
如果指定了 value 则将选中的元素的 inner HTML 设置为指定的值，会替代任何现有的子元素。

+ ##### selection.append(type) 
创建一个指定标签名的元素，并将其追加到选择集元素列表中。
```javascript
d3.selectAll("p").append("div");
// 等价于:
d3.selectAll("p").append(function() {
  return document.createElement("div");
});
// 等价于:
d3.selectAll("p").select(function() {
  return this.appendChild(document.createElement("div"));
});
```
 无论是 `type` 是字符串还是返回 DOM 元素的函数，都会返回一个新的包含被添加元素的选择集。每个新的元素都会继承当前元素的数据(如果有的话)。

+ ##### selection.insert(type[, before]) 
为选择集中每个选中的插入一个指定类型(标签名)的元素，插入的位置为第一个匹配 before 选择条件的元素。
```javascript
d3.selectAll("p").insert("div");
// 等价于:
d3.selectAll("p").insert(function() {
  return document.createElement("div");
});
// 等价于:
d3.selectAll("p").select(function() {
  return this.insertBefore(document.createElement("div"), null);
});
```
 在上述两种例子中，都会返回一个被插入元素的新的选择集。每个新的元素都会继承当前元素的数据(如果有的话)。

+ ##### selection.remove() 
从当前文档中移除选中的元素。返回的选择集(被移除的元素)已经与文档脱离。

+ ##### selection.clone([deep]) 
在所选元素之后插入所选元素的克隆，并返回包含新添加的克隆元素的选择集。如果 deep 为真则选中元素的后代元素也会被克隆(深度克隆)。否则仅仅克隆所选元素自身。

+ ##### selection.sort(compare)
返回一个新选择集，其中包含了当前选择集中所有元素的经过 compare 函数排序之后的副本。在排序后，将排序后的元素重新插入原来的文档中替换未排序的元素。

+ ##### selection.order()
重新将元素插入到文档中以使得文档中每个分组的次序与选择集的次序匹配。如果数据已经有序的话，这个方法与 selection.sort 等效，但是要更快。。

+ ##### selection.raise()
按序重新插入每个选中的元素，每次插入的元素都作为其父元素的最后一个子元素。等价于:
```javascript
selection.each(function() {
  this.parentNode.appendChild(this);
});
```

+ ##### selection.lower()
按序重新插入每个选中的元素，每次插入的元素都作为其父元素的第一个子元素。等价于:
```javascript
selection.each(function() {
  this.parentNode.insertBefore(this, this.parentNode.firstChild);
});
```

+ ##### selection.sort(compare)
返回一个新选择集，其中包含了当前选择集中所有元素的经过 compare 函数排序之后的副本。在排序后，将排序后的元素重新插入原来的文档中替换未排序的元素。

****

#### Joining Data
+ ##### selection.data([data[, key]]) 
将指定数组的数据 data 与已经选中的元素进行绑定并返回一个新的选择集，返回的新的选择集使用 update 表示: 此时数据已经成功的与元素绑定。并且定义了 enter 和 exit 方法用来返回需要加入元素和移除元素的选择集。data 可以是任意数据类型的数组(e.g., 数值或对象), 可以是一个返回数组的方法(比如为每个分组继续绑定数组时). 当数据分配给元素时，会被存储在元素的 `__data__` 属性上, 因此可以在重新选中元素时继续使用与元素对应的数据。

 data 会被指定给选择集中的 each group(每个分组)。如果选择集中包含多个分组(比如 d3.selectAll 后跟随 selection.selectAll)，则 data 应该应该被指定为一个函数。

 例如根据如下数值矩阵创建一个 HTML 表格:
```javascript
var matrix = [
  [11975,  5871, 8916, 2868],
  [ 1951, 10048, 2060, 6171],
  [ 8010, 16145, 8090, 8045],
  [ 1013,   990,  940, 6907]
];

var tr = d3.select("body")
  .append("table")
  .selectAll("tr")
  .data(matrix)
  .enter().append("tr");

var td = tr.selectAll("td")
  .data(function(d) { return d; })
  .enter().append("td")
    .text(function(d) { return d; });
```

+ ##### selection.enter()
返回 enter 选择集: 没有对应 DOM 节点的数据的占位节点。
```javascript
var div = d3.select("body")
  .selectAll("div")
  .data([4, 8, 15, 16, 23, 42])
  .enter().append("div")
    .text(function(d) { return d; });
```

+ ##### selection.exit()
返回 exit 选择集: 没有对应数据的已经存在的 DOM 节点。exit 选择集通常用来移除多余的元素。
```javascript
// 使用新的数据更新 DIV 元素
div = div.data([1, 2, 4, 8, 16, 32], function(d) { return d; });
// 通过 enter 选择集为 [1, 2, 32] 添加新的元素
div.enter().append("div").text(function(d) { return d; });
// 移除与 [15, 23, 42] 绑定的元素:
div.exit().remove();
```

+ ##### selection.datum([value])
获取或设置每个选中元素上绑定的数据。与 selection.data 不同, 这个方法不会进行数据链接计算并且不影响索引, 不影响 enter 和 exit 选择集。
```html
<ul id="list">
  <li data-username="shawnbot">Shawn Allen</li>
  <li data-username="mbostock">Mike Bostock</li>
</ul>
```
 通过将每个元素绑定的数据设置为其 dataset 属性
```javascript
selection.datum(function() { return this.dataset; })
```