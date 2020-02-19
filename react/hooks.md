## Hooks 学习笔记

#### 注意的知识点

- 组件里有默认参数而且需要根据入参的变化而变化时使用函数 ()=>{} 传参：

```

function App(props) {
  const [count, setCount] = useState(() => {
    return props.defaultCount || 0;
  })
}



```

- setState 可以接一个函数，参数为上一次的 state

```
setState(lastCount=>lastCount-1)
```

- useMemo(() => fn) 等价于 useCallback(fn)

```
区别在于，useMemo用来储存变量，useCallback用来储存函数句柄
const double = useMemo(() => {
  return count * 2;
}, [count === 3]);

const onClick = useCallback(() => {
  setClickCount(clickCount => clickCount + 1)
}, []);
```

- useRef 的两种使用场景
   (1)存储变量或者函数
   (2)相当于 class component 里的 createRef // 不举例子

```
const counterRef = useRef([]);
counterRef.current = params; // 储存变量，每次render依旧保存的是上一次的值

```

```
function useCount(defaultCount) {
  const [count, setCount] = useState(defaultCount)
  const it = useRef(); // 储存定时器，防止每次render后就是新的句柄了
  useEffect(() => {
    it.current = setInterval(() => {
      setCount(count => count + 1)
    }, 1000)
  }, []);
  useEffect(() => {
    if(count >= 10) {
      clearInterval(it.current);
    }
  }, []);

  return [count,setCount]
}
```

- 函数作为参数传递时加 useCallback

```
// 因为hooks中最大的一个坑就是，每次render整个函数都会重新加载，定义函数即使不发生变化，但是句柄将被视为新的句柄， 所以就用到了useMeno， useCallback储存变量与函数

function useSize(){
  const [size, setSize] = useState({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
  });
  const onResize = useCallback(() => {
    setSize({
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    })
  }, []);
  useEffect(() => {
    window.addEventListener('resize', onResize, false);
    return () => {
      window.removeEventListener('resize', onResize, false);
    }
  }, [])
}

```

- function component 有了 hooks 的支持后有了类的功能

- 实现生命周期

```
useEffect(() => {
  //componentDidMount
  return () => {
  //componentWillUnmount
  }
}, [])

useEffect(() => {
  //componentDidUpdate
})

```

- props 与 state

```
function Counter() {
  const [count,setCount] = useState(0);
  const prevCountRef = useRef();
  useEffect(() => {
    prevCountref.current = count;
  })
  const prevCount = prevCountref.current;
  return <h1>Now: {count}, before: {prevCount}</h1>
}

```

#### lazy 懒加载

```
  {
      pathname: PRODUCT_DETAIL_PATH,
      component: lazy(() => import('../views/Product/ProductDetail')),
      exact: true,
      title: '商品详情'
  }
```
由于react的lazy按需引入必须搭配Suspense才能实现懒加载过程
```
import React, { Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch, Redirect } from 'react-router-dom';
import { Spin } from 'antd';
import { HOME_PATH,ADMIN_PATH,ERROR_PATH } from './models/routeConstants';
import { AppRoutes, RouteType } from './routes/routeTable';
const App: React.FC = () => {
  return (
    <Router>
		{/*这里使用了Suspense这个组件，包裹整个路由，并且在路由组件没有加载前加载antd的<Spin className="spin-loading-class" tip="Loading...">Ui组件,从而达到懒加载的过程*/}
      <Suspense fallback={<Spin className="spin-loading-class" tip="Loading..."></Spin>}>
        <Switch>
          {
            AppRoutes.map((route: RouteType) => (
              <Route exact={route.exact} path={route.pathname} key={route.pathname} render={(routePorps: any) => (
                <route.component {...routePorps} />
              )} />
            ))
          }
        </Switch>
      </Suspense>
    </Router>
  );
}
export default App;

```
- 这是在路由中使用懒加载的一个小案例，当然lazy+Suspense的懒加载组合也可以使用在父组件组合子组件的情况下使用，对此就不必做过多介绍，需要的大佬可以前往官网观看组件懒加载的案例与概念

#### useReducer与useContext的结合使用
创建一个store
```
import React, { createContext, useReducer } from 'react';
import reducer from '@/models/reducer';

// 创建 context
export const Context = createContext({});

/**
 * 创建一个 ContextProvider 组件
 */
export const ContextProvider = props => {
  const initData = {};
  const [data, dispatch] = useReducer(reducer, initData);
  return <Context.Provider value={{ data, dispatch }}>{props.children}</Context.Provider>;
};
```
通过context传给App
```
<ContextProvider>
  <App />
</ContextProvider>
```
App里面任何组件都可以取到context,实现一个简易的全局store
```
import { useContext } from 'react';
import { Context } from 'components/GlobalContext';

/**
 * 获取context
 */
export function getContext() {
  const { data, dispatch } = useContext(Context);
  return { data, dispatch };
}
```

#### hooks

- 关于定时器1
直接当作常量定义timer在function中，每次render句柄会改变，定时将无法移除，解决办法，可以将定时器防止function外层定义或者使用useRef

- 关于定时器2
使用setInterval时，在定时函数中如果涉及到setState,不能直接setState(state-1); 这种操作只会改变一次，因为每次定时函数拿到的state都是初始值，不能拿到更新后state-1，应该使用这种办法setState
```
    setState(state=>state-1)
```

