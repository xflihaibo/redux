##  redux

批处理
事务： 同步线程执行

* Stack reconcile 会深度优先遍历所有的 Virtual DOM 节点，进行Diff。它一定要等整棵 Virtual DOM 计算完成之后，才将任务出栈释放主线程所以，在浏览器主线程被 React更新状态任务占据的时候，用户与浏览器进行任何的交互都不能得到反馈，只有等到任务结束，才能突然得到浏览器的响应。
* react16 :为解决setState 阻塞 而 React Fiber   ReactDOMFiber.render();

### Fiber
* Fiber 是一种轻量的执行线程，同线程一样共享定址空间，线程靠系统调度，并且是抢占式多任务处理，Fiber 则是自调用，协作式多任务处理。

#### Fiber Reconcile 与 Stack Reconcile 主要有两方面的不同。
* 首先，使用协作式多任务处理任务。将原来的整个 Virtual DOM 的更新任务拆分成一个个小的任务。每次做完一个小任务之后，放弃一下自己的执行将主线程空闲出来，看看有没有其他的任务。如果有的话，就暂停本次任务，执行其他的任务，如果没有的话，就继续下一个任务。

#### Fiber整个页面更新并重渲染过程分为两个阶段。
>* Reconcile 阶段。此阶段中，依序遍历组件，通过diff 算法，判断组件是否需要更新，给需要更新的组件加上tag。遍历完之后，将所有带有tag的组件加到一个数组中。这个阶段的任务可以被打断。
>* Commit 阶段。根据在 Reconcile 阶段生成的数组，遍历更新DOM，这个阶段需要一次性执行完。如果是在其他的渲染环境 -- Native，硬件，就会更新对应的元素。