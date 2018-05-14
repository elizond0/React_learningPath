# react_learningPath

## 0.简介

* React起源于Facebook的内部项目，该公司积极尝试引入HTML5技术用来架设Instagram网站，开发中发现HTML5的性能下降明显，达不到预期的效果，就自己开发了React框架。
* 特点:
1. 虚拟DOM: React也是以数据驱动的，每次数据变化React都会扫瞄整个虚拟DOM树，自动计算与上次虚拟DOM的差异变化，然后针对需要变化的部分进行实际的浏览器DOM更新
2. 组件化： React可以从功能角度横向划分，将UI分解成不同组件，各组件都独立封装，整个UI是由一个个小组件构成的一个大组件，每个组件只关系自身的逻辑，彼此独立
3. 单项数据流：React设计者认为数据双向绑定虽然便捷，但在复杂场景下副作用也是很明显，所以React更倾向于单向的数据流动-从父节点传递到子节点。（使用ReactLink也可以实现双向绑定，但不建议使用）

## 1.基础环境

* html中引入react.js和react-dom.js文件即构成基础环境
* React.createClass注册一个组件类(组件名需要首字母大写),通过调用React.createElement('h1', null, 'Hello')生成html内容
* ReactDOM.render是React的最基本方法，用于将模板转为HTML语言，并插入指定的DOM节点

## 2.JSX语法简介

* JSX即Javascript XML,它使用XML标记来创建虚拟DOM和声明组件,方便开发
* 使用JSX语法的支持,需要babel进行转换,demo中暂用babel的核心文件browser.js
* 特点:
1. 使用熟悉的语法仿照HTML来定义虚拟DOM
2. 程序代码更加直观
3. 于JavaScript之间等价转换，代码更加直观
4. 支持在{}中使用二元(||&&)/三元(XXX?XXX:XXX)运算符表达式,不支持if...else

## 3.JSX数组输出

* 详情可看03.html的demo,在此列出一些需要注意的点:
1. 数组遍历生成html的时候,需要设置key,用于dom树更新时作为标记
2. ReactDOM.render是会替换原先的内容的(保留最后一次)
3. JSX允许直接在模版插入JavaScript变量。如果这个变量是一个数组，则会展开这个数组的所有成员。
4. 由于JS文件的编译会规则,在return切勿换行

## 4.React组件：state

* React将界面组件看成一个状态机，用户界面拥有不同状态并根据状态进行渲染输出，用户界面和数据始终保持一致。开发者的主要工作就是定义state，并根据不同的state渲染对应的用户界面。
* react是单向数据流,通知React组件数据发生变化的方法是调用成员函数setState(data,callback)。这个函数会合并data到this.state,并重新渲染组件。渲染完成后，调用可选的callback回调。
* 需要注意的点:
1. getInitialState函数必须有返回值，可以是null,false,对象
2. 访问state数据的方法是”this.state.属性名”
3. 与vue不同,在使用{}传值的时候,没有引号""/'',加了会变成字符串

## 5.React组件：props和render

* props和state的区别:
1. props--父组件传过来的属性(类名,颜色,字体,回调函数等)
2. state--是当前组件状态(通常会改变)
* render创建组件时是必须的,render函数的主要流程是检测this.props和this.state,再返回一个单一组件实例
* render函数应该是纯净的(只涉及定义和显示),不应该(修改组件state,DOM读写操作,浏览器交互等)

## 6.React组件：句柄函数(生命周期)

* 一个组件完整的生命周期包含实例化阶段、活动阶段、销毁阶段三个阶段。每个阶段又由相应的方法管理,主要涉及三个动作:
1. mounting:表示正在挂接虚拟DOM到真实DOM。
2. updating:表示正在被重新渲染。
3. unmounting:表示正在将虚拟DOM移除真实DOM。
* 每个动作术语提供相应函数
1. componentWillMount()
2. componentDidMount()
3. componentWillUpdate(object nextProps, object nextState)
4. componentDidUpdate(object prevProps, object prevState)
5. componentWillUnmount()

## 7.文字闪动练习demo

* 注意事项:
1. 组件render函数用变量生成行内样式时,需要在{}中传入一个对象,例如div style={{opacity:this.state.opacity}}
2. 组件内生命周期使用计时器调用匿名函数时,需要绑定this指向,或者使用箭头函数(因为箭头函数内部没有this,箭头函数内的this指向的是父执行环境里的this,同时也不能用作构造函数)

## 8.React组件：props属性验证

* PropsTypes限定数据类型,例如propTypes:{title:React.PropTypes.string.isRequired},要注意PropTypes首字母大写
* getDefaultProps设置默认值,getDefaultProps:function(){return {title:"hello"}},传入props后被替换

## 9.React组件：获取真实DOM节点

* 获取真实DOM首先需要在标签内定义ref="key"属性,然后通过组件内的方法(this.refs.[key]])进行业务操作

## 10.React表单:事件响应和bind函数复用

* 表单demo详情见10.html,注意事项如下:
1. react组件内:label标签里的for在react里需要替换成htmlFor;select内option默认选中selected需要替换成defaultValue
2. html标签内使用bind(this,[传入参数])进行函数复用

## 11.React表单:name复用

* 使用name复用，表单控件内必须有name属性,react会对event.target.name和组建方法自动绑定

## 12.React表单:可控组件

* React的State有缓存机制，然后原生input也有自己的缓存机制，这两种缓存机制我们在编码中是要进行取舍的。将input中的value绑定到state的React组件就是可控组件，反之则是不可控组件。
* 由于react的渲染策略,构成可控表单组件需要onChange事件,并且将value绑定到state中,通过方法和绑定实现"双向绑定"