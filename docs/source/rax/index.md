## 架构分层
Rax逻辑上可以分为三层：

- **rax**  
提供和宿主无关的对外API，其通过调用`rax-reconciler`来开启vdom的运转
    
    - createElement  
    - createContext
    - render
    - hooks
    - Component、PureComponent
    - ...
     

- **rax-reconciler**   
创建、维护、更新`vdom`，第一次挂载生成`vdom结构`，更新通过遍历`vdom结构`逐层比较props和state在更新、重建有区别的`子vdom`并生成patch（rax是边diff边patch），patch通过调用`driver`进行实际宿主环境的绘制

    在rax内部`vdom`就是是由以下几种类型的节点形成的树结构:         
    1.NativeComponent[源码](https://github.com/alibaba/rax/blob/master/packages/rax/src/vdom/native.js)         
    在`JSX`中返回的原生标签将生成一个NativeComponent实例节点,如`<div></div>` 
    2.CompositeComponent[源码](https://github.com/alibaba/rax/blob/master/packages/rax/src/vdom/composite.js)     
    我们写的自定义的组件（包括函数式组件）将生成一个CompositeComponent实例节点,如 `<Header />`
    3.TextComponent[源码](https://github.com/alibaba/rax/blob/master/packages/rax/src/vdom/text.js)      
    纯文本节点将生成一个CompositeComponent实例节点,如 `Hellow World`
    4.FragmentComponent[源码](https://github.com/alibaba/rax/blob/master/packages/rax/src/vdom/fragment.js)      
    `<></>`或`[]`将生成一个FragmentComponent实例节点
    5.FragmentComponent[源码](https://github.com/alibaba/rax/blob/master/packages/rax/src/vdom/empty.js)      
    `null`、`undefined`、`true`、`false` 将生成一个EmptyComponent实例节点，它主要用来占住当前位置，当下次更新成一个实际渲染节点时可以找到要挂载的index

- **driver**   
根据rax给出的关于UI操作的 增、删、改、查抽象，宿主进行实现从而实现真实绘制。如driver-dom就是调用dom api进行实际页面操作

