##获取touchmove过程中手指所在的DOM元素  
最近以three开始的着一系列游戏火爆了，我想来自己也山寨一个不同玩法的试试，不试不知道，一试吓一跳，发现了一个之前不知道的问题。  
具体问题是这样：在鼠标或者手指移动过程中，获取鼠标或者手指所在的DOM元素，如下图所示：  
<img src="http://holdjs.sinaapp.com/wp-content/themes/v6/images/4.png" width="" height="">  

可是在实现过程中，发现一个问题，当用鼠标事件处理时，mouseover过程中，能通过event.target很方面的获取到当前鼠标滑过的DOM，但是采用touch事件时，当touchstart获取到event.target后，只要touch没有结束，touchmove过程中获得的DOM对象永远是touchstart时获取到的DOM。后来查资料重新认识了下touchstart/touchmove/touchend：  

1.  touchstart：手指放在一个DOM元素上；  
2.  touchmove：手指拖曳一个DOM元素；
3. touchend：手指从一个DOM元素上移开。 

从上面可以看出，从touchstart到touchmove再到touchend，event.target始终是一个DOM元素，与mouseover有很大的区别。再通过event.target无法实现效果时，我尝试完全用touch所移动的坐标与DOM元素的坐标交叉范围来判断当前手指所在DOM元素，这样虽然实现了效果，但是总觉得不理想，略微复杂，总觉得一定有其它方式可以实现，然后到@司徒正美大大的群里问了下，果不其然，司徒说用document.elementFromPoint，这玩意之前从未用过，google了下，发现如下  
#####语法： 
oElement = document.elementFromPoint(iX,iY)

#####参数： 
iX :　 必选项。整数(Integer)。单位：象素(Pixel)。定位横坐标偏移量。
iY :　 必选项。整数(Integer)。单位：象素(Pixel)。定位纵坐标偏移量。

#####返回值：oElement :　 
对象(Element)。返回获取的对象的引用。

于是乎，通过document.elementFromPoint完美实现我所需要的效果，更重要的是实现简单。