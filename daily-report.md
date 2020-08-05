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