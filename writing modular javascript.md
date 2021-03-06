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

这些是真正在web app工作的东西。每个module都是自我包含的代码片段并且运行app的单个方面。
我之前使用这种方式写了web app，所有的模块代码交织在一起就像意大利面一样混乱，当一个片段消失，整个app就崩溃。
当所有东西都被整齐的放在module里面（用sandbox提供的交互），一个片段消失就不会引起任何错误。

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
正如你所看到的，我们在core上使用一个方法create_module来注册这个module。（当然，我还没有创建这个core。但我们会走到那一步的。）这个方法将会有两参数：module的名字（在这个例子中是“search-box”），还有一个是方法，
这个方法返回一个module对象。我在这里show出来的可能是最基础的module模型。注意，创造方法的几点：第一、有个单一的参数，它将是我们的sandbox的一个实例（当我们看到sandbox，我们会看到为什么实例会比有所有的module权限进入同样的sandbox对象更加好）.这个sandbox对象仅仅是一个module对外的一个连接（当然，正如zakas说的，没有技术上的限制阻止你直接从模块内进入base或core但你不应该这么做。）如你所见，这个方法返回一个对象，就是我们的module对象。至少，module仅仅有一个init方法（当我们启用module时被使用），还有一个destroy方法（当我们关闭这个module时被使用）。

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
首先，我们分配需要的三个变量。由于我们需要来自module的dom元素，我们需要一个find方法在我们sandbox里面。最好【0】所得到的是什么？sb.find方法返回一些相似的jquery对象：匹配选择器的元素会被赋予一个数字键key，他们是一些方法或属性。然而，我们只打算要一个根dom元素，我们是在抢占（由于只有一个元素会有那个id，我们就可以确保只返回一个元素。）


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
正常来说，当正在做一个动态的search（那不需要一个页面ajax更新），我们将有权进入产品面板和恰当的过滤它们。
但我们的module要存在不管有还是没有产品面板；并且，它只通过sandbox和外界联系。所有这就是zakas建议的：
我们仅仅告诉sandbox（它会轮着告诉core）用户已经执行了搜索操作。那么core就会给其他module提供信息。如果有i个回应，它就会拿着数据运行它。我们通过sb.notify方法做这件事；它带着一个有两个属性的对象：我们要执行的事件的类型和与事件相关的数据。在这个例子中，我们在做一个‘perform-search’的事件，相关的数据是query。
这是search box所要做的；如果还有另外一个module暴露可以被搜素的能力，core就会把数据给它。


注释这个的整洁之处在于方法是很多的。module在我们这个例子中使用这个事件将不会做任何的ajax-y。
但是没有理由另外一个module不会那样，或者搜索一些其他方法。

quitSearch方法不是那么的复杂。

```js
quitSearch : function () { 
    input.value = ""; 
    sb.notify({ 
        type : 'quit-search', 
        data : null 
    });                 
}
```

首先我们清空search的内容，然后让sandbox知道我们正在做一个‘quit-search’的操作；另外这没有相关的数据。
相不相信都好，这就是整个search的module，相当简单吧，让我们进入下一个。

The Filters Bar Module
我们开网上商店来让用户有能力通过分类看产品。所以，让我们完善filters bar，就是用户点击分类的名字来显示已经给了分类的项目。

```js
CORE.create_module("filters-bar", function (sb) { 
    var filters; 
    return { 
        init : function () { 
            filters = sb.find('a'); 
            sb.addEvent(filters, 'click', this.filterProducts); 
        }, 
        destroy : function () { 
            sb.removeEvent(filters, 'click', this.filterProducts); 
            filters = null; 
        }, 
        filterProducts : function (e) { 
            sb.notify({ 
                type : 'change-filter', 
                data : e.currentTarget.innerHTML 
            }); 
        } 
    };         
});
```
这其实不是很复杂。我们需要一个变量来支持filters。如你所见，我们正在使用sb.find得到所有的引擎。
但是什么页面上的所有引擎被过滤的机会是什么？不是很好。一旦我们去写find方法你就知道它是怎么返回和
我们的module一致的dom元素。然后，我们添加一个点击时间到这个filters，它调用filterProduct方法。
如你所见，这个方法只是简简单单告诉sandbox我们的‘change-filter’事件，给点击的数据的链接以文本
（e是事件对象，currentTarget是被点击的元素）。

当然，destory简单地摆脱了事件监听。

The Product Panel Module

这个可能会有点长和有点复杂，我们以这个壳开始：
```js
CORE.create_module(“product-panel”, function (sb) {  
    var products; 
         
    function eachProduct(fn) { 
        var i = 0, product; 
        for ( ; product = products[i++]; ) { 
            fn(product); 
        } 
    } 
    function reset () { 
        eachProduct(function (product) { 
            product.style.opacity = '1';     
        }); 
    } 
    return { 
        init : function () {}, 
        reset : reset, 
        destroy : function () {}, 
        search : function (query) {}, 
        change_filter : function (filter) {}, 
        addToCart : function (e) {} 
 
    };       
});
```
花点时间浏览上面这段代码；和我们以前看过的module没有很大的不同；只有一些。主要的不同就是我已经使用两个helper方法，
创造外面的返回对象。第一个叫做eachProduct，如你所见，它仅仅使用一个方法并且在产品列表的每个item里面运行它。
另外一个是reset方法。我们可以一眼就看懂它。

现在我们看看init和destory的方法。
```js
init : function () { 
    var that = this; 
    products = sb.find('li'); 
    sb.listen({ 
            'change-filter'  : this.change_filter, 
            'reset-fitlers' : this.reset, 
            'perform-search' : this.search, 
            'quit-search'    : this.reset 
        }); 
    eachProduct(function (product) { 
        sb.addEvent(product, 'click', that.addToCart);        
    });  
}, 
destroy : function () { 
    var that = this; 
    eachProduct(function (product) { 
        sb.removeEvent(product, 'click', that.addToCart);         
    }); 
    sb.ignore(['change-filter', 'reset-filters', 'perform-search', 'quit-search']); 
},
```
在init里面，我们收集所有的产品（我们把它们放在list item里面）。然后，我们必须让sandbox知道我们对哪几个事件感兴趣。我们传递一个对象给sb.listen，
这个对象使用事件的名字作为它的key，还有事件的方法作为它的值对于每个属性。例如，
我们正在告诉sandbox什么时候某人要执行‘perform-search’的事件，我们想要回应它通过执行我们的search方法。
很有希望，你会看到它是怎么工作的！


然后，我们使用我们的eachProduct 方法来分配点击事件到每个产品。当一个产品被点击，我们会运行addToCart
。我们必须缓存this因为它的值在这个方法里会改变全局对象。

在destroy里面，我们简简单单地从产品移除了事件的handles，然后让sabdbox知道不再对事件感兴趣
（事实上我认为没有必要，因为我们在core里面处理事情的方式。但是我扔出它是因为有些东西在‘back end’发生改变了。）

现在我们就会看到其他模块触发这个事件的时候调用这个方法。

```js
search : function (query) { 
    reset(); 
    query = query.toLowerCase(); 
    eachProduct(function (product) { 
        if (product.getElementsByTagName('p')[0].innerHTML.toLowerCase().indexOf(query) < 0) { 
            product.style.opacity = '0.2'; 
        }  
    }); 
}, 
change_filter : function (filter) { 
    reset(); 
    eachProduct(function (product) { 
    if (product.getAttribute('data-8088-keyword').toLowerCase().indexOf(filter.toLowerCase()) < 0) { 
        product.style.opacity = '0.2'; 
        } 
    }); 
}, 
addToCart : function (e) { 
    var li = e.currentTarget; 
    sb.notify({ 
        type : 'add-item', 
        data : { id : li.id, name : li.getElementsByTagName('p')[0].innerHTML, price : parseInt(li.id, 
        10) } 
    });  
}
```
我们以search开始；这个方法在‘perform-search’出现的地方被调用。如你所见，它把search的query作为参数。
首先，我们重置产品区域（只是在前面搜索的结果或过滤的结果）。然后，我们在每个产品上面去循环我们的helper方法。
记住，产品是一个项目列表；在里面它是一个带有产品描述的段，那是我们将要搜索的（在实际例子中，这可能不是什么事）。
我们获得段落的文本然后将它和query的文本做对比。（注意，这两个都运行了toLowerCase（）方法。）
如果结果比0还小，即意味着没有找到匹配的东西，我们会把产品的透明度设置为0.2.那就是我们将要怎么隐藏产品的方法
在这个例子当中。


要指出的是，所有的reset方法都会将所有产品的透明度重置为1。

changeFilters的方法和search方法非常相似；这次，取代搜索产品的描述，我们更加取向使用html5的data-*属性。这些允许我们添加自定义属性到html的元素而没有破坏sepc规则。然而他们必须以‘datat-’为开始，而且我必须添加自己个人的后缀给它，因此它们就不会和第三方代码使用的属性相冲突。传递给这个方法的filters和data属性相比较会包含items的类名。如果不匹配我们会降低这个元素的透明度。

最后一个方法是addToCart，产品之一被点击时它就会被运行，我们会得到这个点击元素并且发通知到系统，通告它‘add-item’事件。这次，我们传递给它的是一个对象。它包含产品id，产品名字，还有产品价格。在这个例子当中，我们很懒而使用元素id作为id和价格，使用产品描述作为名字。



The Shopping Cart Module
我们来看再一个模块。就是“shopping cart” module：

```js
CORE.create_module(“shopping-cart”, function (sb) {  
    var cart, cartItems; 
 
    return { 
        init : function () { 
            cart = sb.find('ul')[0];  
            cartItems = {}; 
 
            sb.listen({ 
                'add-item' : this.addItem 
            }); 
        }, 
        destroy : function () { 
            cart = null; 
            cartItems = null; 
            sb.ignore(['add-item']); 
        }, 
        addItem : function (product) { 
        } 
    }; 
});
```
我认为你正在慢慢掌握它了；我们使用了两个变量：cart里面的the cart和 the item。在module的初始化阶段，我们会设置这些到购物车的ul还有一个空的对象。然后，我们会让sandbox知道我们想要响应一个事件。在摧毁事件上我们都不做这些。


下面是当一个item被添加到购物车时会发生的事。

```js
addItem : function (product) { 
    var entry = sb.find('#cart-' + product.id + ' .quantity')[0]; 
    if (entry) { 
        entry.innerHTML =  (parseInt(entry.innerHTML, 10) + 1); 
        cartItems[product.id]++; 
    } else { 
        entry =
        sb.create_element('li', { id : "cart-" + product.id, children : [ 
        sb.create_element('span', { 'class' : 'product_name', text : product.name }), 
        sb.create_element('span', { 'class' : 'quantity', text : '1'}), 
        sb.create_element('span', { 'class' : 'price', text : '$' + product.price.toFixed(2) }) 
                ], 
                'class' : 'cart_entry' }); 
 
        cart.appendChild(entry); 
        cartItems[product.id] = 1; 
    } 
}
```
这个方法把我们刚才在产品面板的module看到的产品对象为参数。然后，我们通过选择器‘#cart-’+product.id+'.quntity'获得某个元素。这个方法通过在一个有id：cart-id_number内加一个类quantity’ 来查找元素。如果在之前已经被添加到购物车，这就会被找到。如果找到了，我们就增加这个元素的内部html（用户已经添加到购物车的产品的数量）然后更新cartItems对象的入口，它保持跟踪购物流程。

如果没有找到这个元素，就是用户第一次添加这个产品到购物车。在这种情况下，我们会使用sandbox的create_element方法；如你所见，它会带着一个类似jquery的属性对象。这里有个特殊的例子就是孩子属性，它是一个元素的数组用来插入到我们创建的元素当中。如你所见，我们用三个span标签创建一个列表item：产品名字，数量，和价格。然后，我们追加这个列表到购物车和添加这个产品到cartItem对象。

那就是我们的module的所有代码；我应该解释我已经把这些放在一个文件叫做module.js。现在我们知道我们的module将需要什么接口来工作，我们已经准备好去建立sandbox啦。


The support:sandbox
我知道我已经在之前提及过这个，但是sandbox的外部接口不发生改变真的很重要。这是因为所有的sandbox都依赖它。当然，你可以在方法里添加方法或改变代码，只要你没有改变这个方法和函数的返回。


如果我们提取module.js文件，我们将会看到有以下这些方法是sandbox需要提供给这个module的：
```js
find
addEvent
removeEvent
notify
listen
ignore
create_element
```
所以让我们开始工作。因为我要使用sandbox.create的代码来创造一个sandbox的实例，我们需要创造一个只有一个方法的对象：

```js
var Sandbox = { 
    create : function (core, module_selector) { 
            var CONTAINER = core.dom.query('#' + module_selector); 
            return { 
                 
            }; 
        }     
};
```
这里，我们开始。如你所见，create的方法会得到两个参数：一个指向core还有一个是module的名字。
那么我们创造一个变量CONTAINER，它指向和module代码一致的dom元素。现在，我们开始写我们列出来的方法的代码。

```js
find : function (selector) { 
    return CONTAINER.query(selector); 
},
```
这个相当简单。事实上，大部分sandbox的方法都相当简单，因为它应该要有很简单的外包来给module提供适量的权限进入core。当我们调用的core.dom.query方法返回一个容器，它就会给容器一个方法通过选择器来让它找到孩子元素，我们正在使用这个来限制一个module的能力去影响到dom，因此把它保持得想javascript一样在html中。

```js
addEvent : function (element, evt, fn) { 
    core.dom.bind(element, evt, fn); 
}, 
removeEvent : function (element, evt, fn) { 
    core.dom.unbind(element, evt, fn); 
},
```

像我说的那样，大部分这些sandbox都相当小；我们只会将事件数据用数据连接到钩子上。

```js
notify : function (evt) { 
    if(core.is_obj(evt) && evt.type) { 
        core.triggerEvent(evt); 
    } 
}, 
listen : function (evts) { 
    if (core.is_obj(evts)) { 
        core.registerEvents(evts, module_selector); 
    }             
},  
ignore : function (evts) { 
    if (core.is_arr(evts)) { 
        core.removeEvents(evts, module_selector); 
    }          
},
```
这三个方法，如你所见，是modules用来通知其他modules它的行为的工具。我已经添加了一些错误核对的东西来确保在我们发送数据到core上之前所有东西都okay。注意，当我们需要告诉core我们要监听什么或忽视什么时，要传递modules的名字。

```js
create_element : function (el, config) { 
    var i, text; 
    el = core.dom.create(el); 
    if (config) { 
        if (config.children && core.is_arr(config.children)) { 
            i = 0; 
            while (config.children[i]) { 
                el.appendChild(config.children[i])); 
                i++; 
            } 
            delete config.children; 
        } else if (config.text) { 
           text = document.createTextNode(config.text); 
           delete config.text; 
           el.appendChild(text); 
        } 
        core.dom.apply_attrs(el, config); 
    } 
    return el; 
}
```
这个很明显是sandbox的最长的方法（诚实地说，我想得越多，我就认为它更应该放在core里面，但anyway~~~）。像我们所知道的，它携带元素的名字和对象的配置，我们从创造dom元素开始（使用一个core方法）。然后，如果有配置对象，而且它有一个数组叫做children，我们在每个孩子上面去循环然后追加到元素上。不然我们就删除孩子属性，如果我们有一个文本属性，我们会设置j一个元素的文本到它上面然后删除文本属性（在这个例子中我们有孩子和文本）。最后，我们会使用另外一个core方法应用剩下的属性还有返回元素。


好啦，sandbox结束了。我知道这个很简单，但是只是给你一个印象怎么去创造sandbox，你还是可以创建其他的方法的根据你自己的需要。



The Fundation: Base and core
我们的下一步是core,以下面的代码为开始：
```js
var CORE = (function () { 
    var moduleData = {}, debug = true; 
 
    return { 
        debug : function (on) { 
            debug = on ? true : false; 
        }, 
 
    }; 
 
}());
```
我们将要使用module数据模型来存储针对这个module所知道的全部东西；debug变量是否出错显示在控制台。我们用debug方法来开启或关闭错误。

让我们以用来注册module的方法create_module为开始：
```js
create_module : function (moduleID, creator) { 
    var temp; 
    if (typeof moduleID === 'string' && typeof creator === 'function') { 
        temp = creator(Sandbox.create(this, moduleID)); 
if (temp.init && temp.destroy && typeof temp.init === 'function' && typeof temp.destroy === 
'function') { 
    moduleData[moduleID] = { 
        create : creator, 
        instance : null 
    }; 
    temp = null; 
} else { 
this.log(1, "Module \"" + moduleId + "\" Registration: 
FAILED: instance has no init or destroy functions"); 
} 
} else { 
this.log(1, "Module \"" + moduleId +  "\" Registration: FAILED: 
one or more arguments are of incorrect type" ); 
 
    } 
},
```
我们要做的第一件事就是确保参数传递到方法的类型是正确的；如果不正确，我们就调用一个log方法，它携带一个安全号码和信息。

接下来，我们创建一个module的副本仅仅是为了确保它有个init和destory的方法；如果没有，我们再log一个错误。如都通过了，我们就增加一个对象到moduleData；当我们启动模块时，我们将为实例存储creator的方法和一个空点。然后我们就删除module的复制模块。

```js
start : function (moduleID) { 
    var mod = moduleData[moduleID]; 
    if (mod) { 
        mod.instance = mod.create(Sandbox.create(this, moduleID)); 
        mod.instance.init(); 
    } 
}, 
start_all : function () { 
    var moduleID; 
    for (moduleID in moduleData) { 
        if (moduleData.hasOwnProperty(moduleID)) { 
            this.start(moduleID); 
        } 
    } 
},
```
接下来，我们添加一个方法来启动module；你可能会期望，它接收一个module的名字作为一个简单的参数。如果有在moduleData里面有一致的模块，我们就运行他的方法，传递给它一个新的sandbox实例。然后，我们会启用它通过它的init方法。

我们可以创造一个方法使得很容易就启用所有的modules，因为这是我们将想要的。我仅仅需要在moduleData上循环然后把每个module的id传递到每个start方法里面。不要忘记使用hasOwnProperty部分，我知道它很没必要又很丑陋（至少对于我来说），但是有一种情况：某人想要添加item到对象的原型对象。

```js
stop : function (moduleID) { 
	var data; 
	if (data = moduleData[moduleId] && data.instance) { 
		data.instance.destroy(); 
		data.instance = null; 
	} else { 
this.log(1, "Stop Module '" + moduleID + "': FAILED : 
module does not exist or has not been started"); 
	} 
}, 
stop_all : function () { 
	var moduleID; 
	for (moduleID in moduleData) { 
		if (moduleData.hasOwnProperty(moduleID)) { 
			this.stop(moduleID); 
		} 
	} 
},
```
接下来的两个方法应该很明显：stop和stop_all。stop方法携带一个module的名字；如果系统知道module的名字还有module正在运行，我们就可以调用module的destory方法，然后设置实例为空。如果module不存在或不在运行，就报告错误。

stop_all的方法和start_all的方法类似，就是在每个module上调用stop函数。

下一步：处理事件

```js
registerEvents : function (evts, mod) { 
	if (this.is_obj(evts) && mod) { 
		if (moduleData[mod]) { 
			moduleData[mod].events = evts; 
		} else { 
			this.log(1, ""); 
		} 
	} else { 
		this.log(1, ""); 
	} 
}, 
triggerEvent : function (evt) { 
	var mod; 
	for (mod in moduleData) { 
		if (moduleData.hasOwnProperty(mod)){ 
			mod = moduleData[mod]; 
			if (mod.events && mod.events[evt.type]) { 
				mod.events[evt.type](evt.data); 
			} 
		} 
	} 
}, 
removeEvents : function (evts, mod) { 
	var i = 0, evt; 
	if (this.is_arr(evts) && mod && (mod = moduleData[mod]) && mod.events) { 
		for ( ; evt = evts[i++] ; ) { 
				delete mod.events[evt]; 
			} 
	} 
},
```
就如我们所知道的，registerEvent 携带事件的一个对象和注册它们的module。再来，我们做一些错误checking（我已经留了错误空白在这个例子里，以简化代码）。如果evts是一个对象，我们也知道我们正在讨论讨论哪个module，我们只是把对象放进module的橱柜moduleData。

当要触发事件，我们就被给予一个有类型和数据的对象。我们会再次循环moduleData 的每个module：如果module有一个事件属性而且事件对象有一个key跟我们执行的事件一致，我们就会调用存储在事件的方法然后把事件的数据传递给它。

移除事件很简单，我们会得到事件的对象（然后在正常的错误审核之后）我们会在它上面循环然后移除在来自module的事件对象的数组上的事件（注意：我想我有一点混淆的截屏，但这是正确的版本。）

```js
log : function (severity, message) { 
	if (debug) { 
		console[ (severity === 1) ? 'log' : (severity === 2) ? 'warn' : 'error'](message); 
	} else { 
		// send to the server 
	}      
},
``` 
这个方法萦绕我们很久了；基本上，如果我们在debug的代码里面，我们就报告错误到控制台；否则我们就传递到服务器。oh，那三元的东西？那只是利用程度的参数来决定用哪个firebug的方法来报告错误：1===console.log，2===console.warn,>2===console.error。

现在我们已经准备好看core的部分，它给sandbox基础的方法；对于这个大多数，分在了一个dom对象，因为我有强迫症。

```js
dom : { 
	query : function (selector, context) { 
		var ret = {}, that = this, jqEls, i = 0; 
 
		if (context && context.find) { 
			jqEls = context.find(selector); 
		} else { 
			jqEls = jQuery(selector); 
		} 
		 
		ret = jqEls.get(); 
		ret.length = jqEls.length; 
		ret.query = function (sel) { 
			return that.query(sel, jqEls); 
		} 
		return ret; 
	},
```
这是我们的第一个方法，query。它携带一个选择器和一个上下文。现在记住，这是个core，我们可以直接指向基础层jquery。在这个情况下，上下文应该是一个jquery对象。如果上下文有一个find方法，我们就可以设置jqEls 为context.find(selector)的结果；如果你很熟悉jquery，你就会发现这个可以得到上下文的子元素；这就是我们是如何得到sandbox.query的方法的！然后我们可以设置调用get方法之后的返回对象；这个返回一个原dom元素的对象。然后，我们给ret长度属性，所有可以循环很容易。最后，我们给它一个jquery的方法：这个方法只能有一个参数，一个选择器，它调用core.dom.query，传递选择器和jqEls作为参数。


```js
bind : function (element, evt, fn) { 
	if (element && evt) { 
		if (typeof evt === 'function') { 
			fn = evt; 
			evt = 'click'; 
		} 
		jQuery(element).bind(evt, fn); 
	} else { 
		// log wrong arguments 
	} 
}, 
unbind : function (element, evt, fn) { 
	if (element && evt) { 
		if (typeof evt === 'function') { 
			fn = evt; 
			evt = 'click'; 
		} 
		jQuery(element).unbind(evt, fn); 
	} else { 
		// log wrong arguments 
	} 
},
```
在这个dom事件有bind和unbind两个方法，我决定提供一个津贴给用户。用户必须传递两个方法，但如果evt参数是一个方法，我们就假设用户给我们留下了一个他想要处理的事件类型。在这种情况下，我们假设一个点击事件，由于它的普遍性。然后，我们使用jquery的bind方法把它绑起来。我需要注明的是因为我们的query方法返回的是dom集合，跟jquery很相似。这个方法可以通过给我们设置成jquery对象，没有任何问题。

我们已经把最后四个方法分组因为它们都非常简单；dom.create简单地返回一个新的dom元素；dom.apply_attrs 使用jquery的attr方法来给元素提供属性。最后，我们已经得到两个helper方法，我们可以使用它来验证我们的参数。

相不相信都好，这就是整个core的内容；我们的base是jquery，我们已经准备好安装它。


Puttting it All Together
现在我们准备建一些html代码来看javascript的行为。我不会那么详细地带着你走，因为这不是教程的重点。
```js
<!DOCTYPE HTML> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <title>Online Store</title> 
    <link rel="stylesheet" href="default.css" /> 
</head> 
<body> 
    <div id="main"> 
        <div id="search-box"> 
            <input id="search_input" type="text" name='q' /> 
            <button id="search_button">Search</button> 
            <button id="quit_search">Reset</button> 
        </div> 
  
        <div id="filters-bar"> 
            <ul> 
                <li><a href="#red">Red</a></li> 
                <li><a href="#blue">Blue</a></li> 
                <li><a href="#mobile">Mobile</a></li> 
                <li><a href="#accessory">Accessory</a></li> 
            </ul> 
        </div> 
  
       <div id="product-panel"> 
           <ul> 
		<li id="1" data-8088-keyword="red"><img src="img/1.jpg"><p>First Item</p></li> 
		<li id="2" data-8088-keyword="blue"><img src="img/2.jpg"><p>Second Item</p></li> 
		<li id="3" data-8088-keyword="mobile"><img src="img/3.jpg"><p>Third Item</p></li> 
		<li id="4" data-8088-keyword="accessory"><img src="img/4.jpg"><p>Fourth Item</p></li> 
		<li id="5" data-8088-keyword="red mobile"><img src="img/5.jpg"><p>Fifth Item</p></li> 
		<li id="6" data-8088-keyword="blue mobile"><img src="img/6.jpg"><p>Sixth Item</p></li> 
		<li id="7" data-8088-keyword="red accessory"><img src="img/7.jpg"><p>Seventh Item </p></li> 
		<li id="8" data-8088-keyword="blue accessory"><img src="img/8.jpg"><p>Eighth Item</p></li> 
		<li id="9" data-8088-keyword="red blue"><img src="img/9.jpg"><p>Ninth Item</p></li> 
		<li id="10" data-8088-keyword="mobile accessory"><img src="img/10.jpg"><p>Tenth Item</p></li> 
            </ul> 
  
        </div> 
          
        <div id="shopping-cart"> 
            <ul> 
            </ul>  
        </div>  
    </div> 
    <script src="js/jquery.js"></script> 
    <script src="js/core-jquery.js"></script> 
    <script src="js/sandbox.js"></script> 
    <script src="js/modules.js"></script> 
</body> 
</html>
```

这没什么特殊的；最重要的事情就是要注意每个主要的div都要id跟javasript module一致。不要忘记html5的data-*属性给我们的分类帅选。

当然了要给它样式：
```js
body { 
    background:#ececec; 
    font:13px/1.5 helvetica, arial, san-serif; 
} 
#main {  
    width:950px; 
    margin:auto;  
    overflow:hidden;  
} 
#search-box, #filters-bar {  
    margin-left:10px;  
} 
#filters-bar ul {  
    list-style-type:none;  
    margin:10px 0;  
    padding:0;  
    border-top:2px solid #474747;  
    border-bottom:2px solid #474747;  
    } 
#filters-bar li {  
    display:inline-block;  
    padding:5px 10px 5px 0; 
} 
#filters-bar li a { 
    text-decoration:none; 
    font-weight:bold; 
    color:#474747; 
} 
#product-panel { 
    float:left; 
    width: 588px; 
} 
#product-panel ul { 
    margin:0; 
    padding:0; 
} 
#product-panel li { 
    list-style-type:none; 
    display:inline-block; 
    text-align:center; 
    background:#474747; 
    border:1px solid #eee; 
    padding:15px; 
    margin:10px; 
} 
#product-panel li p { 
    margin:10px 0 0 0; 
} 
  
#shopping-cart { 
    float:left; 
    background:#ccc; 
    height:300px; 
    width:300px; 
    padding:30px; 
    border:1px solid #474747; 
} 
  
#shopping-cart ul { 
    list-style-type:none; 
    padding:0; 
} 
  
#shopping-cart li { 
    padding:3px; 
    margin:2px 0; 
    background:#ececec; 
    border: 1px solid #333; 
} 
  
#shopping-cart .product_name { 
    display:inline-block; 
    width:230px; 
} 
  
#shopping-cart .price { 
    display: inline-block; 
    float:right; 
}

```

效果不显示啦！！！！！
well，这就是全部啦，但是再让我们做一件事：让我们做一件事在dojo上，来展示怎么去使用core更简单地转换base。。。噢啦







