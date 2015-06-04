笔记链接地址：https://learn.jquery.com/plugins/advanced-plugin-concepts/。
定义我们的插件：
```js
// Plugin definition.
$.fn.hilight = function( options ) {
 
    // Extend our default options with those provided.
    // Note that the first argument to extend is an empty
    // object – this is to keep from overriding our "defaults" object.
    var opts = $.extend( {}, $.fn.hilight.defaults, options );
 
    // Our plugin implementation code goes here.
 
};
 
// Plugin defaults – added as a property on our plugin function.
$.fn.hilight.defaults = {
    foreground: "red",
    background: "yellow"
};
```
在script里面修改插件的默认属性：
```js
// This needs only be called once and does not
// have to be called from within a "ready" block
$.fn.hilight.defaults.foreground = "blue";
```
现在的调用就是设置以后的新的默认属性：
```js
$( "#myDiv" ).hilight();
```
可以通过给插件传递参数覆盖新设置的默认属性：
```js
// Override plugin default foreground color.
$.fn.hilight.defaults.foreground = "blue";
 
// ...
 
// Invoke plugin using new defaults.
$( ".hilightDiv" ).hilight();
 
// ...
 
// Override default by passing options to plugin method.
$( "#green" ).hilight({
    foreground: "green"
});
```
拓展插件(public)：
```js
// Plugin definition.
$.fn.hilight = function( options ) {
 
    // Iterate and reformat each matched element.
    return this.each(function() {
 
        var elem = $( this );
 
        // ...
 
        var markup = elem.html();
 
        // Call our format function.
        markup = $.fn.hilight.format( markup );
 
        elem.html( markup );
 
    });
 
};
 
// Define our format function.
$.fn.hilight.format = function( txt ) {
    return "<strong>" + txt + "</strong>";
};
```
cycle插件：
```js
Jquery插件—Cycle Plugin插件实现图片循环播放等 
 
图片展示效果的jQuery插件很多，都非常实用，我常用的就是jQuery Cycle Plugin。 
jQuery Cycle Plugin循环插件，不仅支持图片循环，而且支持任意元素的循环功能，效果非常丰富，可支持鼠标悬停暂停，自动停止，开始和结束事件调用等等，目前你所 看到的图片效果大部分都支持。 
需要自定义CSS样式，如： 
#ads { height:  200px; width:  200px; overflow:hidden;} #ads .ads li, 
#ads .ads li img {width:960px; height:387px;} 使用实例： 
 
一，包含文件部分 
自己在网上下载jquery插件和这个循环图片播放的插件： <script type="text/javascript" src="jquery.js"></script>  <script type="text/javascript" src="jquery.cycle.js"></script>  
二，HTML部分  
<div id="ads">  <ul class="ads"> 
  <li><a href="#dinggou"><img src="images/ts_01.jpg" alt=""/></a></li>   <li><a href="#dinggou"><img src="images/ts_02.jpg" alt=""/></a></li>   <li><a href="#dinggou"><img src="images/ts_03.jpg" alt=""/></a></li>  
</ul> 
</div> 
三，Javascript部分 $(function(){ 
$('#ads .ads').cycle（{ fx : 'fade', timeout : 4000, pager : '.num',   pagerEvent : 'mouseover',   pauseOnPagerHover : true, 
  activePagerClass : 'current', 
  pagerAnchorBuilder : function(idx, slide) {   idx += 1; 
  return '<li>' + '</li>';  
 
}）；  
}) 
jQuery Cycle Plugin插件使用非常简单，如上实例对ID为ads的DIV元素调用jQuery Cycle Plugin，
这样就实现了一个图片循环展示的效果，jQuery Cycle Plugin可选效果非常多.
```
插件private化,建立closure：
```js
// Create closure.
(function( $ ) {
 
    // Plugin definition.
    $.fn.hilight = function( options ) {
        debug( this );
        // ...
    };
 
    // Private function for debugging.
    function debug( obj ) {
        if ( window.console && window.console.log ) {
            window.console.log( "hilight selection count: " + obj.length );
        }
    };
 
    // ...
 
// End of closure.
 
})( jQuery );
```
具体的参数也是一种病：
```js
jQuery.fn.superGallery = function( options ) {
 
    // Bob's default settings:
    var defaults = {
        textColor: "#000",
        backgroundColor: "#fff",
        fontSize: "1em",
        delay: "quite long",
        getTextFromTitle: true,
        getTextFromRel: false,
        getTextFromAlt: false,
        animateWidth: true,
        animateOpacity: true,
        animateHeight: true,
        animationDuration: 500,
        clickImgToGoToNext: true,
        clickImgToGoToLast: false,
        nextButtonText: "next",
        previousButtonText: "previous",
        nextButtonTextColor: "red",
        previousButtonTextColor: "red"
    };
 
    var settings = $.extend( {}, defaults, options );
 
    return this.each(function() {
        // Plugin code would go here...
    });
 
};
```
要的参数找不到，不要的参数一大堆。
```js
var delayDuration = 0;
 
switch ( settings.delay ) {
 
    case "very short":
        delayDuration = 100;
        break;
 
    case "quite short":
        delayDuration = 200;
        break;
 
    case "quite long":
        delayDuration = 300;
        break;
 
    case "very long":
        delayDuration = 400;
        break;
 
    default:
        delayDuration = 200;
 
}
```
占用代码空间又缩小了范围，太详细！

让插件访问的元素被完全控制Give Full Control of Elements：
bad
```js
// Plugin code
$( "<div class='gallery-wrapper' />" ).appendTo( "body" );
 
$( ".gallery-wrapper" ).append( "..." );
```
good
```js
// Retain an internal reference:
var wrapper = $( "<div />" )
    .attr( settings.wrapperAttrs )
    .appendTo( settings.container );
 
// Easy to reference later...
wrapper.append( "..." );
```
settings的信息写在：
```js
var defaults = {
    wrapperAttrs : {
        class: "gallery-wrapper"
    },
    // ... rest of settings ...
};
 
// We can use the extend method to merge options/settings as usual:
// But with the added first parameter of TRUE to signify a DEEP COPY:
var settings = $.extend( true, {}, defaults, options );
```
让使用这定义css样式：
```js
var defaults = {
    wrapperCSS: {},
    // ... rest of settings ...
};
 
// Later on in the plugin where we define the wrapper:
var wrapper = $( "<div />" )
    .attr( settings.wrapperAttrs )
    .css( settings.wrapperCSS ) // ** Set CSS!
    .appendTo( settings.container );
```
提高回调的能力：
```js
var defaults = {
 
    // We define an empty anonymous function so that
    // we don't need to check its existence before calling it.
    onImageShow : function() {},
 
    // ... rest of settings ...
 
};
 
// Later on in the plugin:
 
nextButton.on( "click", showNextImage );
 
function showNextImage() {
 
    // Returns reference to the next image node
    var image = getNextImage();
 
    // Stuff to show the image here...
 
    // Here's the callback:
    settings.onImageShow.call( image );
}
```
取代上面的方法初始化回调函数的好处是可以使用this：
```js
$( "ul.imgs li" ).superGallery({
    onImageShow: function() {
        $( this ).after( "<span>" + $( this ).attr( "longdesc" ) + "</span>" );
    },
 
    // ... other options ...
});
```
