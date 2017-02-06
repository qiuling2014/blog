### webpack

#### 目录：
- 1. webpack是什么
- 2. 如何调用webpack
- 3. 编译js
- 4. Css样式和图片的加载
- 5. 功能标识（Feature flags）
- 6. 多个入口文件
- 7. 优化通用代码
- 8. 异步加载

#### 1. webpack是什么

> webpack is a module bundler.

官方对webpack的定位是模块打包器，相比于gulp或是grunt,webpack的竞争对手应该是browserify。

Webpack 是一个前端资源加载/打包工具，只需要相对简单的配置就可以提供前端工程化需要的各种功能，并且如果有需要它还可以被整合到其他比如 Grunt / Gulp 的工作流，
[webpack with gulp](http://webpack.github.io/docs/usage-with-gulp.html)。

#### 2. 如何调用webpack

选择一个目录下有`webpack.config.js`文件的文件夹，然后运行下面的命令:

- `webpack` 开发环境下编译
- `webpack -p` 产品编译及压缩
- `webpack --watch` 开发环境下持续的监听文件变动来进行编译(非常快!)
- `webpack -d` 引入 source maps
- `webpack --colors` 让编译输出更加漂亮

能配合npm script命令使用：
```js
// package.json
{
	'scripts': {
		'dev': 'webpack-dev-server --devtool eval --progress --colors',
		'deploy': 'NODE_ENV=production webpack -p'
	}
}
```

#### 3. 编译js

webpack可以和browserify、RequireJS一样作为一个loader(加载工具)来使用。

```js
// webpack.config.js
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      { test: /\.coffee$/, loader: 'coffee-loader' },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015', 'react']
        }
      }
    ]
  },
  resolve: {
    // 现在你require文件的时候可以直接使用require('file')，不用使用require('file.coffee')
    extensions: ['', '.js', '.json', '.coffee']
  }
};
```
载入babel-loader、coffee-loader。

如果你希望在require文件时省略文件的扩展名，只需要在webpack.config.js中添加 resolve.extensions 来配置。



#### 4. Css样式和图片的加载

当你require了CSS(less或者其他)文件，webpack会在页面中插入一个内联的`<style>`，去引入样式。当require图片的时候，bundle文件会包含图片的url，并通过`require()`返回图片的url。

```js
module.exports = {
  entry: './main.js',
  output: {
    path: './build', // 图片和js会放在这
    publicPath: 'http://mycdn.com/', // 这里用来生成图片的地址
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      { test: /\.less$/, loader: 'style-loader!css-loader!less-loader' }, // 用!去链式调用loader
      { test: /\.css$/, loader: 'style-loader!css-loader' },
      {test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'} // 内联的base64的图片地址，图片要小于8k，直接的url的地址则不解析
    ]
  }
};
```

#### 5. 功能标识（Feature flags）
项目中有些代码我们只为在开发环境（例如日志）或者是内部测试环境（例如那些没有发布的新功能）中使用，那就需要引入下面这些魔法全局变量（magic globals）：

```js
if (__DEV__) {
  console.warn('Extra logging');
}
// ...
if (__PRERELEASE__) {
  showSecretFeature();
}
```

同时还要在webpack.config.js中配置这些变量，使得webpack能够识别他们。

```js
// webpack.config.js

// definePlugin 会把定义的string 变量插入到Js代码中。
var definePlugin = new webpack.DefinePlugin({
  __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'true')),
  __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))
});

module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  },
  plugins: [definePlugin]
};
```

#### 6. 多个入口文件

有两个页面：profile和feed。如果你希望用户访问profile页面时不加载feed页面的代码，那就需要生成多个bundles文件：为每个页面创建自己的“main module”（入口文件）。

```js
// webpack.config.js
module.exports = {
  entry: {
    Profile: './profile.js',
    Feed: './feed.js'
  },
  output: {
    path: 'build',
    filename: '[name].js' // name是基于上边entry中定义的key
  }
};
```

##### 提取公共类库
> 这部分我没有用到，但也是基于webpack 内置的插件，目的是将用到的jquery 等第三方库整合到一个文件，否则都合并到一个文件，会造成这个文件特别大
首先在entry 声明第三方库。

```js
entry: {
    app: path.resolve(APP_PATH, 'index.js'),
    //添加要打包在vendeors里面的库
   vendors: ['jquery', 'moment']
},
plugins: {
      //把入口文件里面的数组打包成vendors.js
    new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js'),
}
```

#### 7. 优化通用代码

Feed和Profile页面存在大量通用代码(比如React、公共的样式和组件等等)。webpack可以抽离页面间公共的代码，生成一个公共的bundle文件，供这两个页面缓存使用:

```js
// webpack.config.js

var webpack = require('webpack');

var commonsPlugin =
  new webpack.optimize.CommonsChunkPlugin('common.js'); // 引入插件

module.exports = {
  entry: {
    Profile: './profile.js',
    Feed: './feed.js'
  },
  output: {
    path: 'build',
    filename: '[name].js' // 为上面entry的key值
  },
  plugins: [commonsPlugin]
};
```
在上一步引入自己的bundle之前引入<script src="build/common.js"></script>


#### 8. 异步加载
虽然CommonJS是同步加载的，但是webpack也提供了异步加载的方式。这对于单页应用中使用的客户端路由非常有用。当真正路由到了某个页面的时候，它的代码才会被加载下来。

```js
if (window.location.pathname === '/feed') {
  showLoadingState();
  require.ensure([], function() { // 这个语法痕奇怪，但是还是可以起作用的
    hideLoadingState();
    require('./feed').show(); // 当这个函数被调用的时候，此模块是一定已经被同步加载下来了
  });
} else if (window.location.pathname === '/profile') {
  showLoadingState();
  require.ensure([], function() {
    hideLoadingState();
    require('./profile').show();
  });
}
```
剩下的事就可以交给webpack，它会为你生成并加载这些额外的 chunk 文件。

webpack 默认会从项目的根目录下引入这些chunk文件。你也可以通过 output.publicPath来配置chunk文件的引入路径

```js
// webpack.config.js
output: {
    path: "/home/proj/public/assets", // webpack的build路径
    publicPath: "/assets/" // 你require的路径
}
```


#### 9. 如何构建一个gulp与webpack相配合的前端工作流呢?



#### 参考资料

[1]. [https://github.com/petehunt/webpack-howto](https://github.com/petehunt/webpack-howto)
