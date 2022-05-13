# node-development使用手册
### 1.普通调试-js运行当前文件:
   打开js文件: ①运行菜单 -> ②使用node运行当前文件
   
<img src="/static/snapshots/node_development/1.jpg" />

### 2.普通调试-js调试当前文件:
   打开js文件: ①双击行号打断点 -> ②运行菜单 -> ③使用node调试当前文件

<img src="/static/snapshots/node_development/1.jpg" />

### 3.自定义运行调试-js项目调试:
   打开js项目: ①运行菜单 -> ②自定义node运行

<img src="/static/snapshots/node_development/1.jpg" />

   launch.json中目前支持的变量 `${workspaceFolder}` 指当前项目的根目录
   ```json
	 {
	     "configurations": [ // 配置项
	         {
	             "mode": "debug", // 运行模式
	             "name": "自定义调试文件", //运行菜单的名称(需要唯一)
	             "outFiles": [ // 如果是构建工具构建的，需要使用, 指明输出路径
	                 "${workspaceFolder}/dist/**/*.js"
	             ],
	             "program": "${workspaceFolder}/dist/app.js", //程序运行的入口文件(编译后的)
	             "request": "attach", //请求方式: 固定attach
	             "skipFiles": [ // 调试跳过文件
	                 "<node_internals>/**/*.js"
	             ],
	             "sourceMap": false, // 是否启用sourceMap
	             "sourceMapPathOverrides": { // sourceMap位置
	             },
	             "type": "node" // 类型:node(固定)
	         }
	     ],
	     "version": "0.1"// 版本号
	 }
   ```
   ---
   express脚手架项目配置(使用[express生成器](https://www.expressjs.com.cn/starter/generator.html)生成):
   ```json
   {
       "configurations": [
           {
               "mode": "run", //run 或者 debug
               "name": "express运行",
               "program": "${workspaceFolder}/bin/www",
               "request": "attach",
               "type": "node"
           }
       ],
       "version": "0.1"
   }
   ```
   ---
   webpack-js项目配置
   
   项目结构:
   
<img src="/static/snapshots/node_development/2.jpg" />

   webpack.dev.config.js:
   ```js
   const path = require('path');
   module.exports = {
		entry: {
			index: './index.js'
		},
		mode: "development", 
		devtool: "source-map", //开启sourcemap
		target: 'node',
		node: {
			__filename: false,
			__dirname: false
		},
		output: {
			filename: '[name].js',
			path: path.join(__dirname, 'dist')
		}
   }
   ```
  webpack-js运行和调试配置:
  ```json
  {
      "configurations": [
          {
              "mode": "debug",
              "name": "自定义调试文件",
              "outFiles": [
                  "${workspaceFolder}/dist/**/*.js"
              ],
              "program": "${workspaceFolder}/dist/index.js",
              "request": "attach",
              "skipFiles": [
                  "<node_internals>/**/*.js"
              ],
              "sourceMap": true,
              "sourceMapPathOverrides": {
  				"webpack://webpack/*": "${workspaceFolder}/*" // 这里是以`webpack://`开头+生成的source-map文件中的sources字段部分。
              },
              "type": "node"
          }
      ],
      "version": "0.1"
  }
  ```
  sourceMapPathOverrides: 键值对<br />
  `key`: 对应source-map中的sources字段(如下图:)
  
 <img src="/static/snapshots/node_development/3.jpg" />
 
  `value`: 可以使用`${workspaceFolder}`