# webpack是模块打包工具

### tips

1. 命令行 eg:

```
webpack ./index.js 全局中寻找webpack
git ...  全局中寻找git.exe

npx webpack ./index.js 在项目下的node_modules中寻找webpack

webpack.config.js下的
"scripts": {
	"build": "webpack"
}
npm run build 也是在node_modules中寻找webpack
```

2. webpack-cli 的作用是能让我们在命令行里使用webpack

   

3. npm 安装时  -S = --save  -D = --save-dev 

​     --save 是加入到package.json文件中  -dev是开发时环境



### 1.loader

#### 1.1.0 执行顺序 从右到左 从下到上

​		['style-loader', 'css-loader']



#### 1.1. 样式

​	'css-loader'和'style-loader'

​		'css-loader' 可以帮我们分析出几个css文件之间的关系，从而帮我们合并成一个css

​		'style-loader' 帮我们把'css-loader'生成的css挂载到页面的head中



