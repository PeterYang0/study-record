#### 安装 react-native-vector-icons

```
yarn add react-native-vector-icons
```

PS: react-native 0.60+ 集成了自动 link

#### android 配置 react-native-vector-icons

- 编辑 android/app/build.gradle（NOT android/build.gradle）并添加以下内容：

```
project.ext.vectoricons = [
    iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf' ] // Name of the font files you want to copy
]

apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

-

#### ios 配置 react-native-vector-icons

- cd ios 进行 pod install

- Info.plist 中添加字体

```
<key>UIAppFonts</key>
	<array>
		<string>AntDesign.ttf</string>
		<string>Entypo.ttf</string>
		<string>EvilIcons.ttf</string>
		<string>Feather.ttf</string>
		<string>FontAwesome.ttf</string>
		<string>FontAwesome5_Brands.ttf</string>
		<string>FontAwesome5_Regular.ttf</string>
		<string>FontAwesome5_Solid.ttf</string>
		<string>Foundation.ttf</string>
		<string>Ionicons.ttf</string>
		<string>MaterialIcons.ttf</string>
		<string>MaterialCommunityIcons.ttf</string>
		<string>SimpleLineIcons.ttf</string>
		<string>Octicons.ttf</string>
		<string>Zocial.ttf</string>
		<string>iconfont.ttf</string>
	</array>
```

- 使用
  图标库：https://oblador.github.io/react-native-vector-icons

```
import AntDesign from 'react-native-vector-icons/AntDesign';

<AntDesign name="linechart" size={size} color={color} />
```

#### 自定义 iconfont 图标库

- 下载 iconfont 库，将 iconfont.ttf 拖到字体列表
- ios 到 Info.plist 添加

```
<string>iconfont.ttf</string>
```

- android 到 android/app/build.gradle 也添加对应字体库
- 新建 Icon 组件并因为对应 icon.json

```
import {createIconSet} from 'react-native-vector-icons';
import mapper from './config';

const Icon = createIconSet(mapper, 'iconfont', 'iconfont.ttf');
export default Icon;

export const Button = Icon.Button;
export const TabBarItem = Icon.TabBarItem;
export const TabBarItemIOS = Icon.TabBarItemIOS;
export const ToolbarAndroid = Icon.ToolbarAndroid;
export const getImageSource = Icon.getImageSource;
```

- 需要将 icon.json 的格式转换一下

```
import * as json from '@/assets/icons/iconfont.json';

const {glyphs = []} = json.default;
const mapper = {};
glyphs.forEach(item => {
  mapper[item.name] = item.unicode_decimal;
});
export default mapper;
```
