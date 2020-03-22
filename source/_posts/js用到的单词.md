---
uuid: cf745db9-f49e-7928-e983-ec9527f46ebd
title: js用到的单词
date: 2020-03-22 21:29:38
tags: jsES5
category: 学习整理的前端
---
document-->文档	element-->元素	node-->节点	
### 获取元素	根据	介绍	
.getElementById("btn")	根据id获取这个标签(元素)	从文档中找到id值为btn的这个标签(元素)，document.getElementById("id属性的值");======>返回的是一个元素对象	
.getElementsByTagName("p");	.getElementsByTagName("标签的名字");返回的是一个伪数组,	无论获取的是一个标签,还是多个标签,最终都是在数组中存储的,这行代码的返回值就是一个数组	
.getElementsByClassName("cls");	根据类样式的名字来获取元素，返回的是一个伪数组,	无论获取的是一个标签,还是多个标签,最终都是在数组中存储的,这行代码的返回值就是一个数组	
.querySelectorAll(".cls")	根据选择器的方式获取元素		
-------	
### 事件			
.onclick	      注册鼠标点击事件		
.onmouseover	  注册鼠标进入事件		
.onmouseout	    注册鼠标离开事件		
.onfocus	      注册获取焦点的事件		
.onblur	        注册失去焦点的事件		
.onkeydow     	注册键盘按下事件		
.onkeyup	      注册键盘抬起事件		
.onload	        页面加载完毕,这个事件就会触发		
.onmousemove	  注册鼠标拖动事件	clientX，clientY，pageX，pageY   坐标轴	
.onscroll	      注册滚动事件		
.onmousedown	  注册鼠标按下		
.onmouseup	    注册鼠标抬起		
document.onkeydown = function(e){console.log(e.keyCode);}	查看键盘按下的是哪个键，对应着数字		
-----
对象.addEventListener("事件类型",事件处理函数,false)	为元素绑定事件谷歌和火狐支持,IE8不支持		
my$("btn").removeEventListener("click",f1,false);	解绑事件的时候,需要在绑定事件的时候,使用命名函数		
对象.attachEvent("有on的事件类型",事件处理函数)	谷歌不支持,火狐不支持,IE8支持		
my$("btn").detachEvent("onclick",f1);	解绑事件的时候,需要在绑定事件的时候,使用命名函数	
-----
### DOM方法			
.innerText	innerHTML才是真正的获取标签中间的所有内容		
.textContent	设置标签中的文本内容		
.innerText	凡是成对的标签,中间的文本内容,设置的时候,都使用innerText这个属性的方式	document.getElementById("p1").innerText="这是一个p"	
.style.width 	凡是css中这个属性是多个单词的写法,在js代码中DOM操作的时候.把-干掉,后面的单词的首字母大写即可		
.className="cls"	在js代码中DOM操作的时候,设置元素的类样式,不用class关键字,应该使用,className		
.removeAttribute("class")	删除calss		
.classList.add("类名称")	添加css类		
.classList.remove("类名称")	删除css类		

##  offset系列			
元素.offsetLeft（宽高上下左右）	样式在style标签中获取方法	:父级元素margin+父级元素padding+父级元素的border+自己的margin	
.offsetWidth:	获取元素的宽		
.offsetHeight:	获取元素的高		
.offsetLeft:	获取元素距离左边位置的值		
.offsetTop:	获取元素距离上面位置的值

##  scroll系列			
.scrollWidth	元素中内容的实际的宽		
.scrollHeight	元素中内容的实际的高		
.scrollTop	向上卷曲出去的距离		
.scrollLeft	向左卷曲出去的距离	

##  client系列	可视区域		
.clientHeight	可视区域的高(没有边框),边框内部的高度		
.clientWidth	可视区域的宽(没有边框),边框内部的宽度		
.clientLeft	左边边框的宽度		
.clientTop	上面的边框的宽度		
.clientX	可视区域的横坐标（单机的位置距离X轴开始位置的坐标）		
.clientY	可视区域的纵坐标（单机的位置距离Y轴开始位置的坐标）		
------
### 节点属性
getAttribute("score")	在html标签中添加的自定义属性,如果想要获取这个属性的值,需要使用getAttribute("自定义属性的名字")才能获取这个属性的值	获取自定义属性的值	
setAttribute("属性的名字","属性的值")	设置自定义属性:setAttribute("属性的名字","属性的值");	设置自定义属性	
removeAttribute("属性的名字")	移除自定义属性:removeAttribute("属性的名字")	移除自定义属性	
nodeType:	节点的类型:1----标签,2---属性,3---文本		
nodeName	节点的名字:标签节点---大写的标签名字,属性节点---小写的属性名字,文本节点----#text		
nodeValue	节点的值:标签节点---null,属性节点---属性值,文本节点---文本内容		
parentNode	父级节点		
parentElement	父级元素		
childNodes	子节点		
children	子元素		
firstChild	第一个子节点		
firstElementChild	第一个子元素		
lastChild	最后一个子节点	IE8中是第一个子元素	
lastElementChild	最后一个子元素	IE8中不支持	
previousSibling	某个元素的前一个兄弟节点	IE8中是第一个子元素	
previousElementSibling	某个元素的前一个兄弟元素	IE8中不支持	
nextSibling	某个元素的后一个兄弟节点		
nextElementSibling	某个元素的后一个兄弟元素
-----
.write("标签的代码及内容")	创建元素,缺陷:如果是在页面加载完毕后,此时通过这种方式创建元素,那么页面上存在的所有的内容全部被干掉		
对象.innerHTML="标签及代码"	对象.innerHTML="标签代码及内容"		
document.createElement("标签的名字")	document.createElement("标签名字");对象	"//把创建后的子元素追加到父级元素中
    父级对象.appendChild(pObj);"	
父级元素.appendChild(子级元素对象)	追加元素		
父级元素.inerstBefore(新的子级对象,参照的子级对象)	把新的子元素插入到参考子元素的前面		
父级元素.removeChild(要干掉的子级元素对象)	移除父级元素中子级元素		
getComputedStyle	获取任意一个元素的任意一个样式属性的值	"兼容代码：function getStyle(element,attr) {
    //判断浏览器是否支持这个方法
   return window.getComputedStyle? window.getComputedStyle(element,null)[attr]:element.currentStyle[attr];
  }"	使用方法： console.log(getStyle(my$("dv"),"top"))
getComputedStyle(elenent,null)	传入一个对象和一个空值		
### 功能			
##  排他功能			
阻止a跳转	阻止超链接的默认的跳转:return false		
------
### location对象			
console.log(window.location.hash)	地址栏上#及后面的内容		
console.log(window.location.host)	主机名及端口号		
console.log(window.location.hostname)	主机名		
console.log(window.location.pathname)	文件的路径---相对路径		
console.log(window.location.port)	端口号		
console.log(window.location.protocol)	协议		
console.log(window.location.search)	搜索的内容		
location.href="http://www.jd.com"	设置跳转的页面的地址		
location.assign("http://www.jd.com")	设置跳转的页面的地址		
location.reload()	重新加载--刷新		
location.replace("http://www.jd.com")	没有历史记录		
------	
### userAgent			
console.log(window.navigator.userAgent)	 通过userAgent可以判断用户浏览器的类型		
console.log(window.navigator.platform)	通过platform可以判断浏览器所在的系统平台类型.		
-----	
### client系列	可视区域		
.clientHeight	可视区域的高(没有边框),边框内部的高度		
.clientWidth	可视区域的宽(没有边框),边框内部的宽度		
.clientLeft	左边边框的宽度		
.clientTop	上面的边框的宽度		
-----
### 定时器			
var timeId = setInterval(function () {}, 1000);	"参数1:函数
参数2:时间---毫秒---1000毫秒--1秒
执行过程:页面加载完毕后,过了1秒,执行一次函数的代码,又过了1秒再执行函数.....
返回值就是定时器的id值"		
clearInterval(timeId);	参数:要清理的定时的id的值		
window.setTimeout(函数,时间);	另一个定时器-------一次性的定时		
clearTimeout(timeId);	参数:要清理的定时的id的值		
-----
### 事件冒泡			
控制:addEventListener中的第三个参数是控制事件阶段的，	事件触发过程中，会出现事件冒泡效果，如何阻止冒泡		
获取:e.eventPhase 这个属性可以知道这个属性是当前的那个阶段，	window.enent.cancelBubble = true;//谷歌，ie支持，火狐不支持		
1是捕获阶段 从外向内	事件处理函数中的参数（e）.stopPropagation()//阻止事件冒泡//ie不支持，这是火狐的标准		
2是目标阶段 唯一的	事件参数e 在ie的浏览器不存在可以使用 window.enent来代替		
3是冒泡阶段 从里向外，一般是冒泡阶段			
------
兼容代码			
-------
鼠标移动的时候文字不被选中			
window.setSelection().removeAllRanges()||docnment.selection.empty()			
-----
知道三个值可根据比例来的出第四个值			
公式			
x/y = a/b   移项  ay = bx   求y = bx/a			
-------
不占空间的原型prototype	使用方法：自定义对象名称.prototype.自定义方法名称 = function(){}   或者一个值		
创建实例化对象    prototype使用这个创建里面的this认清	使用，举例子		
" function 实例化对象名（首字母大写）(btnObj,dvObj,json) {
        this.btnObj = btnObj;
        this.dvObj = dvObj;
        this.json = json;
    }
    实例化对象名.prototype.init = function(){
        var  than = this;
        this.btnObj.onclick = function(){
            for(var key in json){
                than.dvObj.style[key] = than.json[key];
        };
        };
    };
"	" var json = {
        ""width"":""100px"",
        ""height"":""500px"",
        ""backgroundColor"":""yellow"",
        ""border"":""1px solid red""
    }
    var sc = new Person(my$(""btn""),my$(""dv""),json);
    sc.init();"		
------
####  重点			
function(){this}.bind(than)//函数中的this就是bind中的than（变量对象）			
比如定时器中的this是window，然后在函数中.bind(构造函数顶级this的变量名)，定时器中的this就是bind出来的变量名，bind改变了里面this的指向			
-----
### 函数中的继承问题     Proson.call(this,父级形参)没有继承方法	构造函数.call（当前对象this）		
function Person(name){this.name = name}			
function Student(name,exe){proson.call(this,name);this.exe = exe;}			
var stu = new Student(“小明”,10)			
------
### 改变this的指向     当前对象就是想让这个this指向谁    调用的时候改变this的指向			
函数名/方法名.prototype/方法.call(当前对象,值，值...)	f1.call(当前对象,值，值...)		
函数名/方法名.prototype/方法.apply(当前对象,[值，值，值，..])	f1.apply(当前对象,[值，值，值，..])		
改变this的指向，使用方法			
函数名.apply(对象,[参数1，参数2，参数3.。。。。])；			
函数名.call(对象,参数1,参数2,......)			
我想要使用其他对象的某个方法，其他对象.方法名.apply（当前对象，参数1.....）/call(当前对象，参数......)			
这个方法就会被当前的对象所用，同时这个方法中的this就是当前对象，在调用方法的时候改变了this的指向			
-----
复制一份的时候，把参数传入到函数中，第一个参数默认为window    可以穿this   是复制的时候改变了this的指向			
.bind();这个方法是复制的意思，参数可以在复制的时候传进去，也可以在复制之后调用的时候传进去			
-----
### 函数内的几个属性			
函数.name   返回值是函数的名字			
函数名.arguments   返回值是一个伪数组			
函数名.length   返回形参的个数			
函数名.caller   返回有谁调用了这个函数			