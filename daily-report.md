## 2020年8月4日
### 今日一题 
**webpack 中 loader 和 plugin 的区别是什么？**
> While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks like bundle optimization, asset management and injection of environment variables.

* loader是一个转换器，将A文件进行编译成B文件，比如：将A.less转换为A.css，单纯的文件转换过程。  
* plugin是一个扩展器，它丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务，比如资源管理、bundle文件优化等操作
### React生命周期
React组件的生命周期根据广义定义描述，可以分为挂载、渲染和卸载这几个阶段。当渲染后的组件更新时，我们会重新去渲染组件，直至卸载。
因此，我们可以把React生命周期分成俩类。
* 当组件在挂载或卸载时；
* 当组件接手新的数组时，即组件更新时。
```javascript
import React, { Component, PropTypes } from 'react';

class App extends Component {
    static propTypes = {
        //...
    };
    static defaultProps = {
        //...
    };
    constructor(props) {
        super(props);
        this.state = {
            // ...
        };
    }
    componentWillMount() {
        // 将要挂载
    }
    componentDidMount() {
        // 已经挂载
    }
    componentWillUnmount() {
        // 将要卸载
    }
    render() {
        return <div>This is a demo.</div>
    }
}
```
`propTypes`和`defaultProps`分别代表`props`类型检查和默认类型。这俩个属性就被声明成静态属性，意味着从类的外面可以这么访问：`APP.propTypes`和`APP.defaultProps`。  
`componentWillMount`方法在`render`方法之前执行，而`componentDidMount`方法在`render`方法之后执行，分别代表渲染前后的时刻。
这个初始化过程没啥特别的，包括读取初始`state`和`props`以及俩个组件生命周期方法`componentWillMount`和`componentDidMount`，这些都只会在组件初始化时运行一次。  
如果我们在`componentWillMount`中执行`setState`方法，又会发生什么呢？组件会更新`state`，但组件只渲染一次。因此，这是无意义的执行，初始化时的`state`都可以放在`this.state`。  
如果我们在`componentDidmount`中执行`setState`方法，又会发生什么呢？组件会再次更新，不过在初始化过程中就渲染了俩次组件，这并不是一件好事情，但实际情况是，有一些场景不得不需求`setState`，比如计算组件的位置或宽高时，就不得不让组件先渲染，更新必要的信息后再次渲染。  
`componentWillUnmount`方法中，我们常常会执行一些清理的方法，如事件回收或是清除定时器。  
更新过程是指父组件向下传递`props`或组件自身执行`setState`方法时发生的一系列动作。  
如果组件自身的`state`更新了，那么会依次执行`shouldComponentUpdate`、`componentWillUpdate`、`render`和`componentDidUpdate`。
`shouldComponentUpdate`是一个特别的方法，它接收需要更新的`props`和`state`，让开发者增加必要的条件判断，让其在更新时更新，不需要时不更新。因此，当方法为`false`的时候，组件不再向下执行生命周期方法。  
## 2020年8月5日
### 今日一题 
**请解释React中props和state的区别？**
props（“properties” 的缩写）和 state 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的几个重要的不同点就是：
* props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）。  
* props 是不可修改的，所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。 由于 props 是传入的，并且它们不能更改，因此我们可以将任何仅使用 props 的 React 组件视为 pureComponent，也就是说，在相同的输入下，它将始终呈现相同的输出。  
* state 是在组件中创建的，一般在 constructor中初始化 state。  
* state 是多变的、可以修改，每次setState都异步更新的。

### Array.from() 方法
从一个类似数组或者可迭代对象创建一个新的，浅拷贝的数组实例
```javascript
console.log(Array.from('hello')) // (5) ["h", "e", "l", "l", "o"]
Array.from([1,2,3], index => {console.log(index)})
Array.from({length: 5}, (v, i) => i); // (5) [0, 1, 2, 3, 4]
```
### JS解析url查询参数
```javascript
const url = 'www.u.com/home?id=2&type=0&dtype=-1';
const parseUrl  = (url) => {
	const query = url.split('?')[1];
	const paramsArr = query.split('&');
	const result = {}
	paramsArr.forEach(param => {
		const temp = param.split('=');
		result[temp[0]] = temp[1];
	})
	return result;
}
parseUrl(url); // {id: "2", type: "0", dtype: "-1"}
```
### React 
#### vsCode 协助写代码插件 Live Share
#### 创建React
```
npm(cnpm) i create-react-app -g  
create-react-app xxx  
cd xxx  
npm start  
```
## 2020年8月6日
### 今日一题 
** 浏览器的本地存储(1)的cookie了解多少？**
**定义**：HTTP cookie，通常直接叫做cookie，是客户端用来存储数据的一种选项，它既可以在客户端设置也可以在服务器端设置。cookie会跟随任意HTTP请求一起发送。
**优点**：兼容性好
cookie 将信息存储于用户硬盘，因此可以作为全局变量，这是它最大的一个优点。它最根本的用途是 Cookie 能够帮助 Web 站点保存有关访问者的信息，以下列举cookie的几种小用途。

* 保存用户登录信息。这应该是最常用的了。当您访问一个需要登录的界面，例如微博、百度及一些论坛，在登录过后一般都会有类似"下次自动登录"的选项，勾选过后下次就不需要重复验证。这种就可以通过cookie保存用户的id。
* 创建购物车。购物网站通常把已选物品保存在cookie中，这样可以实现不同页面之间数据的同步(同一个域名下是可以共享cookie的)，同时在提交订单的时候又会把这些cookie传到后台。
* 跟踪用户行为。例如百度联盟会通过cookie记录用户的偏好信息，然后向用户推荐个性化推广信息，所以浏览其他网页的时候经常会发现旁边的小广告都是自己最近百度搜过的东西。这是可以禁用的，这也是cookie的缺点之一。  
**缺点**：
* 安全性：由于cookie在HTTP中是明文传递的，其中包含的数据都可以被他人访问，可能会被篡改、盗用。
* 大小限制：cookie的大小限制在4KB左右，若要做大量存储显然不是理想的选择。
* 增加流量：cookie每次请求都会被自动添加到Request Header中，无形中增加了流量。cookie信息越大，对服务器请求的时间也越长。
[精选文章](https://segmentfault.com/a/1190000004743454)
