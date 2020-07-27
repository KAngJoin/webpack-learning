### 配置 ES6/7+ 语法转译为ES5语法

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

### 使用webpack-dev-server配置本地开发服务

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

### 使用HtmlWebpackPlugin插件自动生成html文件

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