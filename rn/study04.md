#### react-navigation 5.x

新版本的 react-navigation

- 依旧是分为 container 与 stack

```
import {NavigationContainer} from '@react-navigation/native';
import {createStackNavigator} from '@react-navigation/stack';
```

- 但是不在以配置的形式生成 stack，更像是 react-router 的形式

```
const Stack = createStackNavigator(); // 创建stack或者stack
<NavigationContainer>
      <Stack.Navigator
        screenOptions={}  // 可以屏幕设置属性，例如icon
        initialRouteName="Home"  初始路由
      >
        <Stack.Screen name="home"
          component={Home}
          options={{
            headerShown: false, // 去掉header
            initialParams={{itemId: 42}} //初始参数
            headerRight: () => ( // 右侧按钮
              <Button
                onPress={() => alert('This is a button!')}
                title="Info"
                color="#000"
              />
            ),
            headerTitle: props => <LogoTitle {...props} />, // 支持以一个组件代替title
            title: '首页',
            headerStyle: {
              backgroundColor: '#f4511e',
            },
            headerTintColor: '#fff', // 返回按钮和标题都使用这个属性作为它们的颜色
            headerTitleStyle: {
              fontWeight: 'bold', // 如果我们想为标题定制fontFamily，fontWeight和其他Text样式属性，我们可以用它来完成。
            },
          }} />
      <Stack.Navigator>
<NavigationContainer>
```

- 子页面的 props 中含有 navigation, route

```
const {navigation, route} = props;
route.params.itemId // 取值
navigation.navigate('home', {id: 5}) // 跳转，传参
navigation.setOptions({ title: 'Updated!' })} // 更新配置
```

- 底部 tab 导航

```
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';
const Tab = createBottomTabNavigator(); // 创建一个tab
function MyTabs() { // 一个tab页面
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={AAA} />
      <Tab.Screen name="Settings" component={BBB} />
    </Tab.Navigator>
  );
}

<NavigationContainer> // 可以把这个tab当作某一个页面
      <Stack.Navigator initialRouteName="Home" tabBarOptions={{}}>
        <Stack.Screen name="MyTabs" />
    <Stack.Navigator>
<NavigationContainer>

// Navigator-tabBarOptions
// activeTintColor: 设置TabBar选中状态下的标签和图标的颜色；
// inactiveTintColor: 设置TabBar非选中状态下的标签和图标的颜色；
// showIcon: 是否展示图标，默认是false；
// showLabel: 是否展示标签，默认是true；
// upperCaseLabel - 是否使标签大写，默认为true。
// tabStyle: 设置单个tab的样式；
// indicatorStyle: 设置 indicator(tab下面的那条线)的样式；
// labelStyle: 设置TabBar标签的样式；
// iconStyle: 设置图标的样式；
// style: 设置整个TabBar的样式；
// allowFontScaling: 设置TabBar标签是否支持缩放，默认支持；
// safeAreaInset：覆盖的forceInset prop，默认是{ bottom: 'always', top: 'never' }，可选值：top | bottom | left | right ('always' | 'never')；
```

- 主题色

```
const MyTheme = {
    ...DefaultTheme,
    colors: {
      ...DefaultTheme.colors,
      ...theme,
      primary: theme.primaryColor,
    },
  };

  <NavigationContainer theme={MyTheme}>
  </NavigationContainer>

// 这是默认主题色
// primary: 'rgb(255, 45, 85)',
// background: 'rgb(242, 242, 242)',
// card: 'rgb(255, 255, 255)',
// text: 'rgb(28, 28, 30)',
// border: 'rgb(199, 199, 204)',


  const {colors} = useTheme(); //取出颜色使用
```

- redux

```
import {createStore, combineReducers} from 'redux';

import appReducer from './appReducer';

const store = createStore(
  combineReducers({
    app: appReducer,
  }),
);
export default store;

```
