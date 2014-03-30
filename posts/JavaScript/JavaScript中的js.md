##JavaScript中的this解析  

在写js的过程中我们经常会用到this关键字，对于初学者来说经常混淆而用错，得不到想要的结果而疑惑，因为js的语言特性使得this与其它语言不太一样，今天我们就分析下this的表现有哪些形式,本文前提是在浏览器中运行。  
####1.在全局范围使用this，会指向window  
<pre>
console.log(this);//[object Window]即window
</pre>
<!--more-->
#####2.普通函数调用，this指向window
<pre>
function test(){
    console.log(this);//window
}
</pre>
####3.方法调用，this指向调用方法的对象
<pre>
function Class(){
}
Class.prototype.test = function(){
    console.log(this);
}
var mm = new Class();
mm.test();//console.log(mm);
</pre>
####4.方法赋值表达式，this指向window
<pre>
function Class(){}
Class.prototype.test = function(){
    console.log(this);
}
var mm = new Class();
var me = mm.test;
me();//console.log(window) 
</pre>  
其实不难理解，在当mm.test赋值给me,对于me()的执行，就相当于普通函数的执行，因而指向window.  
####5.call和apply中的this  
<pre>
function foo(a, b, c) {
    console.log(this);
}
var bar = {};
foo.apply(bar, [1, 2, 3]); // 输出Object {} 即bar
foo.call(bar, 1, 2, 3); // 输出Object {}
</pre>
*当使用 Function.prototype 上的 call 或者 apply 方法时，函数内的 this 将会被 显式设置为函数调用的第一个参数。因此函数调用的规则在上例中已经不适用了，在foo 函数内 this 被设置成了 bar。  
看完前面这几种情况，我们再来看下面的例子:
<pre>
var Class = {};
Class.test = function(){
    console.log(this);//会输出什么呢？
    function inner(){
        console.log(this);//这又会输出什么呢？
    }
    inner();
}
Class.test();
</pre>
看上面的代码，你觉得第一个this会输出什么？第二个this又会输出什么？如下图所示，这是在chrome下看到的结果：  
<img src="http://holdjs.sinaapp.com/wp-content/themes/v6/images/pic20130908.png" width="100%">  
前面提到的四种情况，作为方法调用的函数中的this指向调用方法的对象，所以常见的错误就是会以为inner函数中this也会和test的this一样指向Class，实际上指向了window，为什么出现这种情况？怎样才能让this正确指向我们设想的对象？  
<pre>
var Class = {};
Class.test = function(){
    var self = this;
    function inner(){
        console.log(self);//输出 Object {test: function}
 
    }
    inner();
}
Class.test();
</pre>  
对上面的代码分析可以看到，我们只是做了一点小的改动，在test内部创建局部变量self把this赋值给self,然后利用self指向了我们需要的目标。