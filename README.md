## 配置

#### entry



## ES2015+ 新特性

#### 模块

> webpack 内部已经实现对 ES2015中的`import` 和 `export` 的原生支持

## package.json

#### 确保安装包私有？

```json
{
  ...
+ "private": "true",
  ...
}
```

#### 防止意外发布代码？

```json
{
  ...
- "main": "index.js",
  ...
}
```

## 开发环境和生产环境

#### devDependencies 和 dependencies

- devDependencies 只用与开发环境
- dependencies 用于生产环境

举个🌰

```json
"devDependencies": { 
  "webpack": "^4.44.1",
  "webpack-cli": "^3.3.12"
},
"dependencies": {
  "lodash": "^4.17.20"
}
```

#### npm install sax

###### 指令

- `--save-prod`           **alias**  `-P` ，添加dependencies 里面所有的包。在 `-D` `-O` 不存在时，`-P` 就是默认值
- `--save`                     **alias** `-S` ，添加dependencies 里面所有的包。
- `--save-dev`             **alias** `-D` ，添加devDependencies里面所有的包。
- `--save-optional`    **alias** `-O` ，添加<u>optionalDependencies</u>里面所有的包。
  - `--no-save` 				 阻止保存记录在dependencies 中

###### 参考

- [npm 解析](https://docs.npmjs.com/cli/install)

#### 需求区分----各取所需

###### 开发环境的需求

- 模块热更新  （本地开启服务，实时更新）
- sourceMap    (方便打包调试)
- 接口代理　    (配置proxyTable解决开发环境中的跨域问题)
- 代码规范检查 (代码规范检查工具)

###### 生产环境的需求

- 提取公共代码
- 压缩混淆(压缩混淆代码，清除代码空格，注释等信息使其变得难以阅读)
- 文件压缩/base64编码(压缩代码，减少线上环境文件包的大小)
- 去除无用的代码

###### 开发环境和生产环境的共同需求

- 同样的入口
- 同样的代码处理(loader处理)
- 同样的解析配置

## 配置 ES6/7+ 语法转译为ES5语法

- 对ES6语法的支持

```
npm install babel-loader @babel/core @babel/preset-env --save-dev
```

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/, // 不包括的文件夹 node_modules
        use: {
          loader: 'babel-loader', // es6 转 es5
          options: {
            presets: [ // 只支持部分语法
              '@babel/preset-env'
            ],
            plugins: [ // class
              '@babel/plugin-proposal-class-properties'
            ]
          },
        }
      },
    ],
  },
}
```

babel-env插件只能处理部分ES6的语法，如class，generator等却无能为力，这时我们就需要使用其它的插件了。如@babel/plugin-proposal-class-properties

- 对ES7等高级语法的支持

```
npm install @babel/plugin-transform-runtime  --save-dev
npm install @babel/runtime --save
```

```json
// .babelrc 文件
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "target": {
          "borwsers": [
            "> 1%",
            "last 2 versions"
          ]
        }
      }
    ]
  ],
  "plugins": [
    "@babel/transform-runtime"
  ]
}
```

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/, // 不包括的文件夹 node_modules
        use: {
          loader: 'babel-loader', // es6 转 es5
        }
      },
    ],
  },
}
```

## 使用webpack-dev-server配置本地开发服务

1. `npm install webpack-dev-server --save-dev`

2. webpack.config.js配置devServer属性

   ```js
   module.exports = {
     // ... 
     devServer: { 				// 开启本地静态服务,需要安装webpack-dev-server
       port: 8000, 			// 端口
       progress: true, 		// 进度条
       contentBase: "./dist", 	 // 哪一个文件夹
       compress: true,			// 压缩
       open: true				// 自动打开
     }
   }
   ```

3. `http://localhost:8080/` 这样的本地地址

## 使用HtmlWebpackPlugin插件自动生成html文件

1. `npm install html-webpack-plugin --save-dev`

2. 配置项 webpack.config.js

   ```js
   const HtmlWebpackPlugin = require('html-webpack-plugin')
   module.exports = {
     //...
     plugins: [
       new HtmlWebpackPlugin({
         template: './index.html', 		// 模板html
         filename: 'index.html', 			// 生成的html
         hash: true, 					   // 使用hash解析
         minify: { 
           removeAttributeQuotes: true, 	// 移除双引号
           collapseWhitespace: true 		// 一行，折叠换行符等
         }
       })
     ]
   }
   ```

   ​