##JavaScript Event Loop 浅析
今天微博上出现[@吴多益](http://weibo.com/nwind)的一个帖子，原帖是这样的:  
>话说这些年来我最喜欢问的面试题是『从输入 URL 到页面加载完的过程中都发生了什么事情？』，个人觉得这道题非常非常难，既考深度又考广度，至今还没有人的回答让我满意，所以我想自己挑战一下，整理出一篇文章，有哪位大牛也想来一起挑战么？  
<!--more-->

然后微博被纷纷转载，其中看到[@貘吃馍香](http://weibo.com/itapir)的说了句：了解 event loop 后这些全无压力。尽管搞清楚了event loop还不一定搞得清楚吴多益那道题，但是学习js搞清楚 event loop是必须的。于是乎，我google相关资料，发现一篇文章写得比较易懂[《JavaScript Event Loop 浅析》](http://heroicyang.com/2012/08/28/javascript-event-loop/),另外还有一篇参考文章[How JavaScript Timers Work](http://ejohn.org/blog/how-javascript-timers-work/)  
###原文是这样
####问题场景
先看一段代码，看看执行结果与预期目标是否一致：  
<pre>
//html
&lt;a href="#" id="doBtn"&gt;do something&lt;/a&gt;
&lt;div id="status"&gt;&lt;/div&gt;  
//javascript
void function() {
  var doBtn = document.getElementById('doBtn')
    , status = document.getElementById('status');
  doBtn.onclick = function(e) {
    e.preventDefault();
    status.innerText = 'doing...please wait...';  // 开始啦
    sleep(10000);  // 模拟一个耗时较长的计算过程，10s
    status.innerText = 'done';  // 完成啦
  };
}();  
function sleep(ms) {
  var start = new Date();
  while (new Date() - start <= ms) {}
}
</pre>  
上面代码主要想完成一个功能：按钮被点击时———> 显示一个状态告知用户正在干一些事情———> 开始干———> 事情干完后状态变更为已完成。

看上去没问题，应该是可以工作的，于是在浏览器运行这个页面。可是现实总是残忍的，没有符合预期效果。当点击按钮之后，浏览器就冻结了，用于显示状态的div并没有显示，界面上也没有“doing…”这个提示；经过10s之后，浏览器回过神了，代表耗时较长的计算已经结束，此时用于显示状态的div显示“done”。  

究其原因：JavaScript 引擎是单线程的。而此时还有必要再了解下浏览器内核都有哪些主要的常驻线程，才能解上面的疑惑。浏览器内核常驻线程大致包含以下：  
1. 浏览器 GUI 渲染线程    
2. JavaScript 引擎线程  
3. 浏览器定时触发器线程  
4. 浏览器事件触发线程  
5. 浏览器 http 异步请求线程  

而 GUI 渲染线程和 JavaScript 引擎线程是互斥的，JavaScript 执行时 GUI 渲染线程是挂起的，页面将停止一切的解析和渲染行为。上面的 3、4、5 类线程也会产生不同的异步事件。看下面这张图就应该比较直观了。
  
<img src="http://img.heroicyang.com/js-event-loop.png" width="500px">

因为 JavaScript 引擎是单线程的，所以代码都是先压到队列，然后由引擎采用先进先出的方式运行。事件处理函数、timer 执行函数也会排到这个队列中，然后利用一个无穷回圈，不断从队头取出函数执行，这个就是Event Loop。  

接下来还是继续用图来说明上面的代码为什么没有达到预期效果。

<img src="http://img.heroicyang.com/js-event-loop-1.png" width="500px">  

于是结果就只看到了”done”。  

####怎样解决？

使用setTimeout()，下面是修改后的onclick事件处理函数：
<pre>
doBtn.onclick = function(e) {
  e.preventDefault();  
  status.innerText = 'doing...please wait...';  // 开始啦  
  setTimeout(function() {
    sleep(10000);  // 模拟一个耗时较长的计算过程，10s
    status.innerText = 'done';  // 完成啦
  }, 0);  // 0ms delay
};
</pre>  
为什么这样就解决了呢？还是用上面的队列的图来解释。
<img src="http://img.heroicyang.com/js-event-loop-2.png" width="500px">  

看完是不是豁然开朗，但是还是有几个疑问：  




















