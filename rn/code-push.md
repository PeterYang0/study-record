#### code-push

CodePush 是一个微软开发的云服务器。通过它，开发者可以直接在用户的设备上部署手机应用更新。CodePush 相当于一个中心仓库，开发者可以推送当前的更新（包括 JS/HTML/CSS/IMAGE 等）到 CoduPush，然后应用将会查询是否有更新。
当然我们也可以使用 code-push-server 来代替微软云服务器，自己搭建云服务管理需要更新的代码，待到下一篇来介绍 code-push-server 的使用

#### 安装 code-push

```
yarn global add code-push-cli
```

#### 注册 code-push 账号

```
code-push register
```

会自动打开一个授权网页，让你选择使用哪种方式进行授权登录，一般选择 GitHub 就行，授权成功后就会得到授权码，回到终端输入授权码就注册并登录成功了

登录注册相关命令：

- 注册 `code-push register`
- 登陆 `code-push login`
- 注销 `code-push logout`
- 列出 登陆的 token `code-push access-key ls`
- 删除某个`access-key code-push access-key rm <accessKey>`

#### 在 code-push 服务器注册 App

为了让 CodePush 服务器有我们的 App，我们需要 CodePush 注册 App。这里需要注意如果我们的应用分为 iOS 和 Android 两个平台，这时我们需要分别注册两套 key。系统默认有两套部署环境，一个是 Production 和 Staging，分别对应生产版的热更新部署，Staging 代表开发版的热更新部署，但是我们也可以自定义添加部署环境。在 ios 中将 staging 的部署 key 复制在 info.plist 的 CodePushDeploymentKey 值中，在 android 中复制在 Application 的 getPackages 的 CodePush 中

- 添加部署环境 `code-push deployment add <app_name> test`//创建 test 环境
- 添加应用平台 `code-push app add <app_name> <os> <platform>`
  例如添加 iOS 平台 `code-push app add appname-ios ios react-native`,多个平台执行多次
- 查看应用列表 `code-push app list`
- 查看 APP 的环境信息 `code-push deployment list <app_name>`
- 查看appkey：`code-push deployment ls emilyzskqA -k`
- 查看某个包的版本: `code-push deployment ls github_trending-ios`

* android 配置(rn 0.60+)

1. 在 android/app/build.gradle 文件中，将文件添加 codepush.gradle 为下面的其他构建任务定义 react.gradle：

```
apply from: "../../node_modules/react-native/react.gradle"
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
```

2. MainApplication.java 通过以下更改更新文件以使用 CodePush：

```
...
// 1. 先引入
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends Application implements ReactApplication {

    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        ...

        // 2. 配置
        @Override
        protected String getJSBundleFile() {
            return CodePush.getJSBundleFile();
        }
    };
}
```

3. 在 strings.xml 配置 key

```
<resources>
     <string name="app_name">AppName</string>
     <string moduleConfig="true" name="CodePushDeploymentKey">DeploymentKey</string>
 </resources>
```

- ios 配置(0.60+)

1. 运行 cd ios && pod install && cd ..以安装所有必需的 CocoaPods 依赖项。
2. 打开 AppDelegate.m 文件，并为 CodePush 标头添加导入语句：

```
#import <CodePush/CodePush.h>
```

3. 查找以下代码行，该代码为生产版本的网桥设置源 URL：

```
{
  #if DEBUG
    return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
  #else
    return [CodePush bundleURL]; // 如果要调试code-push 只保留此条，然后拖入bundle.js
  #endif
}
```

4. 将部署密钥添加到 Info.plist

```
  <key>CodePushDeploymentKey</key>
	<string>Dlygo4DDIqVQeWK-rHIomE9XclUKSKc7hM23G</string>
```

#### 推送及更新测试
- android 
```
code-push release-react 《应用名》 android -t "1.0.0" --des "测试热更新" -d Staging
```

- ios
```
code-push release-react 《应用名》 ios -t "1.0.0" --des "测试热更新" -d Staging
```

新建一个bundles文件夹, 手动打包出来
```
react-native bundle --platform ios --entry-file index.js --bundle-output ./bundles/main.jsbundle --assets-dest ./bundles --dev false
```
