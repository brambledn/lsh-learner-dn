##jquery学习篇－－遍历
###1.  $( ).add( )
事例：    

    $("ul").css("border","1px solid #bcbcbc")
           .add("p")
           .css("color","#7f7ddd");

效果图：
<img src="./屏幕快照 2016-04-08 下午12.07.13.png" width = "250" alt="图片描述" align=center />

**分析**：可以看出所有的`ul`元素都有灰色的边框以及紫色的文字，而`p`标签只是有紫色的文字，`add`的效果便是遍历`ul`，加上遍历`p`标签，而在`add p`标签之前的操作并不会作用到`p`上，主要用于想要遍历两个有共同操作的元素，但两者应该有类似于包含关系（即作用于后者的操作需要是前者同样需要的操作）。
*注：文档解释是 将元素添加到匹配元素的集合中，注意并不是将元素插入到dom中，只是多加一个匹配元素*

用法：

	.add('.new-block')     //add(selector)
	.add('p')              //add(elements)
	.add($('.new-block'))  //add(jQueryObject)
	.add('<p>from</p>')     //add(html) html片段,并不是直接匹配html，而是创建新元素，可用于后期插入
	.add(selector,content) //前者是选择器，后者是上下文


###2.   $( ).andSelf( )
事例： 

	$('div').children("ul").andSelf().css("border","1px solid red");

效果图：
<img src="./屏幕快照 2016-04-08 下午3.52.24.png" width = "250" alt="图片描述" align=center />

**分析**：`andSelf`主要是用于用上下文查找时，操作其它元素的同时，附带操作自身。如在`div`中搜索`ul`后，给`ul`设上边框，使用`andSelf`,会给`div`也设上边框。
**注意事项**: `andSelf`需要在操作前面，否则不起作用，如放在操作css前。

###3.   $( ).children( )
事例：
	
	//html
	<ul class="ul1">
      <li>旅游
         <ul class="ul2">
           <li>dd</li>
           <li>44</li>
        </ul>
      </li>
      <li>学习
      </li>
      <li>生活
      </li>
    </ul>
	//js
	$('.ul1').children('li').css('border','1px solid red');
       
效果图：
<img src="./屏幕快照 2016-04-08 下午5.03.11.png" width = "250" alt="图片描述" align=center />
**分析**:  可以看出例子中children只是遍历了`.ul1`下的子元素，而没有遍历其子孙`li`元素，因此`$().children()`，只是**遍历一层**，并返回

###4.  $( ).closest( )
事例：
		
	//html
	<ul class="ul1">
      <li>旅游
         <ul class="ul2">
           <li>dd</li>
           <li>44</li>
        </ul>
      </li>
      <li>学习
      </li>
      <li>生活
      </li>
    </ul>
	//js
	var $close = $('li').closest('ul');
    console.log($close);
结果：
      会返回两个`ul`，一个`.ul2`，一个`.ul2`。
**分析**：可以看出`closest`会返回所有匹配元素对应的最近的父元素。并且从匹配元素开始查找。

###5.  $( ).contents( )
事例：
    
    console.log($('.example').contents());
    
运行结果（console输出）：

  <img src="./屏幕快照 2016-04-13 下午3.21.51.png" width = "250" alt="图片描述" align=center />

**分析**: 在输出结果中可以看到`text`与`comment`节点，`contents()`的作用是返回匹配元素中的所有内容包括文本节点和注释节点。
###6.  $( ).each(function(index, element){      } )
事例：
       
    //html
    <div class="div">
	    全部
	    <!--ddddd-->
	    <ul>
	      <li>
	        第一
	      </li>
	      <li>
	        第二
	      </li>
	      <li>
	        第三
	      </li>
	      <li>
	        第四
	      </li>
	    </ul>
     </div>  
    //js
    $('li').each(function(i,item){
       $(item).append('<p>');
    });
    console.log($('.div').html());

   运行结果：（可在控制台看到） 
   
   <img src="./屏幕快照 2016-04-12 下午6.11.07.png" width = "250" alt="图片描述" align=center />

   **分析**：.each( ) 方法接受函数类型的参数，是循环匹配元素执行函数。格式如：`$( ).each(function(index, element){      } )`，*需要注意的是参数函数内的参数`element`是元素，因此需要将`element`转成`jquery`对象再进行`dom`操作*。
 
  **扩展用法**：  (来自司机端h5用法)
         事例：
        
	var obj = [{
	  name:"dd",
	  sex:"2",
	},
	  {
	    name:"ww",
	    sex:"1",
	  }];
	console.log(obj);
	console.log($(obj));
	$(obj).each(function(i,item){
	   console.log(item);
	});

运行结果（console中）：

<img src="./屏幕快照 2016-04-12 下午6.28.03.png" width = "250" alt="图片描述" align=center />

 分析：可以看出json对象转成jquery对象也可循环执行函数。*思考：`$()`到底处理了什么，`obj`与 `$(obj)`  *，（司机端中更新商品清单使用此种方法）
###7.  $( ).end( )
事例：
	
	   //html
	   <div class="example">
	    全部
	    <!--ddddd-->
	    <ul>
	      <li>
	        第一
	      </li>
	      <li class="no2">
	        第二
	      </li>
	      <li>
	        第三
	      </li>
	      <li>
	        第四
	      </li>
	    </ul>
	    <div class="example2">
	    </div>
	  </div>
	  
	  //js
	  $('li').closest('ul')
	         .find('.no2').css('border','1px solid red')
	         .end().css('border','1px solid green');
          
运行效果： 

<img src="./屏幕快照 2016-04-13 下午2.53.19.png" width = "250" alt="图片描述" align=center />

**分析**：开始时匹配出`.no2`,给它加上红色边框，后使用`end`，再给某个元素加上绿色边框，从效果图可以看到加上绿色边框的是`ul`；即`end`的作用是删除上一次匹配，则相当于去除`.find('.no2').css('border','1px solid red').end()`代码，于是会给`ul`加上绿色边框。*注意点：是删除最近一次匹配！！！*

###8. $( ).eq( )
事例：
      
      //html  与上事例相同
      //js
      $('li').eq(-3).css('color','red');
运行效果：
      
<img src="./屏幕快照 2016-04-13 下午3.36.37.png" width = "250" alt="图片描述" align=center />

**分析**：`eq（index）`是遍历匹配元素，并返回相应index的元素。当index为正值时，从正序开始算起；若为负值，则从倒序算起；事例中为－3，在从后往前第三个元素。

###9. $( ).filter( )
事例：
		 
    //html 与上一事例相同    
	//js-1
	$('li').filter("[data-id='12']")
	       .css("color","red");
	//js-2	
	$('li').filter(function(){
	  return $(this).data('id')=="12";
	}).css('border','1px solid green');

运行效果：

<img src="./屏幕快照 2016-04-13 下午3.56.32.png" width = "250" alt="图片描述" align=center />

**分析**： `js-1``js-2`两段代码实现相同，也是filter的两种用法－－filter(selector),filter(function)

###10. $( ).find( )
事例：

	$('ul').find('li').css('border','1px solid red');

运行效果：
		
<img src="./屏幕快照 2016-04-13 下午4.44.13.png" width = "250" alt="图片描述" align=center />

**分析**：`find()`匹配元素集合中每个元素的后代，由选择器进行筛选。*注意是后代，包括子元素和孙子元素一直往深层遍历*


###11. $( ).first( )/`$().last()`
事例：
       
    $('ul').find('li').first().css('border','1px solid red');
    $('ul').find('li').last().css('border','1px solid red');
**分析**：`.first()`和`.last()`可以说是`eq()`的特殊情况，分别是匹配元素的第一个元素／最后一个元素

###12. $( ).has( )
事例：  

    $('ul').children('li').has('p').css('background-color','green');

运行效果：

<img src="./屏幕快照 2016-04-13 下午5.09.28.png" width = "250" alt="图片描述" align=center />

###13. $( ).is( )
事例：

	console.log($('ul').children('li').is('.no1'));

运行结果：
		
		true
     
###14. $( ).map( )
事例：

	//html
	<div class="example">
    全部
	    <!--ddddd-->
	    <ul>
	      <li data-id = '1' class="no1">
	        第一
	        <p>是吗？</p>
	      </li>
	      <li data-id= '2' class="no2">
	        第二
	      </li>
	      <li data-id= '3'>
	        第三
	      </li>
	      <li data-id= '4'>
	        第四
	      </li>
	    </ul>
	    <div class="example2">
	    </div>
	</div>
	 //js
	$('ul').children('li').map(function(){
	  return $(this).data('id');
	}).each(function(i,item){
	  console.log(item);
	});
运行结果：

<img src="./屏幕快照 2016-04-13 下午5.24.15.png" width = "350" alt="图片描述" align=center />
**分析**：把每个元素通过函数传递到当前匹配集合中，并生成新的jquery对象。如事例，将`each`与`map`结合可逐个返回每个元素的`data-id`属性。
###15. $( ).next( )/`$().nextAll()` `$().nextUntil()`
事例及运行结果：


----------


		
	<div class="example">
	    全部
	    <!--ddddd-->
	    <ul>
	      <li data-id = '1' class="no1">
	        第一
	        <p>是吗？</p>
	      </li>
	      <li data-id= '2' class="no2">
	        第二
	      </li>
	      <li data-id= '3' class="no3">
	        第三
	      </li>
	      <li data-id= '4' class="no4">
	        第四
	      </li>
	    </ul>
	    <div class="example2">
	    </div>
	  </div>
	$('ul').children('li.no2').next().css('color','green');
<img src="./屏幕快照 2016-04-13 下午5.36.23.png" width = "350" alt="图片描述" align=center /> 
	


----------


	$('ul').children('li').next().css('color','green');//请勿使用
<img src="./屏幕快照 2016-04-13 下午5.37.51.png" width = "350" alt="图片描述" align=center /> 


----------


	$('ul').children('li.no1').nextAll('.no3').css('color','green');
<img src="./屏幕快照 2016-04-13 下午5.37.11.png" width = "350" alt="图片描述" align=center /> 

----------


	$('ul').children('li.no1').nextAll('.no3').css('color','green');
<img src="./屏幕快照 2016-04-13 下午5.44.56.png" width = "350" alt="图片描述" align=center /> 

----------


		
	$('ul').children('li.no1').nextUntil('.no4').css('color','green');
<img src="./屏幕快照 2016-04-13 下午5.46.33.png" width = "350" alt="图片描述" align=center /> 

**分析**:返回匹配元素的同级元素的下一个。

###16. $( ).not( )
事例：
	
	$('ul').children('li').not('.no4').css('color','green');
运行效果：
<img src="./屏幕快照 2016-04-13 下午5.52.36.png" width = "350" alt="图片描述" align=center /> 
**分析**：将匹配元素从匹配集合中删除，返回去除匹配元素后的集合。
###17. $( ).offsetParent( )
事例：
		
	<div class="example" style="position:relative">
    全部
	    <!--ddddd-->
	    <ul style="position: absolute">
	      <li data-id = '1' class="no1" >
	        第一
	        <p>是吗？</p>
	      </li>
	      <li data-id= '2' class="no2">
	        第二
	      </li>
	      <li data-id= '3' class="no3">
	        第三
	      </li>
	      <li data-id= '4' class="no4">
	        第四
	      </li>
	    </ul>
	    <div class="example2">
	    </div>
	  </div>
	$('li').offsetParent().css('border','1px solid green');
运行结果：
<img src="./屏幕快照 2016-04-13 下午5.58.35.png" width = "350" alt="图片描述" align=center />

**分析**：返回带有定位的第一个父元素

###18. $( ).parent( )/`$().parents()` `$().parentsUntil()`
事例及运行效果：

----------

	$('li').parent().css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.02.18.png" width = "350" alt="图片描述" align=center />

----------


	$('li').parents().css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.01.41.png" width = "350" alt="图片描述" align=center />

----------
		
	$('li').parentsUntil('.example').css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.02.18.png" width = "350" alt="图片描述" align=center />

**分析**：获得父元素，其中`parent()` 获取匹配元素的父元素；`parents()`获取所有祖先元素，即一直向外遍历，可通过选择器筛选，如不筛选则会获得所有祖先元素包括`html``body`标签

###19. $( ).prev( )/`$().prevAll()` `$().prevUntil()`
事例及运行效果：
		
	$('li.no2').prev().css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.06.45.png" width = "350" alt="图片描述" align=center />


----------
		
	$('li.no3').prevAll().css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.08.14.png" width = "350" alt="图片描述" align=center />


----------

	$('li.no3').prevUntil('.no1').css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.10.36.png" width = "350" alt="图片描述" align=center />
**注意：直到匹配到特定元素，则不包括起始元素(`.no3`)，也不包括结束元素（`.no1`）**
**分析**：`prev`是匹配元素的兄弟元素的上一个。
###20. $( ).siblings( )
事例：
		
	$('li.no3').siblings().css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.14.42.png" width = "350" alt="图片描述" align=center />

**分析**：返回除`.no3`外的兄弟元素

----------

	$('li.no3').siblings('.no2').css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.15.57.png" width = "350" alt="图片描述" align=center />

**分析**：返回类为`.no2`的`.no3`的兄弟元素
###21. $( ).slice( ) 
	
	$('li').slice(0,3).css('border','1px solid green');
<img src="./屏幕快照 2016-04-13 下午6.20.27.png" width = "350" alt="图片描述" align=center />
**分析**：`.slice( )`匹配元素集合缩减为指定的指数范围的子集，第一个值为起始index，第二个值为结束index，其中不包括结束index；当只有一个值时，则这个值为起始index，从此位置到最后。













