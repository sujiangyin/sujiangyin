头脑风暴
让我们想一会我们要实现什么。
```js
我们想要一个松散耦合的结构，它的功能分散成独立的module，理想情况是没有module间的依赖关系。当有趣的事情发生时或者中间层对一些信息发生响应时模块会和应用的其他部分交谈。
```
比如，如果我们有一个javascript的应用，它的作用是网上面包店，一个这样“有趣”的消息来自module可能是“42块面包正准备派送”。

我们用一个不同的层解读来自module的消息以至于a)module不直接进入core
b)module不要直接和module交流。这个有助于阻止应用程序发生错误，也提供我们一个方式来启动发生错误的module。

另一个关键的是安全保障。事实上我们大部分人都不知道内部的应用安全是个重大的问题。我们告诉自己我们在构建应用程序，我们足够聪明，因为我们指出了什么是公用的什么是私有的。

然而，如果你有办法决定一个module可以在系统里做什么的时候难道不会有很大的帮助吗？例如，如果我知道我已经在我的系统里限制了权限而不允许一个公开聊天的窗口小部件和一个后台module对接，或者一个有数据库写的权限得到module对接，我们可以限制有人利用我已经在窗口小部件传递一些xss的漏洞的机会。modules不应该有权限进入任何东西。他们可能在大多数当前的架构，但他们真的需要吗？

对有一个中间层处理权限的module可以进入你的框架的一部分可以给你添加保障，这个意味着一个module只能做我们允许它做的那些。


The Proposed Architecture
建筑的答案我们搜索的是三个著名的patterns的结合：the module，facade，和mediator。

相对与传统模型的模块是直接和其他模块交流的特点。在这个解耦的建筑里，他们将只会公布感兴趣的事件（理想状况下，系统没有其他模块的知识。）中介模式将被用来从这些模块中订阅消息，并处理相应的通知应是什么。facade模块将用来限制模块的权限。

我将会更详细地介绍每个patterns

   Design patterns
   Module Theory
   High-level Summary
   Module pattern
   Object literal notation
   CommonJS modules
Facade Pattern
Mediator Pattern

Applying To Your Architecture
   The Facade - Abstraction Of The Core
   The Mediator - The Application Core
   Tying It All Together

module理论
你可能已经使用模块的一些变量在你已经存在的结构中。如果不是，这一节就会呈现一个简短的启迪。
```js
module是一个稳健的应用的建筑的完整的片段，通常是一个更大系统的单一用途部分，是可以互换的。
```
根据你怎么完善modules，为他们定义依赖性可以被自动加载来把其他部分连续连续放到一起变得更有可能。这样想更有扩展性，而不是跟踪他们的各种各样的独立性还有手动加载模块或者注入script标签。

任何好的应用程序都应该建立模块化组件。返回到Gmail来说，你可以考虑模块独立的功能模块，可以存在于自己的，例如聊天功能。被聊天的modules支持，基于功能单元的复杂性，它可能有更多规则的子模块依赖。例如，某人可以简单地有一个modules来处理表情符号的使用，它可以在聊天窗口使用也可以发送到系统的另一个部分。

```js

```





