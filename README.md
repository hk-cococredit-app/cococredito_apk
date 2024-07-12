# COCO CREDIT 香港本土项目说明文档

## 框架

ionic 跨平台框架： https://ionicframework.com/
原生交互插件： https://capacitorjs.com/docs/v2
React: https://react.docschina.org/
Sass: https://www.sass.hk/
Typescript: https://www.tslang.cn/docs/home.html

## 项目结构

```sh
apps/cococredit/src
|
+-- assets            # assets 文件夹可以包含所有静态文件，例如图像、字体等。
|
+-- components        # 在整个应用程序中使用的共享组件
|
+-- config            # 所有的全局配置、 env 变量等都从这里导出并在应用程序中使用
|
+-- pages             # 包含每个功能模块
  |
  +-- applyStep             # 填写资料页面
  |
  +-- home                  # 首页
  |
  +-- login                 # 登录/注册
  |
  +-- me                    # 个人中心
  |
  +-- product               # /confirm: 产品确认页输入金额 /repay-plan: 贷款计划明细  /credit: 提交产品后的贷款动画页面 /face: 人脸预览页 
  |
  +-- protocol              # 协议列表页面（协议内容还是单个html静态文件）
  |
  +-- record                # /loan 贷款记录  /repay 还款记录
  |
  +-- repay                 # 还款  /offline 线下便利店二维码还款  /online 线上银行转账还款 /receipts 提交还款凭证
|
+-- hooks             # 在整个应用程序中使用的共享 hooks
|
+-- locales           # 国际化文件
|
+-- providers         # 三方埋点的provider
|
+-- routes            # 路由
|
+-- theme             # 主题
|
+-- utils             # 工具类
```

```sh

libs
|
+-- analysis          # 埋点库，具体使用请查看 README.md
|
+-- core              # 公共的UI库
|
+-- fp-id             # 指纹ID获取，使用第三方服务 FingerPrint (仅在h5才获取)
|
+-- modules           # 业务模块
|
+-- net               # 网络请求
|
+-- storage           # 持久化存储
|
+-- utils             # 常用工具方法

```

## 安装

1. 配置 Npm 仓库

https://packages.aliyun.com/npm/npm-registry/guide

注： 需要开通 Quark 的权限

2. 根目录下 npm install 或 yarn install

```sh

yarn install

```

3. apps/cococredit 目录下

```sh

cd apps/cococredit
yarn

```

## 环境

参考 src/config/index.ts 里面的配置说明

## 运行

- 脚本命令说明：

开发环境： yarn run start
测试环境： yarn run build-test
生产环境： yarn run build

1. 运行 h5 项目：

依赖安装完成之后，直接运行命令启动开发环境

```sh

yarn start

```

2. 运行 ios 项目：

测试环境： yarn && yarn run build-test && npx @capacitor/cli@2.5.0 sync ios
生产环境： yarn && yarn run build && npx @capacitor/cli@2.5.0 sync ios

然后使用 Xcode 打开 ios/App/App.xcworkspace，然后配置证书等即可运行

注：每次打包后需同步到 ios 或 android 工程。

```sh

npx @capacitor/cli@2.5.0 sync ios

npx @capacitor/cli@2.5.0 sync android

```

3. 运行 android 项目：

测试环境： yarn && yarn run build-test && npx @capacitor/cli@2.5.0 sync android && npx cap sync android
生产环境： yarn && yarn run build && npx @capacitor/cli@2.5.0 sync android

注：每次打包后需同步到 ios 或 android 工程。

```sh

npx @capacitor/cli@2.5.0 sync ios

npx @capacitor/cli@2.5.0 sync android

```

每次同步后 Android 需修改：

- "platform('com.google.firebase:firebase-bom:30.3.1')" 去除两边的引号
- 搜索 <activity <service <provider 标签，需要加上 android:exported="false" （如果已经有的话就不修改）
- 搜索 @string/fb_app_id @string/fb_app_name ，里面报红的修改成 fb id, client token 等（在 app/src/main/res/values/strings.xml 文件里面复制）

- 发版检查:
- <bool name="branch_test_mode">true</bool> 注释
- <meta-data android:name="io.branch.sdk.TestMode" android:value="true" /> 设置为 false
- Branch.enableTestMode(); 注释

然后使用 Android Studio 打开

## 自定义的原生插件说明

-  @quark-base-plugin/app-install    app 安装记录, 插件名：QKAppInstallRecords
-  @quark-base-plugin/contacts       获取通讯录插件，插件名：QKContacts
-  @quark-base-plugin/device         设备信息插件，插件名：QKDevice
-  @quark-base-plugin/face-verify    人脸插件，插件名：QKFaceVerify
-  @quark-base-plugin/live-update    热更新插件 (直接使用hooks)
-  @quark-base-plugin/photos         照片信息插件，插件名：QKPhotos

使用方式：

```

import { Plugins } from '@capacitor/core';

Plugins.插件名.插件方法

```

插件API使用请查看https://codeup.aliyun.com/60b71d6d66bba1c04b443900/quark-frontend/quark-native-plugin
或
参考项目里面的使用

附：开发自定义插件文档：https://capacitorjs.com/docs/v2

## 发版注意事项

1. 热更新功能

每次发版在 live-update.json 修改 versionId 的值，保持和之前的版本不一致即可。

src/routes/index.tsx 参考代码

```

const { checkForUpdates } = useLiveUpdate();

```

2. jenkins 打包

测试环境：http://jenkins.bowenfin.com/job/SG_HK_Business_Frontend/job/Cococredit_H5/
生产环境：http://jenkins.bowenfin.com/job/HK_Business_Frontend/job/Cococredit_H5/

## License

[MIT]
