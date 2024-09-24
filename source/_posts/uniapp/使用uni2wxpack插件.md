---
title: uni2wxpack的使用
date: 2023-01-10 20:51:18
tags: [uniapp, 微信小程序]
categories: uniapp
cover: https://gitee.com/zyjgyp/imgs/raw/master/imgs/uni2wxpack.webp
---

1. 通过命令行创建`uni-app`应用

```JavaScript
vue create -p dcloudio/uni-preset-vue my-project
```

必须使用`vue-cli`创建项目，如果上边创建不成功的话去[https://github.com/dcloudio/uni-preset-vue)](https://github.com/dcloudio/uni-preset-vue)这个链接`clone`到本地，然后将`dcloudio/uni-preset-vue`替换成你克隆下来的本地的目录中

```JavaScript
vue create -p C:\Users\Administrator\Desktop\uni-preset-vue my-project
```

2. 项目创建完成之后，执行命令`npx uniapp2wxpack --create`，创建完成之后项目根目录会自动创建`projectToSubPackageConfig.js`文件，`package.json`会出现以下命令

   ```JavaScript
   // 微信小程序开发
   npm run dev:mp-weixin-pack
   // 头条小程序开发
   npm run dev:mp-toutiao-pack
   // 支付宝小程序开发
   npm run dev:mp-alipay-pack
   // 百度小程序开发
   npm run dev:mp-baidu-pack

   // 微信小程序打包（生产环境）
   npm run build:mp-weixin-pack
   // 头条小程序打包（生产环境）
   npm run build:mp-toutiao-pack
   // 支付宝小程序打包（生产环境）
   npm run build:mp-alipay-pack
   // 百度小程序打包（生产环境）
   npm run build:mp-baidu-pack
   ```

   还会多出几个几个文件夹，放自己开发的原生小程序的代码

   - `mainWeixinMp` 微信小程序
   - `mainToutiaoMp` 头条小程序
   - `mainBaiduMp` 百度小程序
   - `mainWeixinMp` 支付宝小程序

3. 将自己的原生小程序的代码放入之后，执行命令`npm run dev:mp-weixin-pack`这个命令![图片](https://pic.imgdb.cn/item/659e114d871b83018af7d503.png "图片")出现上边这个图片中内容代表运行成功。

4. 成功之后会生成一个`dist`的文件夹，然后通过微信开发者工具导入`dist->dev->mp-weixin-pack`这个文件夹。（如果打开之后没有内容，重新打开一下。
   ![图片](https://pic.imgdb.cn/item/659e128e871b83018afc5825.png "图片")

5. 将`uniapp`作为分包，微信小程序作为主包。在`mainWeixinMp/app.json`中，将`uniSubpackage`设置为分包目录。`uniSubpackage`中的`pages`一定要设置成`[]`。插件会自动生成`pages`

```JavaScript
// mainWeixinMp -> app.json中的 subpackages
{
  "subPackages":[{
    "root":"uniSubpackage",
    "pages":[]
  }]
}
```

6.  导入`uview`组件库

    - 一定要通过网页上下载方式去导入生成`uni_modules`，不要通过`npm`的方式去下载。
      ![图片](https://pic.imgdb.cn/item/659e33b2871b83018a77087b.png "图片")

      > 通过`npm`方式的方式去下载，会出现`uview`中的组件引入自己的组件，采用的是绝对路径。项目运行起来会直接去小程序根目录下的`node_modules`中去找，而我们小程序的依赖中并没有`uview`这个依赖，导致组件找不到。(不知道有没有大神有其他方案)

    - 下载完成之后，要下载` sass``sass-loader `，执行命令

      ```JavaScript
      // 安装sass
      npm i sass -D

      // 安装 sass-loader
      npm i sass-loader -D
      ```

    - 再引入`uView`的全局`SCSS`主题文件，在项目根目录的 uni.scss 中引入此文件。

    ```JavaScript
    /* uni.scss */
    @import '@/uni_modules/uview-ui/theme.scss';

    ```

    - 再`App.vue`中引入 uView 基础样式

    ```JavaScript
    <style lang="scss">
        /* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
        @import "@/uni_modules/uview-ui/index.scss";
    </style>
    ```

    --- 测试

    ```JavaScript
        // src -> pages -> index -> index.vue
        <template>
            <view style="padding: 20px;">
                <u-button type="primary" text="确定"></u-button>
            </view>
        </template>
    ```

    ![图片](https://pic.imgdb.cn/item/659e370a871b83018a820e5d.png "图片")
