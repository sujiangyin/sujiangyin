当用javascript写一个完整的web应用，让它变得有组织就很重要；维持一个意大利面条编码的项目只会让你噩梦连连。
在这个教程中，我会指导你怎么样模块化你的代码而让它更容易管理1很大的javascript工程。

介绍：
最近，我重复看了我最爱的javascript的演示之一：可扩展的javascript应用程序，作者Nicolas C. Zakas。
我发现他推荐的模块化的js模式很迷人，所以我决定尝试一下。我从他的幻灯片获取代码的理论片段，延伸和自定义他们，然后提出
我今天想展现给你的内容。我几乎不能说是一个专家：我仅仅一个星期就想出了这个写javscript的方法。但我希望可以告诉你的是如何
能够把zakas运用到实践中，让你按照这个方式去思考代码。

总结：
总的来说，一个好的javascript会包含四层，从下到上，依次是：
base：
```js
这个会是你的javascript框架，像jquery，如果你正在使用一个，或者写你自己的。
```
app core:
```js
它是控制其他所有东西勾在一起和你的web app运行的。它也为sandbox提供一个平衡层；这似乎看起来有点冗余一旦你进
入理解它。而且你会质疑为什么不让它直接与base层通话。然而，由于一个不同的框架，我们让它经过核心运行所有东西以便我们更加
容易交换base。那么，我们就只需要改变core的中间层的方法。
```
The Sandbox:
```js
这是个小型的api。仅仅是api——我们web app的每个部分都可以有权进入。一旦你已经创建了自己喜欢的sandbox，外部接口不要发生
任何改变就变得很重要。这是因为你将会有更多的模块依赖于它，改变api就意味着你要穿过每个模块而更新它——这不是你想要的
。当然，你可以在sanndbox的方法里修改代码（尽管它们还是做同样的事情），或添加方法。
```
modules:
```js
这些是真正在web app工作的东西。每个module都是自我包含的代码片段并且运行app的单个方面。我之前使用这种方式写了web app，
所有的模块代码交织在一起就像意大利面一样混乱，当一个片段消失，整个app就崩溃。当所有东西都被整齐的放在module里面
（用sandbox提供的交互），一个片段消失就不会引起任何错误。
```
为了解释这个方案，我们将会提供一个小型的网上商店（仅仅是前台）。这就真正表现出模块的好处，因为一个网上商店有很多分界
明显的部分：
```js
a product panel
a shopping cart
a search box
a way to filter products
```
每个这些片段都很清楚的被分离在页面中，但他们都需要和其他的模块交互——不是一个初始者就是一个接受者。我们将会看到这是怎
么工作的！
好的，理论足够，我们开始代码吧。


建modele块：
当我开始这个工程时，我没有一个core或者sandbox来支持。所以我决定从编写module开始，因为这个会给我一个idea关于modeles需要什么来进入sandbox。


当创建一个module，我使用一个这样方案，和zakas的例子的代码很相似：
```js
CORE.create_module("search-box", function (sb) { 
    return { 
        init : function () {}, 
        destroy : function () {} 
    }; 
});
```
正如你所看到的，我们在core上使用一个方法create_module来注册这个module。（当然，我还没有创建这个core。但我们会走到那一步的。）这个方法将会有两参数：module的名字（在这个例子中是“search-box”），还有一个是方法，这个方法返回一个module对象。我在这里show出来的可能是最基础的module模型。注意，创造方法的几点：第一、有个单一的参数，它将是我们的sandbox的一个实例（当我们看到sandbox，我们会看到为什么实例会比有所有的module权限进入同样的sandbox对象更加好）.这个sandbox对象仅仅是一个module对外的一个连接（当然，正如zakas说的，没有技术上的限制阻止你直接从模块内进入base或core但你不应该这么做。）如你所见，这个方法返回一个对象，就是我们的module对象。至少，module仅仅有一个init方法（当我们启用module时被使用），还有一个destroy方法（当我们关闭这个module时被使用）。

所以，让我们来真正的创建一个module。

The Search Box Module
```js
  var input, button, reset; 
 
    return { 
        init : function () {}, 
        destroy : function () {}, 
        handleSearch : function () {}, 
        quitSearch : function () {} 
    }; 
});
```
你应该理解这里发生了什么：我们的module将会使用三个变量：input，
button和reset。除此之外，还有两个必须的module方法在return对象里，我们正创建两个和search相关的方法。我想没有什么理由它们必须在return模块的对象里；它们可以在return的声明上面被声明，然后1通过调用来使用。让我们进入它的每一个方法：

```js
init : function () { 
    input = sb.find('#search_input')[0]; 
    button = sb.find('#search_button')[0]; 
    reset  = sb.find("#quit_search")[0]; 
 
    sb.addEvent(button, 'click', this.handleSearch); 
    sb.addEvent(reset, 'click', this.quitSearch); 
}
```
首先，我们分配需要的三个变量。由于我们需要来自module的dom元素，我们需要一个find方法在我们sandbox里面。最好【0】所得到的是什么？sb.find方法返回一些相似的jquery对象：匹配选择器的元素会被赋予一个数字键key，他们是一些方法或属性。然而，我们只打算要一个原dom元素，我们是在抢占（由于只有一个元素会有那个id，我们就可以确保只返回一个元素。）


这些元素的两个是botton（button和reset），因此我们需要hook up一些事件处理。必须也要把它们添加到我们sandbox！正如你看到的，增加事件的方法是你的标准：它需要元素，时间和方法。

摧毁一个module又是怎样的呢：

```js
destroy : function () { 
    sb.removeEvent(button, 'click', this.handleSearch); 
    sb.removeEvent(reset, 'click', this.quitSearch); 
    input = null; 
    button = null; 
    reset = null; 
},
```

这个非常简单：一个和addEvent方法相反作用的removeEvent。然后，设置三个变量为空。

这两个事件的监听参考search方法，让我们先看第一个：
```js
handleSearch : function () { 
    var query = input.value; 
    if (query) { 
        sb.notify({ 
            type : 'perform-search', 
            data : query 
        }); 
    } 
},
```
首先，获得search的input值。如果有东西，就继续前进。但我们该做什么？
正常来说，当正在做一个动态的search（那不需要一个页面ajax更新），我们将有权进入产品面板和恰当的过滤它们。但我们的module要存在不管有还是没有产品面板；并且，它只通过sandbox和外界联系。所有这就是zakas建议的：我们仅仅告诉sandbox（它会轮着告诉core）用户已经执行了搜索操作。那么core就会给其他module提供信息。如果有i个回应，它就会拿着数据运行它。我们通过sb.notify方法做这件事；它带着一个有两个属性的对象：我们要执行的事件的类型和与事件相关的数据。在这个例子中，我们在做一个‘perform-search’的事件，相关的数据是query。这是search box所要做的；如果还有另外一个module暴露可以被搜素的能力，core就会把数据给它。









