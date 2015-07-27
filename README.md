#imagesLoaded.js

----------

> imagesLoaded是一款用于检测页面中的图片是否被加载的js插件。imagesLoaded是非常有用的插件，当你的页面中某幅图片没有被加载时，默认会显示一个红叉或图片alt文本，imagesLoaded可以将未加载的图片替换为你设置的图片。

> http://www.htmleaf.com/jQuery/Image-Effects/201503141520.html

> http://imagesloaded.desandro.com/


##使用方法
	
	var imagesLoaded = require('imagesLoaded');

	imagesLoaded( elem, callback )
	// you can use `new` if you like
	new imagesLoaded( elem, callback )   
	

- elem：该参数是元素，节点列表，数组或选择器字符串。
- callback：图片被加载后的回调事件。

使用callback回调函数和绑定always事件是一样的（看事件描述一节内容）。

	// 作为元素
	imagesLoaded( document.querySelector('#container'), function( instance ) {
	  console.log('all images are loaded');
	});
	// 作为选择器字符串
	imagesLoaded( '#container', function() {...});
	// 多个元素
	var posts = document.querySelectorAll('.post');
	imagesLoaded( posts, function() {...});  


##事件
	
imagesLoaded是一个事件发射源，你可以为事件绑定事件监听。

	var imgLoad = imagesLoaded( elem );
	function onAlways( instance ) {
	  console.log('all images are loaded');
	}
	// bind with .on()
	imgLoad.on( 'always', onAlways );
	// unbind with .off()
	imgLoad.off( 'always', onAlways );   


###always

	imgLoad.on( 'always', function( instance ) {
	  console.log('ALWAYS - all images have been loaded');
	});  

> 在所有图片被加载或确认broken时触发。
> 
> instance：imagesLoaded 实例对象。


###done

	imgLoad.on( 'done', function( instance ) {
	  console.log('DONE  - all images have been successfully loaded');
	});   

> 在所有图片被成功加载没有broken图片的时候触发。


###fail

	imgLoad.on( 'fail', function( instance ) {
	  console.log('FAIL - all images loaded, at least one is broken');
	});

> 在所有图片被加载至少有一幅图片是broken图片的时候触发。  

###progress

	imgLoad.on( 'progress', function( instance, image ) {
	  var result = image.isLoaded ? 'loaded' : 'broken';
	  console.log( 'image is ' + result + ' for ' + image.img.src );
	});  

>    在每一幅图片都被加载之后触发。
> 
-  instance：imagesLoaded 实例对象。
-  image：加载图片时的loading对象的实例对象。


##属性

###LoadingImage.img

img：img元素。

###LoadingImage.isLoaded

Boolean ：当图片被成功加载时为true。

###imagesLoaded.images

要检测的所有图片的LoadingImage实例对象。


	var imgLoad = imagesLoaded('#container');
	imgLoad.on( 'always', function() {
	  console.log( imgLoad.images.length + ' images loaded' );
	  // detect which image is broken
	  for ( var i = 0, len = imgLoad.images.length; i < len; i++ ) {
	    var image = imgLoad.images[i];
	    var result = image.isLoaded ? 'loaded' : 'broken';
	    console.log( 'image is ' + result + ' for ' + image.img.src );
	  }
	});      


##与jQuery结合使用

如果你在页面中引入的jQuery，imagesLoaded 可以作为一个jQuery插件来使用。

	$('#container').imagesLoaded( function() {
	  // images have loaded
	});    

###jQuery Deferred

作为jQuery插件来使用时会返回一个jQuery Deferred object。你可以和前面使用事件的方法一样使用.always()、.done()、.fail()和.progress()。

	$('#container').imagesLoaded()
	  .always( function( instance ) {
	    console.log('all images loaded');
	  })
	  .done( function( instance ) {
	    console.log('all images successfully loaded');
	  })
	  .fail( function() {
	    console.log('all images loaded, at least one is broken');
	  })
	  .progress( function( instance, image ) {
	    var result = image.isLoaded ? 'loaded' : 'broken';
	    console.log( 'image is ' + result + ' for ' + image.img.src );
	  });   