Learning AngularJS
================
更新

**注意: 关于Angular 2: Angular 2有很多改变，我可能会在夏天的时候会出一个关于Angular 2的教程！**

***我已经注意到了主文件夹已经变得混乱不堪了，但基于所有的链接都已写死在README中了，所以我就不准备再整理这些文件了。如果有新的教程的话我就会为那个课程创建单独的文件夹，用来存放对应的代码和README - *多谢访问！* ***

![ng!](https://angularjs.org/img/AngularJS-large.png)

虽然我已经学习 AngularJS 有一段时间了，也在还在很多地方看了一些视频但却落下了，从没创建任何的具体的东西来实战。这个项目会帮助我跟踪我在 thinkster.io 和 codeschool.com上俩课程的进度： 

1. [A Better Way to Learn AngularJS](http://www.thinkster.io/angularjs/GtaQ0oMGIl/a-better-way-to-learn-angularjs)  
2. [Shaping up with AngularJS](http://campus.codeschool.com/courses/shaping-up-with-angular-js/intro)  

这个项目也会包含来自其他资源的代码，当然我会在注释以及下边加上我找到他们的引用（链接）。也会有一部分在视频中展示的基础代码示例，这样我们就能准确看到围绕着那个知识点相关的其他示例了。

README也会追踪一些代码、陷阱、注释和其他把戏来一次次的帮助像我一样在学习 AngularJS 的同志们。希望对你有所帮助！

##### 基础知识
- [ ] HTML
- [ ] CSS
- [ ] JS (面向对象、原型、函数、事件、错误处理）
- [ ] Model-View-Controller概念
- [ ] DOM文档对象模型 

##### 依赖
- [ ] Web 浏览器
- [ ] 有 O'Rielly 出版的 AngularJS 书（用AngularJS开发下一代Web应用）。如果你是学生的话，你可以使用你大学的 VPN 在 [这里](http://proquest.safaribooksonline.com/book/programming/javascript/9781449355852/firstchapter) 看。

### 00-概念
AngularJS严重依赖MVC模式：
- **Model** 是包含了在view中展现给用户的以及用于用户交互的数据。（把它看做是我们存储数据的地方）
- **View** 就是用户能看的东西。（用户界面或者DOM）
- **controller** 包含所有的逻辑。

在AngularJS中也使用了一些*术语*：
- **数据绑定** 就是在model和view之间数据的同步。
- **双向数据绑定** 是angular的一个特性，说的是在文档中的一切都是实时的。任何时候只要输入改变了，那么表达式就会自动计算且相应的DOM也会更新。
- **Scope作用域** ~~就是变量和数据（作为model）存储的地方，可以把他作为传统意义上的 *scope作用域*，允许其他所有组件访问在model中的数据。~~ 所以有人指出这个定义时模糊的。深入阅读之后，事实证明Scope作用域就是你的app执行表达式的地方，scope是可以嵌套的且根scope(rootscope)通常不会包含任何东西。不用试图去剥离复杂定义，而是去思考 *一个controller* 的scope 是我们的数据存储的地方，“形成了controller和view之间的粘合剂”。  
- **Templates模板** 所有的带有angular代码的HTML文件都是模板，因为angular必须填充表达式和其他的gadgematics（特殊运算？）。
- **Directives指令** 可以给HTML元素应用特殊行为。例如，在我们的[00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-0-concepts.html)文件中的`ng-app`属性就是链接到了一个指令，他能自动初始化应用。当我们在`<input>`元素上定义`ng-model`属性时，我们创建了一个用于存储和更新这个`<input>`字段的值到*scope*的指令。
- **filters过滤器** 格式化一个用于展现给用户的表达式的值，过滤器很有用，在我们的[00-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-0-concepts.html)文件中我们使用`currency`来格式化类似账单或者货币的表达式的输出内容。

[00-1-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-1-concepts.html)的图片说明：  
![Interaction between Controllers-Scope/Model-View](https://docs.angularjs.org/img/guide/concepts-databinding2.png)  *image from [AngularJs Docs](https://docs.angularjs.org/guide/concepts)*

- **Services服务** 包含所有的“独立逻辑”，换句话说就是一种提供和操作我们的应用数据的方式。我们使用服务来达到我们在应用的其他东方复用的目的。例如在[00-1-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-1-concepts.html)中所有的逻辑都放在了`InvoiceController`中。在[00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html)中我们则会重构我们的代码，将换算逻辑放进在另外一个模块中的服务中。

- **Dependency Injection依赖注入** 在[00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html)中我们看到`ng-app`定义了`invoice-srv-demo`作为这个应用的主模块。在定义这个模块时，我们声明`finance`是一个主模块的依赖。我们也在传入来自finance模块的`currencyConverter`依赖后定义了controller的构造函数。这就是*依赖注入*。

### 00-spin迷惑？
#### 点.
多亏了[这个视频](http://www.thinkster.io/angularjs/axAQatdKIq/angularjs-the-dot)才发现这个坑（疑难杂症）。
```html
	<script>
		function FirstCtrl($scope) {}
		function SecondCtrl($scope) {} 
	</script>
	<div ng-app="">
		<div ng-controller="myCtrl">
			<input type="text" ng-model="message">
			<br /><h1><b>{{message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			<input type="text" ng-model="message">
			<br /><h1><b>{{message}}</b></h1>
		</div>
	</div>
```
如果我们的代码上上边的样子，*scope继承*就被破坏了。我们设置的每一个新的`ng-model`的值`message`都会创建一个新的message的示例，这也就意味着在两个`message`之间不再有交集了。为了避免这种情况，我们给`$scope`加一个带有message的`data`的model。我喜欢以每一个controller都有他自身的scope这样来思考，因为下边展示的修改后的代码也不会修复这个问题，且两个controller之间没有任何数据是共享的。
```html
	<!--脚本部分不变-->
	<div ng-app="">
		<div ng-controller="myCtrl">
			<input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			<input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
	</div>
```
然而照着下边的方式修改代码，整个的应用设置了一个scope，这样就会确保scope能够在两个controller之间同步。
```html
<!--脚本部分不变，我们初始化 data.message 的值为 'hi'-->
	<div ng-app="" ng-init="data.message='hi'">
		{{data.message}}
		<div ng-controller="myCtrl">
			<h1>{{2+2}}</h1>
			Type in a message: <input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
		<div ng-controller="yourCtrl">
			Type in a message: <input type="text" ng-model="data.message">
			<br /><h1><b>{{data.message}}</b></h1>
		</div>
```
*（结果来看确实是正确的了，我们需要有一个父scope来处理这种类型的继承）*。我们现在来寻找另外的方式来做这件事情。
#### controller之间的数据共享
这可以通过创建一个提供数据的*factory*来做。然后我们绑定本地的`$scope`到`Data`factory，然后我们就能再controller之间同步数据了。请看[00-2-spin.html](https://github.com/zafarali/learning-angular/blob/master/00-2-spin.html)。
```javascript
var myApp = angular.module('myApp', []);
myApp.factory('Data', function(){return{message:"this is important"};});
function myCtrl($scope,Data){$scope.data = Data;}
function yourCtrl($scope,Data){$scope.data = Data;}
```
### 购物车 （01-1-shopping 和 01-1-forms）
[购物车](https://github.com/zafarali/learning-angular/blob/master/01-1-shopping.html)示例展示了AngularJS框架的一些其他的功能。或许你已经注意到了我们有两种创建`controller`的方法了（*译注：自从1.3之后第一种全局的默认不支持了，需要`$controllerProvider.allowGlobals()`才可以*）:
```javascript
//方法 1
function Ctrl($scope) {};
//方法 2
var myApp = angular.module('myApp', []);
myApp.controller('Ctrl', function($scope){});
```
在[这个提交](https://github.com/zafarali/learning-angular/commit/ce54e0d417f80a0087035b62e92a6f8030ccd4c0#diff-1d019a5313eee180347d30c7801153a5)我们可以比较两种购物车的应用的使用方法。方法2通过使用我们定义的模块来保证了我们的controller不会暴露在全局命名空间中。
本文档页面关于[forms表单](https://docs.angularjs.org/guide/forms)部分我们认为在[01-01-forms.html](https://github.com/zafarali/learning-angular/blob/master/01-1-forms.html)中完整的体现了整齐的技巧和提示。这个页面也展示了`ng-show`、`ng-hide`的功能和其他AngularJS提供的验证技术。
### 02-Filters过滤器
#### 自定义过滤器
这里有语法需要学习，请看这个[视频](http://www.thinkster.io/angularjs/EnA7rkqH82/angularjs-filters)，然后看我[敲](https://github.com/zafarali/learning-angular/blob/master/02-1-filters.html)的伪代码。下边的另一个例子显示了可以像这样`text|decimal2binary:true`来使用额外的参数：
```javascript
//将10进制转化为二进制字符串
app.filter('decimal2binary', function(){
	function convert(num,bool){
	    if(bool) { console.log('You specified true'); }
        return parseInt(num,10).toString(2);
	}
	return function(input, bool){
		return convert(input, bool);
	}
});
```
#### 搜索和过滤
我们可以在[02-2-filters.html](https://github.com/zafarali/learning-angular/blob/master/02-2-filters.html)中发现一个搜索技术的实现。一些其他的内置的过滤器总结如下：
- `limitTo` 会限制展示的结果数字
- `orderBy` 接收一个字符串参数，然后会使用这个属性的映射结果来排序
- `lowercase` 和 `uppercase` 将结果转化为小写和大写（这些最好应用在如同下边展示的数据绑定上）
- `date`，`currency`会格式化我们的数据，且会自动解译的
- `number` 接收一个整数参数，然后舍入到指定的位数。（例如：传入0就会舍入到没有小数位——0小数位）
```html
<span ng-repeat="element in list | filter: search | orderBy: 'lastname' | limitTo: 4">
{{element.lastname | uppercase}}, {{element.firstname | lowercase}}
</span>
```
### 03-Directives指令
#### 自定义元素
指令是AngularJS提供的最吸引人的特性之一。在我们第一个例子中，我们创建了看一个`restrict`为`'E'`的指令，这意味着他是一个元素。我们的指令很简单，就是通过`<calculator>`元素创建的一个用于显示计算器的图片的东东。这个[提交](https://github.com/zafarali/learning-angular/commit/fa0b6c1864501592e73ef3cea3e47e34c9763942)会展示代码细节。
#### 自定义属性和类
这个[提交](https://github.com/zafarali/learning-angular/commit/403aad17fce1eb09e58b759dad63025db5166cf8)展示了一个我们创建的自定义属性。这里，我们不是设置`restrict:"E"`，我们设置的是`restrict:"A"`且提供了一个`link`函数，这个函数就会在这个属性加上的时候执行。如果我们设置`restrict:"C"`，然后这个指令就会在我们有一个那个名字[（参见提交）](https://github.com/zafarali/learning-angular/commit/552ae91250175137f9e2b742883c7973c463dd48)类的时候执行。我们还会有`restrict:"M"`在注释中使用的指令，如下:
```html
<!--directive:myDirective-->
```
#### 有用的指令
指令默认是`restrict:"A"`。指定也会传入`scope` 和 `element`，然后我们就可以像在[03-2-directives.html](https://github.com/zafarali/learning-angular/commit/d9e6515d2c8dd7b816907f98b9fa8f75472d4956)中展示的那样使用。指令也会传入`attrs`，他就是一个带有这个元素所有属性的对象。当我们想要增加或移除类的时候，我们可以利用它使得我们的代码抽象化。这个在[这个提交](https://github.com/zafarali/learning-angular/commit/a0636adf5790aeeb7d20489133bdc23d7d338578)中展现了。我们也会在[这个例子](https://github.com/zafarali/learning-angular/blob/master/03-3-directives.html)中看到使用`$apply()`来执行函数。
#### 指令的通讯（[03-4-readinglist阅读列表.html](https://github.com/zafarali/learning-angular/blob/master/03-4-readinglist.html)）
指令能够像阅读列表示例中那样来进行通信。这里我们创建了一个叫做`<book>`的指令，且这个`book`指令依赖一些属性。每一个`<book>`都定义了一个本地scope。`<book>`还有一个API用来控制本地scope。我们定义的每一个特性都能访问`<book>`的API来操作他的属性。这些属性能在我们roll over（查看）他的时候看到。在[03-6-elements.html](https://github.com/zafarali/learning-angular/blob/master/03-6-elements.html)中展示了一种精简的方式（虽然没有浪漫的或惊悚等属性）。
- **Transclusion嵌入** 在[03-5-transclusion.html](https://github.com/zafarali/learning-angular/blob/master/03-5-transclusion.html)中我们还展示了一个`transclude`属性。他允许在我们的自定义元素中包含的任何内容都可以在其内部展示且不会被模板覆盖掉。
- 在[03-7-nested.html](https://github.com/zafarali/learning-angular/blob/master/03-5-transclusion.html)中展示了*Nested elements嵌套元素* 以及他们之间如何交流。

#### 指令属性
这里有我认为很有用的属性的总结：
- `restrict:"E"||"A"||"C"||"M"` 这声明了指令如何使用，分别作为一个元素E、特性A、类C或者注释M。我们可以任意组合使用他们。
- `template:"string"` 允许你的指令有一个模板。
- `templateUrl:"string"` 允许你的指令加载一个URL的内容作为模板。
- `replace:bool` 如果是true的话，就会替换掉当前元素，默认是false，这样就会附加到这个元素上。
- `transclude` 允许你将指令的孩子移动到模板内部。我们可以通过在目标位置指定`ng-transclude`属性来做使用。在这个例子中，他的目标就是`<span>`而不是其他的`<div>`：
```javascript
//...
transclude:true,
template:"<div><div><span ng-transclude></span></div></div>",
//...
```
- `link:function()` 当指令设置好之后执行。他在设置`timeout`或绑定`event`事件很有用。
- `controller:function()` 允许你设置scope属性且创建接口用于和其他指令交互。同样在你的指令中的函数只需要在这个指令中执行的时候也很有用。
- `controllerAs: 'String'` 允许你在指令中使用[Controller As](https://docs.angularjs.org/api/ng/directive/ngController)语法。*注意东西是绑定在`this`上的，而不是绑定在了`scope`上。* 更多的细节请参照上边的'controllerAs'语法链接。  
-`scope:{}` 允许你为这个元素创建一个新的scope，而不是从父级那里继承scope。_译注：也就是独立scope_
- `require:['^directives']` 强制当前指令必须嵌套在父指令中来确保他的功能正常。

一个需要注意的坑是，当声明你的指令的时候命名为`datePicker`，你必须是HTML中这样`date-picker`使用。

#### Pre-link链接前, Post-link链接后 和 Compile编译
通过阅读我们知道在一个angular应用创建的时候似乎有两个很重要的阶段。
1. 加载Script（暂时对我们不重要）
2. **compile phase编译阶段** 就是指令注册、应用模板以及执行*compile*函数的阶段。
3. **link phase链接阶段** 一旦模板已经应用了且compile编译函数执行完成之后就进入了。在这个阶段*link*函数就会执行无论view中的数据何时更新。我认为他们之间的主要的区别就是*compile*函数只会在编译阶段执行一次，然而`link`函数则是任何的该指令的实例都会执行一次。现在我还没看到使用编译函数的场景，但是在书中的建议是当你需要对所有的情况下都修改东西（_译注：例如修改模板_）的情况下是很有用的。注意下边的函数定义间的不同之处，`compile`函数不能访问scope，因为那时候还没创建呢。
```javascript
return {
	restrict:"",
	require:"",
	controller:function(scope,element,attrs){},
	compile: function(templateElement, templateAttrs, transclude){},
	link: function(scope, instanceElement, instanceAttrs, controller){}
}
```
如果我们看下[03-8-compilevslink.html](https://github.com/zafarali/learning-angular/blob/master/03-8-compilevslink.html)文件，我们发现log在`controller`和`compile`的`pre`和`post`中执行了，但在link中没执行。尝试各种组合之后，我们发现如果你有一个`compile`的话，你就不能有`link`了，但你可以在`compile`函数中设置一个`pre`和`post`函数来达到和`link`一样效果。 
*更新：我刚看了一个[视频](http://www.thinkster.io/angularjs/h02stsC3bY/angularjs-angular-element)，`link`函数和`compile`函数确实是不能共存的。然而，如果我们确实选择了`compile`函数，我们可以返回（从`compile`函数中）另一个函数，这个函数就会变成`link`函数。上这个网站去看看那个很好的例子，展示了这是怎样工作的。

### 04-Scopes
Scope能嵌套。这些嵌套的scope能是*子scope*（从他的父scope中继承属性）或 *独立scope*（不会继承任何东西）。任何我们想执行的表达式，例如`{{name}}`，通过input的`ng-model='name'`都会挂在*scope*上。因此我们可以认为scope就是controllers/directives和view之间的链接。
#### 独立scope
我们看一个在[04-0-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-0-scope.html)展示简单方式去创建一个独立scope。当我们定义一个指令的时候，我们能返回的是`scope`。他能设置为`true`, `false`, 或 `{/*内部细节*/}`。当我们设置为`false`（默认），他就会使用这个指令中已存在的scope。如果是`true`的话就会从父scope那里继承。`{}`会为每个指令创建独立的scope。当你定义了独立scope，需要有一些*绑定策略*或者方法传入到这个scope中的话，下面就来讨论讨论。
##### @
我们在[04-1-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-1-scope.html)中展示了两种使用属性来存储数据到独立scope中的方法。
##### =
`@`操作符会期望一个字符串来绑定我们希望传入的信息，但`=`会期望一个对象，这允许我们设置在父scope和我们的指令scope之间的双向绑定。[04-2-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-2-scope.html)展示了指令scope怎样更新父scope的。注意在我们的属性中我们现在传入的是对象，而不是字符串。
##### &
`&`操作符会执行你传入的表达式。所以他看起来就是如果你传入的在controller和指令之间的值是在一个对象中的，且这个对象必须映射他的属性到这个函数上。在[04-3-scope.html](https://github.com/zafarali/learning-angular/blob/master/04-3-scope.html)中展示了。_译注：其实就是涉及到执行的是一个函数，怎么传参数的问题，传的参数必须是一个对象，然后这个对象的属性就会映射到你的那个要执行的函数的参数中。_
##### 现在都放到一起了！
[04-4-review.html](https://github.com/zafarali/learning-angular/blob/master/04-4-review.html)封装了所有的我们截止到目前已经讨论的所有的概念。注意当我们使用`=`时，我们假定用户会从相同的书店中订阅，因为我们希望能反映在其他的指令中的变化。
#### 一些注释
* 我们注意到有* **一个** 根scope*，且Angular首先会在当前scope中查找，然后才会去父scope中找，直到到了根scope。展示如下：
```html
<script>
function MyParentCtrl($scope){
	$scope.name='John Doe'; 
	$scope.home='Canada';
}
function MyChildCtrl($scope){
	$scope.name='Jane Doe';
}
</script>
<div ng-app>
<dig ng-controller="MyParentCtrl">
Hello {{name}}! You are from {{home}}
<div ng-controller="MyChildCtrl">
Hello {{name}}! You are from {{home}}
</div></div></div>
```
会输出`Hello John Doe! You are from Canada
Hello Jane Doe! You are from Canada`。这展示了重点。
* 广播和发射`$emit(name, args)`允许一个事件event在当前scope和父scope中执行。`$broadcast(name,args)`会允许一个事件在当前scope和子scope中执行。未来这个会有展示，现在我们可以在这个页面[Scope事件传播](https://docs.angularjs.org/guide/scope#scope-events-propagation)上看到。[可以看下边关于这个的专题了解更多](#Scope通信)
* `Controller as` 语法允许我们通过*别名*来引用controller：
```html
<script> function MyLongCtrlName(){
	this.clickMe = function(){
		alert("You clicked me!!");
	}

	this.name = "Type your name here...";
}</script>
<div ng-app ng-controller="MyLongCtrlName as ctrl1">
<input type="text" ng-model="ctrl1.name">
{{ctrl1.name}} 
<button ng-click="ctrl1.clickMe()">Click this button</button>
</div>
```
看起来代码更加清晰了，且在大型应用中我认为我会在更多的地方使用它。

### 05-其他范例和小技巧
在thinkster.io页面上提供了一些可选的方式来考虑controller和不同的方式来组织angular应用。我在这里简要总结下：
* 这样考虑设置：
```javascript
app.controller("Ctrl", function($scope){
	this.hi = 'Hello';
	this.sayHi = function(){
		alert(this.hi);
	}
	return $scope.Ctrl = this;
});
```
我们现在可以访问this：`<div ng-click="Ctrl.sayHi()">Click</div>`。这提供了更加清晰的controller且很接近于上边的`Controller as`语法。
* 组织。我们可以像这样组织和初始化我们的controller。（但这样filters不好使了就)
```javascript
var app = angular.module('myApp', []);
var myAppComponents = {};

myAppComponents.controllers = {};
myAppComponents.controllers.AppCtrl1 = function($scope){/*stuff here*/};
myAppComponents.controllers.AppCtrl2 = function($scope){/*stuff here*/};

myAppComponents.directives = {};
myAppComponents.directives.mydirective = function(){return {/*stuff here*/}};

app.directive(myAppComponents.directives);
app.controller(myAppComponents.controllers);
```
* 当代码很多的时候，我们需要模块了。我们已经看到我们有一个主的app模块，但实际上我们可以单独创建模块。在[00-2-concepts.html](https://github.com/zafarali/learning-angular/blob/master/00-2-concepts.html)中我们已经展示过了，但我们可以在[05-0-modules.html](https://github.com/zafarali/learning-angular/blob/master/05-0-modules.html)中看到重复使用指令和factory的点子。

* `$index`: 在上个例子中我们已经看到了`$index`，他就是在`ng-repeat`中当前项的次序。

#### 鼠标事件
我们可以利用AngularJS的监听器和`$event`对象一起做一些有意思的事情。指令是按着如下惯例`ng-listener`的，listener可以是`click`、`dbl-click`、`mouseenter`等。在[05-1-event.html](https://github.com/zafarali/learning-angular/blob/master/05-1-event.html)中就是展示的我们怎么使用`$event`来log这个事件的。同样的逻辑，我们可以考虑把`$event`传进一个函数中来处理这个事件。

#### 控制台log
Angular有内置的`$log`方法来log东西。[文档](https://docs.angularjs.org/api/ng/service/$log)总结了很全了，且还有很好的示例来展现。

#### 观察model
`$watch(toWatch, toDo, deepWatch)` 是一个函数，允许你观察你的model的变化。我们必须注意的是执行这个表达式会返回一个函数，如果执行了他的话就可以停止这个监测。例如：
```javascript
$scope.$watch('username', function(){
	if($scope.username=='secret'){
		console.log('You typed in a secret name');
	}
});
```
现在假定我们有`ng-model`输入框来让用户输入`username`内容。当这个值更新为`secret`的时候，信息就会在控制台输出了。`$watch`也能监控函数来监测他们的函数的返回值。`deepWatch`参数是一个布尔值。如果为true的话，且监测的是一个带有对象项的数组的话，这个对象的每一个属性都会去监测是否发生了改变。

#### Angular类（HTML Class）
Angular会自动加上一个类到一些元素上，这取决于他如何使用。这里一些使用情况：
* `ng-invalid`/`ng-valid` 会在input元素上，这取决于传入的文本是否通过验证。
* `ng-pristine`/`ng-dirty` - 如果用户没有和一个input严肃进行交互的话，就会有`ng-pristine`类，如果交互了就会有`ng-dirty`类。
* `ng-binding` 给定了任何元素上都有数据绑定。
* `ng-scope` 给定了一个元素有新的scope。
* `ng-class` 是一个元素的属性。我们传进去一个对象，key就是类的名字，而value值就是条件。下边示例：
```javascript
<div ng-class="{ showClass: whenThisIsTrue, showErrorClass: whenErrorIsTrue, neutralClass: !whenThisisTrue && !whenErrorIsTrue }">...</div>
```
查看更多，[这里](https://docs.angularjs.org/guide/css-styling)

#### element()
AngularJS实现了jqLite，他是一个轻量级的jQuery。因此如果你在你的页面中正在使用jQuery的话，你就可以把他当做jQuery元素对待，如果没有，那么只能使用jqLite的API来交互。我们可以在[the Angular Documentation](https://docs.angularjs.org/api/ng/function/angular.element#angular-s-jqlite)看那些jQuery的方法是在jqLite中支持的。  
一些小tip：
* 不要在controller中使用`element`，因为controller只包含view的逻辑。
* 尽量避免使用jQuery。如果我们必须使用的话，这里有一个[视频](https://www.youtube.com/watch?v=bk-CC61zMZk)，他建议我们只在一个指令的link函数中用jQuery操作。*我还没尝试过*

### 06-模板
模板是一种简单的方式来组织你的代码。[06-0-templateurl.html](https://github.com/zafarali/learning-angular/blob/master/06-0-templateurl.html)展示了怎样利用一个指令的`templateUrl`属性来链接一个局部的文件到我们的主view中。注意使用了`templateUrl`我们就必须保证在服务端有这些文件。如果我们没有单独的.html文件的话，我们可以使用`$templateCache`来加载html字符串缓存，这样来模拟这样的效果。然后我们可以执行`app.run()`函数。
我们可以通过`get`函数来得到`$templateCache`中的内容，同样可以使用`put`函数来插入一些东西。在[06-1-templatecache.html](https://github.com/zafarali/learning-angular/blob/master/06-1-templatecache.html)示例中展示了。

### 07-路由
*这需要你有一个在跑的server，在Mac上可以简单的在bash命令行中执行`php -S localhost:8080`*
#### `app.config`函数
在我们的应用的配置config阶段，我们可以使用一些已知的providers对象。他们生成的是service的实例。现在这不是重点，但是基本的我们需要知道在controller中，设置`.config`的时候什么是可用的。
#### `ng-view` 和 `$routeProvider`
`ng-view` 是一个扮演着Angular能加载view的窗口。在`config`函数中，我们注入`$routeProvider`，然后可以配置不同的view以及对应view的controller。如果你之前从没见过路由额话是很难理解的，但是语法很简单。
我们调用`$routeProvider`对象的`.when(pg,{templateUrl:templatePage,controller:pageCtrl})`方法。`pg`字符串就是页面的url长的样子。`templatePage`和`pageCtrl`分别是匹配这个页面的我们想加载进view的字符串和那个对象相关联的controller。在[07-0-ngview.html](https://github.com/zafarali/learning-angular/blob/master/07-0-ngview.html)中展示。 
我们可以链式调用`.when()`方法（大多数人都知道这个通用方法）。还有一个`.otherwise()`方法，他会在没有找到匹配的时候对应的默认页面。在[07-1-routing.html](https://github.com/zafarali/learning-angular/blob/master/07-1-routing.html)中有展示。

**注意：视频中以及在书中的信息稍稍有些过时了。AngularJS不再内置`$routeProvider`了。我们必须将`ngRoute`模块注入到我们的应用中，且需要加上这个模块的对应的链接。参见 07-0-ngview.html 示例**

#### `$routeParams`
当我们把`$routeParams`对象注入进controller的时候，我们就可以访问来自route路由的参数了。我们按照如下形式给`$routeProvider`定义参数：
```javascript
app.config(['$routeProvider', function($routeProvider){
	$routeProvider.when('/path/:message',{
				templateUrl:myUrl,
				controller:myCtrl
			});	
}])
```
我们使用特殊的如同`:message`的语法来表明这是一个参数。因此我们可以利用`#/path/X`加载多个url，X就是我们希望传入的信息。一旦配置了，我们就可以在controller中通过使用`$routeParams.message`来获取了。这里我们必须注意到`:parameter` 和 `$routeParams.parameter`中，参数必须匹配才能得到正确的信息。在[07-2-routeParams.html](https://github.com/zafarali/learning-angular/blob/master/07-2-routeParams.html)中有展示。使用相同的观念，我们能传入多个参数，如下：`.when('/:parameter1/:parameter2/:parameter3', ...)`且能使用`$routeParams.parameter1`等等访问的到。

#### 动态处理
我称之为动态处理（听起来更酷），但实际上只需要用一个函数来返回一个动态的URL来替代之前的展示给用户一个字符串。这里的关键就是给`.when()`方法传入的对象使用`redirectTo`属性。
设想这样的场景，你有一个'MyAmazingBlog'的博客，且已经多年使用`#/blog/postid`，但是你想开始一个新博客'ExtraBlogStuff'，当然还希望将用户重定向到正确的URL。我们可以照如下写一个函数：
```javascript
app.config(['$routeProvider', function($routeProvider){
	$routeProvider.when('/blog/:blogname/:blogpost',{

		//对于MyAmazingBlog或ExtraBlogStuff博客而言，都返回一个正确的模板。
		//如果没有指定的话就会移动到下一个//the next .when()，
		templateUrl:function(routeParams){
		return routeParams.blogname+'.html'; },
		controller:'BlogCtrl'
	})
	.when('/blog/:blogpost', {
		//这里我们重定向我们的老的用户到新的博客
		redirectTo:function(routeParams){
		return '/blog/MyAmazingBlog/'+routeParams.blogpost;
	});
}]);
```
我希望这个例子已经清楚的展示了一个使用的示例，以及怎样正确去使用'$routeParams' 和 '$routeProvider' 对象。
*注意：其他的我们能给在路由provider中的函数传进去的参数如下：*
```javascript
function(routeParams, path, search){
	//routeParams上边我们已经看到了
	//path 是完整的路径url
	//search 返回的是路径url后面的查询
	//i.e. ?query=true&message=what as a key:value object
	return '/'; //必须返回字符串！
}
```
#### Promises
Promises是一种执行链式调用的方式。有些人说他避免了一个函数传进另一个函数这样的嵌套地狱。（其他好处包括：更清晰代码来进行错误处理）
Angular通过一个叫做`$q`的库提供promise。记住，关键的就是我们会处理一个`defer`对象，我们能传入一系列的函数*（生成promises）* ，然后调用`resolve()`方法来执行他们。  
在[07-3-promises.html](https://github.com/zafarali/learning-angular/blob/master/07-3-promises.html)中我们展示了promise对象的基本功能以及一个略微更复杂的使用传入参数的例子。这里也有一种处理错误的方式，如下代码所示：
```javascript
defer.promise
	.then(function(name){
		return name.split('').reverse().join('');
	})
	.then(function(reversedName){
		//do some more processing
		return processedName;
	})
	.then(function(display){
		console.log(display);
	}, function(error){
		//处理之前步骤中的任何错误
	});
```

#### $routeProvider的resolve属性
除了给`.when()`函数的第二个对象参数传入`templateUrl` 和 `controller`之外，我们还可以传入`resolve`，他是一系列的需要在controller实例化之前被resolved的promises。这允许我们在加载view之前加载所有的必须数据。[这个提交](https://github.com/zafarali/learning-angular/commit/2192e8cecc2a39386b9954ff165aaabc17771a79)展示了，为了更清晰，我重申下这个实验：
1. **URL: #/** 这个模板永远不会加载。这是因为我们的promise没有resolve。  
2. **URL: #/sup** 这个模板立即加载了，因为我们resolve了promise，且直接返回了。
3. **URL: #/bye** 这个模板会在3秒后加载。这是因为我们使用了`$timeout`（`setTimeout()`的包装）在3秒后`resolve()`了promise。

这个[视频](http://www.thinkster.io/angularjs/6cmY50Dsyf/angularjs-resolve-conventions)展示了一些当我们使用在`$routeProvider`使用resolve属性时的常规用法。重申下，所有的传进resolve属性的函数会作为属性自身增加到controller中。这个视频也指出了一旦view加载了，我们就能通过`$route.locals.nameOfResolvePromise`来访问resolve的promises的结果了。

#### 处理 `$resolveChangeError`
*如果你的promise没有resolve，且抛出错误了会发黄是呢过什么？*  
不管我们怎样尝试做，我们的应用永远不会加载任何的东西，也不会展示任何的东西。在这种情况下，我们设置另一个默认的controller来处理这种情况。[07-5-ChangeError.html](https://github.com/zafarali/learning-angular/blob/master/07-5-ChangeError.html)展示了，确保你会仔细读注释来理解发生了什么！ 
这个[视频](http://www.thinkster.io/angularjs/8u6fK7qWYv/angularjs-directive-for-route-handling)建议我们创建一个指令来处理`$routeChangeError`。这就交给你去点进去，然后看看到底是怎么做的。

#### `$location`
`$location`是一个围绕`window.location`方法/对象的封装。他提供了一些遍历的获取和设置方法，不管可不可以使用HTML5的方法你都可以使用他们。由于他是由AngularJS提供的，当一些东西改变的时候他自己就知道了（我们不需要`watch()`他）。
一些方法：  
1. `absUrl()` 返回包含path的完整的URL  
2. `host()` 返回host (i.e www.host.com)  
3. `path()` 返回当前app正在的那个path  
4. `search()` 允许我们得到传入的query的键值对。
5. `url()` 返回的是path和query参数。 
*注意`path()`、`search()`和`url()` 对于相同的属性也可以用于设置。*  

- [ ] To Do：路由生命周期

### 和服务端通信
任何和服务端以及javascript打过交道的人都会遇到AJAX以及使用 `XMLHttpRequest()` 方法从服务端获取数据。AngularJS通过如下示例中展示的封装使得处理他们这些对象很容易。
#### `$http`
```javascript
$http.get('api/user/', {params:{id:'5'}}
		).success(function(data,status,headers,config){
			//continue the application
		}).error(function(data,status,headers,config){
			//deal with the error here
		});
```
正如你看到的漂亮的封装了我们需要经常重复使用的功能。上边所有的返回的类型都是promise对象。`$http`对象调用时传入一个`config`对象，格式如下：
```javascript
var config = {
	method: "GET" || "POST", //...
	url: "urlOfTarget",
	params: [{key1:'value1', key2:'value2'}], //会变成 ?key1=value1&key2=value2
	data: 'string'|| object, //..传到服务器上的
	headers: object, //之后定义
	cache: boolean, //缓存数据
	timeout:int //过期时间 _译注：也可以是promise_
}
```
还有一些其他的配置属性，这里为了不做过多讨论，所以就保持他们简单了。
这里还有一些其他的`$http`提供的简写方法：  
1. `$http.get(url, [config])`  
2. `$http.post(url, data, [config])`  
3. `$http.put(url, data, [config])`  
4. `$http.delete(url, [config])`  
5. `$http.jsonp(url, [config])`  
6. `$http.head(url, [config])`  
`$resource`对象是另一个常用于和一个API交互的方法。在下段落中阐述。

#### `$resource`
*需要依赖ngResource模块*
`$resource`是一个对于使用一个API的封装。我们通过调用`$resource(url, parameters, actions, options)`创建一个resource  
```javascript
var UserProfile = $resource('/api/user/:userID', 
//get the id from a passed in string.
{userID:'@id'},
{
	//自定义一个action
	getBalance: {method:'GET',params:{showBal:true}}
});
```
如同操作其他用户定义对象一样来操作这个对象：
```javascript
var users = UserProfile.query(function(){
	//this does: 
	//GET: /api/user/
	//the server returns:
	//[{id:1, name:'Jane'}, {id:2, name:'John'}];

	//we can access specific instaces of the cards as we refer to arrays. I.e users[0], users[1] ...
	var user = users[0]
	user.name = "Doe" //overrides the object property 'name' to Doe.
	user.$save();
	//this does:
	//POST: /api/user/1 {id:1, name:'Doe'},
	//server returns the above.

	user.$getBalance();
	//does a GET: /user/1?showBal=true
	
});
```
我们甚至可以像处理普通对象一样来创建新的实例：
```javascript
var newUser = new UserProfile({name:"James Bond"});
newUser.$save();
```
这些函数/方法我们都可以不经过任何修改来使用：
1. `.get()` 处理GET请求
2. `.$save()` 处理POST请求  
3. `.query()` 处理GET请求且返回一个数组
4. `.$remove()` 处理DELETE请求  
5. `.$delete()` 处理DELETE请求
获取和删除的方法们`.get()`、`.query()`、`.$remove()`以及`.$delete()` 能够传入一个带有`(value,headers)`的回调函数以及一个带有`httpResponse`参数的错误回调函数。
一个完整的示例可能是这样的：`UserProfile.get({id:1}, function(data){/*成功之后做什么事情*/}, function(response){/*处理失败*/})`。  
设置`.$save()`函数被调用时可以传入一些发送的数据，且也会有回调模式。完整示例：
`Notes.$save({noteId:2, author:'Camillo'}, "This is an amazing note wow", successCallback, errorCallback)`。

1. **url** 包含了一个参数化的我们要去与之交互的URL版本。例如他能是：`http://www.myexample.com/data.json` 或者 `http://www.myexample.com/api/user/:id`。
2. **parameters** 设置了我们将要传入到对象中的默认参数。以我来看最可能使用的是`@`参数。后边细说。
3. **options** 后边讨论。
4. **actions** 扩展`$resource`对象的功能。上面已经展示过了，但这里为了更清楚来看另一个例子：
```javascript
app.factory('Notes', ['$resource', function($resource){
	return $resource('api/notes/:id', null, {'update':{method:'PUT'}
	});
}]);
Notes.update({id:'2'}, "This is amazing!");
```
把`Notes`factory注入到我们的controller之后，我们就可以调用`Notes.update({id:id}, data);`

这样我们的代码就能更容易处理，通过仅仅处理对象而不是去处理重复的url的实例。

##### 注意跨域请求
你可能不知道Javascript不能发一个其他域的AJAX请求。也就是说你的网站（假设你不在twitter工作）不能ping twitter来获得推文。但是，却有另外一个方法*JSONP*。（我发现在这之后Twitter不再提供这个API了除非你认证过了，所以我觉得使用[Meetup API for cities](http://www.meetup.com/meetup_api/docs/2/cities/)）。在[08-1-ngResource.html](https://github.com/zafarali/learning-angular/blob/master/08-1-ngResource.html)例子中我用JSONP重写了resource对象的通常的GET方法，这样我就能访问Meetup的API了，且能得到最近cities数据。需要注意的是并不是所有的网站都提供了可用的JSONP的API。这个页面是根据来自[egghead.io](https://www.youtube.com/watch?v=IRelx4-ISbs)的教程来做的。[Meetup Cities API](http://www.meetup.com/meetup_api/docs/2/cities/)。**如果想知道到底发生了什么，请看08-1-ngResource.html中的注释**   

上面的例子是我在网上发现的另一篇有关`ngResource`模块的文章介绍的，但也不能完全展示他的能力。实际上当我们有`.get()`、操作且`.$save()`到服务器上的对象的时候就应该使用`$resource`。

### 需要知道的有趣的东西
我们之前涉及了很枯燥的factory, provider, module, routing 细节，现在我觉得这里有一些不错的东西可以来休息下。
#### `ngAnimate`
在AngularJS中的动画需要我们注入一个名叫`ngAnimate`的特殊的模块，他会给元素增加一些特殊的class，这样就能通过特殊的方式来做动画了。在[08-0-ngAnimate.html](https://github.com/zafarali/learning-angular/blob/master/08-0-ngAnimate.html)中我们看到三种单独的怎样使用CSS来做动画的情况。
这里关键需要记住的就是当动画执行的时候（也就是transitions和animations），元素就会有`.ng-enter-active`的类class。

#### Scope通信
我们之前已经看到了几种使用`$watch`来watch观察events/changes的方式，但这里呢我们介绍另一种周知的`$on`。他监听了一个由名字的事件。例如：
```javascript
scope.$on('myEventName', function(event, param1, param2, ...) {
	console.log(param1 + ' ' + param2);
	scope.doEvent(param1, param2);
});

//这样来触发上边的事件
scope.$emit('myEventName', 'Hello', 'World');
//或者
scope.$broadcast('myEventName', 'Bye', 'World');
```
`$emit` 和 `$broadcast`之间有啥区别呢？如同之前提到的那样`$emit`会向上传播事件，所有在父scope的controller中有监听`myEventName`的地方都会触发。`$broadcast`正好相反，他会把这个事件向下传播。注意这些事件都会在他们自身的scope中执行。 

新的例子[08-2-onEmitBroadcast.html](https://github.com/zafarali/learning-angular/blob/master/08-2-onEmitBroadcast.html)展示了这种情况。记住声明了一个新的controller就会自动创建一个新的scope。这个页面也展示了继承了的scope以及重写属性。我意识到了这是AngularJS的最强大的特性之一。

### Forms表单
*本段内容（表单）是基于[Shaping Up With AngularJS](http://campus.codeschool.com/courses/shaping-up-with-angular-js/)课程的*  

还记得我们上边讨论的[Angluar Classes](https://github.com/zafarali/learning-angular#angular-classes)吗？AngluarJS提供了一些其他的很好的技巧和特性来更容易的做表单验证。让我们一步一步的来看，且会在后边做一个示例：
- `ng-submit` 是`<form>`元素的一个属性，当用户点击'submit'按钮的时候，其中的函数就会被触发。当然还可以选择把这个函数放到`<button>`元素的`ng-click`中。 
- `formname.$valid` 是一个在全局scope下的一个可用对象，formname就是`<form>`元素的`name`属性的值。从这里咱们可以看出来AngluarJS做了很多幕后工作，且会自动验证表单。返回`true` 或者 `false`。
- `$valid`和Angluar Classes结合我们就能做很有意思的表单了！参见[这里示例](https://github.com/zafarali/learning-angular/blob/master/09-0-forms.html)。

### `$compile` 和可变模板
我们利用了AngularJS所有的美好的东西组成了我们的web app，所有的东西都已经渲染好了，且能做一些生动的交互了。最近我遇到了一个当用户点击一些东西的时候，不更改view/刷新页面然后注入新的带有Angular的HTML内容到页面上的问题。我尝试着用`$scope.$apply()`或者`$scope.$digest()`，但是都没用。然后我发现Angular根本不知道我们插入到页面上的那部分新的HTML内容。我发现一个`$compile`的函数，他能够帮我们来完成需要的功能。

在[09-1-compile.html](https://github.com/zafarali/learning-angular/blob/master/09-1-compile.html)中展示的就是我在页面load之后注入了一些HTML的例子。有一些东西可能会迷惑我们：
```javascript 
	//.....
	//编译分为两步：
	var compiledStuff = $compile(myHTML);
	//这返回了一个必须绑定到一个scope的函数
	compiledStuff($scope);
```	
现在我们就可以使用`$compile`加载我们页面需要的任意的HTML了。但需要注意的是，如果AngularJS提供了其他选择的话，推荐使用它。

### Providers 和 Injectors注入器（高级）
- [ ] 需要完成：  
#### Providers  
#### Injectors注入器
