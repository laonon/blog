##JavaScript的作用域
昨天在微博看到知乎一个帖子[你在编程时见过最愚蠢的 Bug 是什么？](http://www.zhihu.com/question/23301215/answer/24195171?utm_source=weibo&utm_medium=weibo_share&utm_content=share_answer&utm_campaign=share_button) ,其中有段js代码，如下：  
<pre>
var number = 10;
var showNumber = function () {
  alert(number);
}
 
(function () {
  number = 20;
  showNumber();
})();
</pre>  
猜猜会输出什么？
<!--more-->  
答案是10  
为什么呢？为什么不是20,按照我们对作用域的理解，这里应该是20啊，那么继续看下面的代码  
<pre>
var number = 10;
var showNumber = function () {
  alert(number);
};
 
(function () {
  number = 20;
  showNumber();
})();
</pre>
这里输出又是什么呢？没错，就是20
这下是不是更迷惑了，你看出这两段代码的不同之处了吗？如果你仔细看会发现，下面的代码比上面的代码多了一个分号，就只有一个分号就得到我们想要的结果。  
大家都知道js很“神奇”，这里的神奇之处在于，你不加分号，浏览器解析成下面的匿名函数作为参数传递给赋值给showNumber的function,而function这里被执行了，因此showNumber的赋值是function的返回值，因为没有返回值，默认返回undefined。那么按照js的作用域函数内部没有number，到上一级查找，alert的当然就是10了。  
js类似的问题很多，仔细去审视代码，良好的代码习惯都是应该一个合格的工程师所具备的。