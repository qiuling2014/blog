# 微信小程序开发指北

## 准备工作

* [小程序文档](https://developers.weixin.qq.com/miniprogram/dev/index.html?t=18122617)
* [mpvue了解一下](http://mpvue.com/mpvue/)
* [Vue.js文档](https://cn.vuejs.org/v2/guide/)
* [下载微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)

## 初始化项目

``` bash
// 创建项目文件夹
mkdir my-project
// 进入项目文件夹
cd my-project
// 初始化项目模板，选择【移动端-小程序】模板
yo worker
```

## 小程序设置

* 项目开始请让需求方先去申请小程序账号;
* 微信公众平台，项目成员添加，添加开发者权限;
* 获取小程序appId、appSecret;

## 互联登录配置

* 在公司[weixinapp@yy.com](mailto:weixinapp@yy.com)开发者账号(可联系支付的张朝峥)下绑定小程序appid，让需求方去处理这项工作。绑定成功后可在【微信公众平台—设置—关联设置】中看到绑定的微信开放平台账号，绑定成功后通知udb同事；
* 提供账号互联appid 和 小程序appId和appSecret 给udb同事进行配置；

<br>

> ### 完成上面的配置后，就可以开始愉快的开发工作了。

## 预览小程序

* 预览需要在微信开发者工具上项目配置appId。

* 总所周知，微信限制了每个月是修改安全域名、业务域名的次数，前期开发过程中如需手机预览，可以先勾选`不校验合法域名、web-view（业务域名）、TLS 版本以及 HTTPS 证`选项（在【微信开发者工具—详情】选项下），同时手机上打开调试模式，这样就不需要配置安全域名、业务域名也能正常访问接口。  
  
## 生命周期

* 同 vue，不同的是mpvue会在小程序 onReady 后，再去触发 vue mounted 生命周期。

* 在页面上的created钩子会在小程序onLaunch后就执行。

## 链接跳转对生命周期的影响

* `redirctTo`：触发onUnload，不触发onHide。

* `navigate`：触发onHide，不触发onUnload。

## mpvue实践总结

### 1.使用mpvue编写组件，slot的坑，热更新bug

组件使用了`slot`，然后页面调用，写入`slot`，小程序未更新到页面上，必须在执行一次`npm run dev`才行。

### 2.background-image不支持本地图片

解决方案：①、把图片编码成base64使用（大图就算了）；②、使用远程图片地址；③、使用image控件代替

### 3.class/style 不支持 vue 的 classObject/styleObject

`computed`方法生成`class`或者`style`字符串，插入到页面中。

### 4.页面数据保留之前旧数据的问题

mpvue中页面跳转后并没有销毁页面实例，而是将其推入页面栈中，所以会保存之前的旧的数据，
在onLoad的时候，将要在本页面展示的数据初始化并且进行新的赋值。

## Flyio的使用

在小程序的开发中，使用Flyio来进行数据交互。文档地址：[fly.js](https://wendux.github.io/dist/#/doc/flyio/readme)

`````javascript
const fly = new Fly();
// 请求拦截器
fly.interceptors.request.use(request => {
  if (wx.getStorageSync('sessionid')) {
    request.headers.cookie = wx.getStorageSync('sessionid')
  }
  return request
});
// 配置请求基地址
fly.config.baseURL="https://f2e.yy.con";

fly.get("/user/login",{ name: 'zhangsan' }).then((d)=>{
  //输出请求数据
  console.log(d.data)
}).catch(err=>{
  console.log(err.status,err.message)
})
`````

## VUEX的使用

使用vuex来管理状态，能很方便的实现数据共用。

## 注意事项

### 资源/后台相关

* 全部接口/资源用https;
* 小程序限制了后台域名数量20个，且每月只能改5次。可用proxyapi.yy.com转发请求规避，详询夏集球;
* 部分后端接口限定了来源域名，要在白名单加上servicewechat.com;
* 请求默认不带cookie，可存在localstorage，在请求headers参数里带（demo中requestWithCookie）;
* 由于部分数据是页面从微信获取传给后端，存在伪造数据的可能性。因此向后端传敏感数据都应该传加密数据;
* tabBar用到的图片并不在项目构建范围中，不会复制dist，需手动复制一下;

### 小程序API相关

* 没有dom类的API，所以不能操控dom;
* getUserInfo等敏感数据接口已废弃，必须由用户点击按钮触发授权;
* 小程序大部分api为异步，少数为同步或异同两版本(如`getAccountInfoSync`, `setStorageSync`, `getStorageSync`),涉及状态变化的应尽量选用同步api;

### 调试工具相关

* model在AppData模块;
* web-view(类似iframe)可以右键调出该webview的控制台;
* 加了信任域名必须删项目重加才生效;
* 开发期间要用微信打开小程序，要先在管理后台添加为开发者;
* “真机调试”为开发版（临时二维码），“上传”为体验版（长期二维码）;

### 组件/UI相关

* 小程序宽度单位rpx, 1rpx = 屏宽*1/750
* 自定义组件不能和小程序自带组件同名;
* html原生标签和小程序组件可以混用;
* mpvue自定义组件不允许绑定事件(巨坑);
* canvas唯一标识符是canvas-id，非id;
* 要覆盖在canvas上，必须用cover-view，此组件不支持大多数css3;
* web-view可用query传参;
* tabBar和topBar，还有alert等，是微信app原生的，不可覆盖;

### 接口封装

由于微信小程序api都是异步返回，类jq写法，基本包含success, fail, complete等参数，返回值为对象。所以封装接口也采用一样风格（下详）。

### 互联登录

先通过`wx.login()`登录，然后判断是否有`userinfo`权限，有则`wx.getUserInfo()`,然后通过互联接口进行互联，并保存cookie于storage，后续请求都主动带上。

[互联登录接口文档](http://doc.game.yy.com/pages/viewpage.action?pageId=29402224#UDB-%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%99%BB%E5%BD%95-%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F-UDB%E7%99%BB%E5%BD%95%E4%BA%92%E8%81%94)

```javascript
const APIS = {
  decrypt: 'https://mclogin.yy.com/open/wxmp/decrypt',
  login: 'https://mclogin.yy.com/open/wxmp/login',
};

const APPID = 'wx3fe99b2876386ec9';

export function setCookies(cookieObj) {
  let cookies = '';

  Object.keys(cookieObj).map((key) => {
    cookies += `${key}=${cookieObj[key]};`;
    return false;
  });

  wx.setStorageSync('cookie', cookies);
}

export function loginAndGetUserInfo({ success, fail }) {
  // 微信登录
  wx.login({
    success: (resLogin) => {
      // 获取用户信息
      wx.getSetting({
        success: (settingRes) => {
          console.log(settingRes);
          if (settingRes.authSetting['scope.userInfo']) {
            console.log('已授权userInfo');
            // 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
            wx.getUserInfo({
              success: (infoRes) => {
                console.log('登录互联');
                const { iv, encryptedData } = infoRes;
                wx.request({
                  url: APIS.login,
                  data: {
                    APPID,
                    code: resLogin.code, // 小程序登录返回的code
                    encryptData: encodeURIComponent(encryptedData), // 小程序获取用户信息返回的加密数据
                    iv: encodeURIComponent(iv), // 小程序获取用户信息返回的加密初始向量
                  },
                  success(res) {
                    const cookie = res.data.data.headers;
                    setCookies(cookie); // 保存cookie
                    success && success(res);
                  },
                  fail(res) {
                    fail && fail(res);
                  },
                });
              },
            });
          } else {
            fail && fail({
              errMsg: '未授权userInfo',
            });
          }
        },
      });
    },
  });
}
```

### 解密数据

出于安全原因，部分微信api只返回[加密数据(encryptedData)](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html)，需要通过后台解密。

[解密数据接口文档](http://doc.game.yy.com/pages/viewpage.action?pageId=29402224#UDB-%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%99%BB%E5%BD%95-%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F-%E6%95%B0%E6%8D%AE%E8%A7%A3%E5%AF%86%E6%8E%A5%E5%8F%A3)

```javascript
const APIS = {
  decrypt: 'https://mclogin.yy.com/open/wxmp/decrypt',
  login: 'https://mclogin.yy.com/open/wxmp/login',
};

export function decrypt({
  data, // {encryptData, iv} 加密数据和向量
  dataType,
  success,
  fail,
}) {
  wx.request({
    header: {
      cookie: wx.getStorageSync('cookie'), // 需要先登录互联，保存cookie
    },
    url: APIS.decrypt,
    data,
    success(res) {
      if (dataType === 'json') {
        success && success(JSON.parse(res.data));
      } else {
        success && success(res.data);
      }
    },
    fail(res) {
      fail && fail(res);
    },
  });
}
```