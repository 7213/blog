## 架构分层
Rax整体上我们可以将其分层三层：

- **rax**  
提供和宿主无关的对外API，其通过调用`rax-reconciler`来开启vdom的运转
    
    - createElement  
    - createContext
    - render
    - hooks
    - Component、PureComponent
    - ...
     

- **rax-reconciler**   
创建、维护、更新vdom树，其产生第一次挂载的vdom结构和后续更新的diff patch（rax是边diff边patch），最后通过调用`driver`进行实际宿主环境的绘制

- **driver**   
根据rax给出的关于UI操作的 增、删、改、查抽象，宿主进行实现从而实现真实绘制。如driver-dom就是调用dom api进行实际页面操作

