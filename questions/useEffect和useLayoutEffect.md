#### useEffect 和 useLayoutEffect 的区别？
- useEffect 在渲染时是异步执行，并且要等到浏览器将所有变化渲染到屏幕后才会被执行, useLayoutEffect 的 detroy 函数的调用位置、时机与 componentWillUnmount 一致，且都是同步调用。useEffect 的 detroy 函数从调用时机上来看，更像是 componentDidUnmount (注意React 中并没有这个生命周期函数)。
- useLayoutEffect 在渲染时是同步执行，其执行时机与 componentDidMount，componentDidUpdate 一致,从源码中调用的位置来看，useLayoutEffect的 create 函数的调用位置、时机都和 componentDidMount，componentDidUpdate 一致，且都是被 React 同步调用，都会阻塞浏览器渲染

#### DOM 的操作里放到 useLayoutEffect里
- 更新的时候DOM 已经被修改，但浏览器渲染线程依旧处于被阻塞阶段，所以还没有发生回流、重绘过程。由于内存中的 DOM 已经被修改，通过 useLayoutEffect 可以拿到最新的 DOM 节点
- 如果放在 useEffect 里，useEffect 的函数会在组件渲染到屏幕之后执行，此时对 DOM 进行修改，会触发浏览器再次进行回流、重绘，增加了性能上的损耗。

#### 父子组件执行顺序
- Component写法下父子组件的生命周期执行顺序
```
父constructor
父componentWillMount
父render
子constructor
子componentWillMount
子render
子componentDidMount
父componentDidMount
```

- useEffect 可以简单看作是componentDidMount、componentDidUpdate和componentWillUnmount的组合(每次运行effect的同时，DOM都已经更新完毕,所以useEffect的执行顺序是)
```
子effect
父effect
```