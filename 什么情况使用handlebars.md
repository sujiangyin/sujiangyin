 为什么使用Handlebarsjs

首先先来谈论一下，数据-展现分离的 web前端开发思路吧。

近些年来，我们已经看到一个趋势：越来越多的 Web 开发工作在前端完成。我们通过 Ajax 或 Web Sockets 与后端通信，然后在以某种方式在 UI 中显示数据。这样在前后端的开发过程中耦合性便降到了最低，前端工程师就可以没有限制的发挥他自己的创意、和妙想。
另外数据拿到了前端来操作，也一定程度的减少了后端的压力，让后端专心的去操作和处理数据的请求。

这带来了一个问题，我们的数据怎么在前端来渲染呢？

一种简单的解决方案是：使用字符串拼接来构建富用户界面,使用一大堆的＋号与数据揉合然后插入到DOM中去。

这是一种简单粗暴的方法，这样做虽然简单但是带来的问题是，数据与界面有一次的胶着在一块了从而导致大量让人讨厌的代码，
并且容易出现错误。

下面我们来看看聪明人的解决方案：

“有没有比这更好的方式？我们不能通过某种方式把展示从数据中分离出来吗？”，就这样，模板的概念进入了我们的世界。

介绍一个模版库 Handlebar.js 来满足这种需求。

模版：简单来说就是一个需要渲染动态数据 view-model.*/

1、先来看看传统的操作方式：

/****JS****/

(function( blog, $ ) {

   var _selection;

       

   blog.init = function( $selection ) {

       _selection = $selection;

   };

   

   blog.displayTweets = function( tweets ) {

       var $list = $( "<ul/ >" );

       

       $.each( tweets || {}, function( index, tweet ) {

           var html = "<i>" + moment( tweet.created_at ).fromNow() + "</i>: ";//这里的moment函数是自定义的，
           暂时不给出

           html += "<b>" + tweet.user.name + "</b> - ";

           html += "<span>" + tweet.text + "</span>";

           html += tweet.retweeted ? " <i class='icon-repeat'></i>" : "";

           html += tweet.favorited ? " <i class='icon-star'></i>" : "";

           

           $( "<li />", { html: html }).appendTo( $list );         

       });

           

      _selection.empty().append( $list.children() );         

   };

}( window.blog = window.blog || {}, jQuery ));

 

var tweets = [

   { id: 0, created_at: "Mon Apr 11 8:00:00 +0000 2013",  text: "Test Tweet 1",

    favorited: false, retweeted: false, user: { name: "User 1" } },

   { id: 1, created_at: "Mon Apr 11 9:00:00 +0000 2013",  text: "Test Tweet 2",

    favorited: true,  retweeted: true,  user: { name: "User 2" } },

   { id: 2, created_at: "Mon Apr 11 10:00:00 +0000 2013", text: "Test Tweet 3",

    favorited: false, retweeted: true,  user: { name: "User 3" } },

   { id: 3, created_at: "Mon Apr 11 11:00:00 +0000 2013", text: "Test Tweet 4",

    favorited: true,  retweeted: false, user: { name: "User 4" } },

   { id: 4, created_at: "Mon Apr 11 12:00:00 +0000 2013", text: "Test Tweet 5",

    favorited: true,  retweeted: true,  user: { name: "User 5" } }

];

 

$( document ).ready( function() {

   twitter.init( $( ".tweets" ) );

   twitter.displayTweets( tweets );

});

/******HTML******/

<div class="tweets"></div>

 

2、模版的形式-handlebars.js：

我们来分析一下上面的代码，上面的有一个twitter 的对象，实现了两个方法：初始化和渲染数据。

但是我们看到渲染数据的方法的代码惨不忍睹，我们的js代码里面出现了一堆DOM的东西。我们以后随着代码的增加将会陷入灾难。

我们的模版来了，它们可以帮助我们简化代码，并从代码中分离标记。

/** tweetListView.js  **/

twitter.init = function( $selection ) {

   _selection = $selection;

   

   Handlebars.registerHelper( "fromNow", function( time ) {

       return moment( time ).fromNow();// 这里利用了handlebars里面的 registerHelper

   });

};

 

twitter.displayTweets = function( tweets ) {

   var templateString = $( "#tweets-handlebars" ).html(),

       template = Handlebars.compile( templateString );//预编译模版

                       

   _selection.empty().append( template( tweets ) );         

};

/** tweets.tpl **/

   <ul>

       {{#each this}}

       <li>

           <i>{{fromNow this.created_at}}</i>:

           <b>{{this.user.name}}</b> -

           <span>{{this.text}}</span>

           {{#if this.retweeted}}<i class="icon-repeat"></i>{{/if}}

           {{#if this.favorited}}<i class="icon-star"></i>{{/if}}

       </li>

       {{/each}}

   </ul>

   看到了吗？我们现在就可以像PHP等动态语言一样的在模版里面标记数据了。

Handlebars 是一个弱逻辑模板引擎，鼓励你分离展现和逻辑，并且速度更快，并且提供了一种预编译模板的机制，
你可以在服务端预编译模板，可以运行在客户端和服务端

所以我们就可以结合Node.js 来实现服务器端的预编译。在后端预编译模版，减少前端的工作量，
并切可以在前端值运行hanlebars的精简版。这样是不是就很酷了。

继续回到  数据-展现分离，这似乎已经成为一种趋势，特别是spa的应用当中对他的要求也是极为迫切的
。目前来看angularjs 的双向数据绑定让富应用插上了腾飞的翅膀，

但是理念是一样的，我们要在数据-展现分离的道路上越走越远。

新年期望~~

对w3c标准进一步理解；

对es5有所研究；

对前端性能优化找到较好的实践；

总结一套比较合理的前端框架。
