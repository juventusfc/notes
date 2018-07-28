# webpack4 使用指南

webpack4 可以实现 0 配置，也可以结合配置文件使用。

## Entry

Entry 指定了打包的起始点。默认值为`./src/index.js`。

### 速记语法

`entry: string|Array<string>`

```javscript
const config = {
  entry: './path/to/my/entry/file.js'
};

module.exports = config;
```

等价于

```javascript
const config = {
  entry: {
    main: "./path/to/my/entry/file.js"
  }
};
```

注意，当传入数组时，表示多入口，但是数组中的各个入口文件会打成一个包。

### 对象语法(推荐)

`entry: {[entryChunkName: string]: string|Array<string>}`

```javascript
const config = {
  entry: {
    app: "./src/app.js",
    vendors: "./src/vendors.js"
  }
};
```

```javascript
const config = {
  entry: {
    pageOne: "./src/pageOne/index.js",
    pageTwo: "./src/pageTwo/index.js",
    pageThree: "./src/pageThree/index.js"
  }
};
```

扩展：当使用多个入口文件时，可以使用 CommonsChunkPlugin 来提取公共代码。

## Output

Entry 指定了打包后文件的存放点。打包后的 main 文件的存放点默认值为`./dist/bundle.js`，其他非 main 文件为`./dist`。

```javscript
const config = {
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};

module.exports = config;
```

当打包多入口文件时，

```javascript
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```

## Loaders

Loaders 用于 webpack 识别打包 js 文件之外的文件类型，如 css、txt 等。

1. 安装 loaders
    > npm install --save-dev css-loader  
    > npm install --save-dev ts-loader
2. 使用 loaders
    > ```javascript
    > module.exports = {
    >   module: {
    >     rules: [
    >       { test: /\.css$/, use: "css-loader" },
    >       { test: /\.ts$/, use: "ts-loader" }
    >     ]
    >   }
    > };
    > ```

## Plugins

Plugins 用于扩展 webpack 功能，主要是打包优化、asset 管理等。

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin"); // 1. require plugin
const webpack = require("webpack"); // 2. to access built-in plugins

const config = {
  // 3. 使用new创建实例，将实例加入plugins数组中
  plugins: [new HtmlWebpackPlugin({ template: "./src/index.html" })]
};

module.exports = config;
```

## Mode

模式包括：development, production(默认值) 和 none

```javascript
module.exports = {
  mode: "production"
};
```

## webpack4 react 结合使用

webpack4 和 react 应用能完美结合使用。[点击此处](https://github.com/juventusfc/webpack4-react-template)获取例子
