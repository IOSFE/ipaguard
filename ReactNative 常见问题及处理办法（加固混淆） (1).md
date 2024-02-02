ReactNative 常见问题及处理办法（加固混淆）
@[TOC]
## 摘要
本文总结了 ReactNative 开发中常见问题及解决方法。从 ScrollView 在 TouchableOpacity 组件内滑动困难到 Xcode 编译路径设置，都有相应解决方案。此外，还介绍了热更新问题、高度获取、强制横屏UI适配、清理缓存等实用技巧。

## 引言
ReactNative 作为一种跨平台开发框架，尽管强大，但也常伴随着一些问题。本文收集并解答了一些常见问题，为开发者提供了一些实用的技术指南。

## 正文

### ScrollView内无法滑动
在 TouchableOpacity 组件内使用 ScrollView 可能导致滑动失效。解决方法是将 ScrollView 内容用 TouchableOpacity 包裹，并设置 `onPress={() => {}}` 属性。

```javascript
<TouchableOpacity onPress={() => {}}>
  <ScrollView>
    {/* Scrollable content */}
  </ScrollView>
</TouchableOpacity>
```

### RN热更新中的文件引用问题
使用 codepush 进行热更新后，在 Android 系统中 src 目录下的音频文件可能无法引用。解决方法是将文件放到原生系统中，因为热更的 bundle 文件无法包含音频文件。

### RN中获取高度的技巧
获取屏幕高度和窗口高度的不同方法：

```javascript
// 屏幕高度（状态栏+安全区+下方虚拟按键操作区）
Dimensions.get('screen').height

// 窗口高度（状态栏+安全区）
Dimensions.get('window').height
```

### RN强制横屏UI适配问题
横屏下获取的宽、高不同于竖屏状态下的尺寸。通常，可以采用横屏下宽度大于高度的普遍规则进行页面适配。

### 低版本RN（0.63以下）适配iOS14图片无法显示问题
修改 `RCTUIImageViewAnimates.m` 文件，添加以下代码片段，确保 iOS14 以上系统可以正常显示图片：

```objective-c
if (_currentFrame) { //275行
    layer.contentsScale = self.animatedImageScale;
    layer.contents = (__bridge id)_currentFrame.CGImage;
} else { //加上这个 不然ios14以上的系统看不见图片
    [super displayLayer:layer];
}
```

### RN清理缓存
清理缓存的步骤：

1. `watchman watch-del-all`
2. `rm -rf node_modules && npm install`
3. `rm -rf /tmp/metro-bundler-cache-*` (`npm start --reset-cache` / `react-native start --reset-cache`)
4. `rm -rf /tmp/haste-map-react-native-packager-*`

### RN navigation参数取值
获取导航参数的方法：

```javascript
console.log(this.props.navigation.state.params.data)
```

### pod install 或者npm install 443问题处理
解决 443 错误的步骤：

1. 修改 `/etc/hosts`，添加：

```
199.232.68.133 raw.githubusercontent.com
140.82.113.3 github.com
```

2. 清空 git 代理：

```
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --global --list
```

3. 设置环境变量：

```
env GIT_SSL_NO_VERIFY=true
```

## 打开要处理的IPA文件
第一项，填写我们需要重签名的 ipa 路径（当前导入的路径跟导出的路径）

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/3115b07e5fc54c4d9f5931934cf31a46.png)




## 设置签名使用的证书和描述文件
测试配置阶段使用开发测试证书，方便安装到手机测试混淆后ipa是否工作正常，测试ok，最后准备上架的时候再改成发布证书和发布描述文件

如果ipa需要特殊的权限配置，可以使用权限配置文件

如果希望直接处理完后安装到设备，则勾选安装到设备选项 苹果手机数据线连接电脑即可识别设备，如果链接成功后没显示设备，则先安装itunes或者ios驱动。 

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/388b6a4e03d243e0ad86c4a44cf2b141.png)



## 开始ios ipa重签名
第四项点击开始处理，ipaguard会自动尝试讲ipa安装到手机，如果是发布证书并且忘记关闭安装到设备选项，则安装可能会失败，但是ipa是正常生成的，可以用来上架。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/6ab7a265553e45c490d6e2d88c2fca72.png)



## 总结
ReactNative 开发中会遇到各种问题，但通过本文提供的方法和技巧，可以有效解决大部分常见问题。除了以上列举的问题外，还有诸如 Xcode 路径配置、iOS 下载链接拼接等问题都有相应的解决方案。

## 参考资料
- [React Native Documentation](https://reactnative.dev/docs/getting-started)
- [ipaguard](https://ipaguard.com/)
- [Apple Developer Documentation](https://developer.apple.com/documentation/)

---

在ReactNative开发中，面对这些常见问题的解决方案是相当有用的。你在实际项目中遇到了类似的问题吗？
