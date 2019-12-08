#### react-navigation

- react-native 0.61
- react-navigation 4.0.10
  安装配置参考[官方文档](https://reactnavigation.org/)

##### 配置流程

```
// 导航配置---类似于react的路由文件
import {createStackNavigator} from 'react-navigation-stack';
import {createAppContainer} from 'react-navigation';
import {Button} from 'react-native';
import React from 'react';
import HomeScreen from '../page/Home';
import Page1 from '../page/Page1';
import Page2 from '../page/Page2';

const Navigator = createStackNavigator({ // 对导航的配置
  Home: {
    screen: HomeScreen,
  },
  Page1: {
    screen: Page1,
    navigationOptions: ({navigation}) => ({
      title: 'page1页面',
    }),
  },
  Page2: {
    screen: Page2,
    navigationOptions: props => {
      const {
        navigation: {
          state: {params}, // 跳转时的参数，类似this.props.history.push({pathName: '',state: {}, // 相当于这里的参数，在params中})
          setParams, // set参数的一个方法
        },
      } = props;
      return {
        title: 'page2',
        headerRight: ( // 右按钮
          <Button
            title={params.mode === 'edit' ? '保存' : '编辑'}
            onPress={() => {
              setParams({mode: params.mode === 'edit' ? '' : 'edit'});
            }}
          />
        ),
      };
    },
  },
});

export default createAppContainer(Navigator);
```

##### 配置参数

```
props.navigation.navigate('Page2', {mode: ''}); // 手动跳转
props.navigation.goBack(); // 手动返回

// 配置
navigationOptions：配置StackNavigator的一些属性。
title：标题，如果设置了这个导航栏和标签栏的title就会变成一样的，不推荐使用
header：可以设置一些导航的属性，如果隐藏顶部导航栏只要将这个属性设置为null
headerTitle：设置导航栏标题，推荐
headerBackTitle：设置跳转页面左侧返回箭头后面的文字，默认是上一个页面的标题。可以自定义，也可以设置为null
headerTruncatedBackTitle：设置当上个页面标题不符合返回箭头后的文字时，默认改成"返回"
headerRight：设置导航条右侧。可以是按钮或者其他视图控件
headerLeft：设置导航条左侧。可以是按钮或者其他视图控件
headerStyle：设置导航条的样式。背景色，宽高等
headerTitleStyle：设置导航栏文字样式
headerBackTitleStyle：设置导航栏‘返回’文字样式
headerTintColor：设置导航栏颜色
headerPressColorAndroid：安卓独有的设置颜色纹理，需要安卓版本大于5.0
gesturesEnabled：是否支持滑动返回手势，iOS默认支持，安卓默认关闭
screen：对应界面名称，需要填入import之后的页面
mode：定义跳转风格
card：使用iOS和安卓默认的风格
modal：iOS独有的使屏幕从底部画出。类似iOS的present效果
headerMode：返回上级页面时动画效果
float：iOS默认的效果
screen：滑动过程中，整个页面都会返回
none：无动画
cardStyle：自定义设置跳转效果
transitionConfig： 自定义设置滑动返回的配置
onTransitionStart：当转换动画即将开始时被调用的功能
onTransitionEnd：当转换动画完成，将被调用的功能
path：路由中设置的路径的覆盖映射配置
initialRouteName：设置默认的页面组件，必须是上面已注册的页面组件
initialRouteParams：初始路由参数
测试中发现，在iphone上标题栏的标题为居中状态，而在Android上则是居左对齐。所以需要我们修改源码，进行适配。
在navigationOptions中设置headerTitleStyle的alignSelf为 ' center '即可解决。
```
