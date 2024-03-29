﻿

## 背景
Unity引擎使用Mono运行时，而C#语言易受反编译影响，存在代码泄露风险。本文通过《QQ乐团》项目实践，提出一种适用于Unity引擎的代码混淆方案，以保护代码逻辑。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/367b61ec63cb44028ce652a7c53a2b39.png)

## 引言
在Unity引擎下，为了防止代码被轻易反编译，需要采取相应的保护措施。本文将分享一种基于实践经验的可行方案，希望能对关注Unity引擎的开发者提供一些参考价值。

## 正文
### Unity引擎下代码混淆的特殊性
- 代码被资源引用：Unity中资源的可视化编辑特性导致代码以组件形式附加到资源实例上，需要注意不破坏资源与代码的对应关系。
- 发布到Web的项目：Unity项目的编译和打包过程捆绑在一起，无法像普通.NET程序那样对编译出的程序集进行混淆后再打包。 
- UnityEngine按函数名进行调用：MonoBehaviour上的方法如Awake、Start等通过方法名称访问，重命名会导致调用失败。

### 思路
由于官方未提供独立的接口进行混淆，作者尝试将代码编译成DLL，混淆后再添加到Unity项目中。然而，遇到了Unity引擎处理DLL中模版类型的缺陷，使得之前的尝试受挫。

### 实际混淆步骤
最终，作者将项目进行分层，独立出敏感的“逻辑层”并编译成DLL进行混淆，加上利用ipaguard加固混淆方案将程序加密处理。
Ipa Guard是一款功能强大的ipa混淆工具，不需要ios app源码，直接对ipa文件进行混淆加密。可对IOS ipa 文件的代码，代码库，资源文件等进行混淆保护。 可以根据设置对函数名、变量名、类名等关键代码进行重命名和混淆处理，降低代码的可读性，增加ipa破解反编译难度。可以对图片，资源，配置等进行修改名称，修改md5。只要是ipa都可以，不限制OC，Swift，Flutter，React Native，H5类app。
代码混淆步骤
1. 选择要混淆保护的ipa文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/5dd411ce5ef84bca9ddbf243cba74cfd.png)



2. 选择要混淆的类名称
选择左侧的代码模块中的OC类名称或者Swift类名称，选择IPA种要混淆的二进制文件，然后勾选可执行文件代码里面的类名称。如果类太多可以使用搜索查看功能，ipaguard提供了级别选择，名称搜索，已选未选过滤来帮助配置混淆对象。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/3928420e83ec4866992622f481f2501b.png)





3. 选择要混淆保护的函数，方法
选择左侧代码模块下的oc方法或者swift方法，点击右侧的选择文件选取一个可执行二进制文件，勾选需要混淆保护的方法和函数。ipaguard提供了风险等级过滤，名称搜索过滤，根据类名称过滤条件来辅助配置混淆目标

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/7133323aa6524718a7f5b8f99c7f3034.png)




4. 配置签名证书
点击左侧的签名配置，设置ios签名证书，描述文件等信息。测试阶段用开发证书，这样可以方便安装到测试机子上检验是否测试后的app运行正常；最终配置测试ok，发布的时候再改成发布证书，混淆配置完后可以提交上架。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/96b9207871974ca4a349dfa1021669fa.png)






5. 混淆和测试运行
点击开始处理按钮，ipaguard将对ipa中选中的内容进行混淆保护，并安装混淆好的ipa到手机上，运行如果ok，点击保存配置，下次直接加载配置即可，无需每次配置要混淆的内容。 

ipaguard在做混淆这块还是做的很人性化的，混淆目标可控，强度可控，极大地简化了配置混淆内容的过程，可视化的操作也非常的方便。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/3f0a2091b5184c918e4491483708bd09.png)
## 总结
- Unity项目的代码反编译较为容易，需要重视代码混淆工作。
- 代码混淆方案实施限制较多，对项目的架构分层有强制性要求。

## 参考资料
- [Ipaguard混淆工具](https://www.ipaguard.com)
- Unity官方文档

以上是根据提供的资料，对英文技术博客进行了改写，添加了更加丰富的内容，并结合MD语法进行了排版。希望对您有所帮助。
