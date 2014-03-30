##JavaScript的insertAdjacentHTML方法  
今天看别人的代码发现一个insertAdjacentHTML方法，对其不了解，于是google了一下，发现它的作用类似之前常用的innerHTML和innerText，但是插入的对象不同，innerHTML和innerText是在节点内部插入，而insertAdjacentHTML则是根据参数的不同选择插入位置，具体用法如下：  
<!--more-->

####该方法的解释：  
insertAdjacentHTML()解析指定的文本，HTML或XML，并插入到DOM树中的节点在指定的位置。它不会重新解析正在使用的元素，因此它不会破坏现有的元素里面的元素。这方面的工作，并避免额外的步骤系列化，让它变得更快，不直接innerHTML操作.  

####语法：  
element.insertAdjacentHTML(position, html);

其中position可以取值

beforebegin在 element 元素的前面；

afterbegin在 element 元素的第一个子元素前面；

beforeend在 element 元素的最后一个子元素后面；

afterend在 element 元素的后面。

html表示要插入的内容，是字符串被解析成 HTML 或 XML 插入到 DOM 树中。  

####示例：  
首先在不执行该方法时dom结构如下   
<pre>
//html
&lt;div id="obj"&gt;hello world&lt;/div&gt;
</pre>  
如下图，可以清楚的看到DOM结构：  
<img src="http://holdjs.sinaapp.com/wp-content/themes/v6/images/2.png" width="480px" >
接下来执行insertAdjacentHTML方法  
<pre>
//javascript
var obj = document.getElementById('obj');
obj.insertAdjacentHTML('beforebegin','<span>insert</span>');
</pre>  
然后用开发者工具可以发现DOM结构已经发生变化，如下图：  
<img src="http://holdjs.sinaapp.com/wp-content/themes/v6/images/3.png" width="480px">  
综上，可以大致了解insertAdjacentHTML方法的使用，另外afterbegin、beforeend、afterend这三个参数就不做详述了。