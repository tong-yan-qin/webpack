​                                                                              

# npm

- node package manage(node包管理器)
- 通过npm命令安装jQuery包（npm install --save jquery），在安装时加上--save会主动生成说明书文件信息（将安装文件的信息添加到package.json里面）

### npm网站

> ​	npmjs.com	网站   是用来搜索npm包的

### npm命令行工具

npm是一个命令行工具，只要安装了node就已经安装了npm。

npm也有版本概念，可以通过`npm --version`来查看npm的版本

升级npm(自己升级自己)：

```
npm install --global npm
```

### 常用命令

npm init(生成package.json说明书文件) 

- npm init -y(可以跳过向导，快速生成)

npm install 

- 一次性把dependencies选项中的依赖项全部安装
- 简写（npm i）

npm install 包名 

- 只下载
- 简写（npm i 包名）

npm install --save 包名 

- 下载并且保存依赖项（package.json文件中的dependencies选项）
- 简写（npm i  包名）

npm uninstall 包名 

- 只删除，如果有依赖项会依然保存
- 简写（npm un 包名）

npm uninstall --save 包名 

- 删除的同时也会把依赖信息全部删除
- 简写（npm un 包名）

npm help 

- 查看使用帮助

npm 命令 --help 

- 查看具体命令的使用帮助（npm uninstall --help）

# webpack起步

webpack是模块化的打包工具，所以在webpack对文件静态资源打包之前必须模块化。

### 什么是模块化

- 文件作用域(模块是独立的，在不同的文件使用必须要重新引用)【在node中没有全局作用域，它是文件模块作用域】
- 通信规则 
  - 加载require
  - 导出exports

### CommonJS模块规范

在Node中的JavaScript还有一个重要的概念，模块系统。

- 模块作用域

- 使用require方法来加载模块

- 使用exports接口对象来导出模板中的成员

  ### 加载`require`

  语法：

  ```
  var 自定义变量名 = require('模块')
  ```

  作用：

  - 执行被加载模块中的代码
  - 得到被加载模块中的`exports`导出接口对象

  ### 导出`exports`

  - Node中是模块作用域，默认文件中所有的成员只在当前模块有效

  - 对于希望可以被其他模块访问到的成员，我们需要把这些公开的成员都挂载到`exports`接口对象中就可以了

    导出多个成员（必须在对象中）：

    ```
    exports.a = 123;
    exports.b = function(){
        console.log('bbb')
    };
    exports.c = {
        foo:"bar"
    };
    exports.d = 'hello';
    ```

    导出单个成员（拿到的就是函数，字符串）：

    ```
    module.exports = 'hello';
    ```

    以下情况会覆盖：

    ```
    module.exports = 'hello';
    //后者会覆盖前者
    module.exports = function add(x,y) {
        return x+y;
    }
    ```

    也可以通过以下方法来导出多个成员：

    ```
    module.exports = {
        foo = 'hello',
        add:function(){
            return x+y;
        }
    };
    ```

```
exports和module.exports的区别:
	每个模块中都有一个module对象
    module对象中有一个exports对象
    我们可以把需要导出的成员都挂载到module.exports接口对象中
	也就是`module.exports.xxx = xxx`的方式
    但是每次写太多了就很麻烦，所以Node为了简化代码，就在每一个模块中都提供了一个成员叫`exports`
    `exports === module.exports`结果为true,所以完全可以`exports.xxx = xxx`
    当一个模块需要导出单个成员的时候必须使用`module.exports = xxx`的方式，=,使用`exports = xxx`不管用,因为每个模块最终return的是module.exports,而exports只是module.exports的一个引用,所以`exports`即使重新赋值,也不会影响`module.exports`。
    有一种赋值方式比较特殊：`exports = module.exports`这个用来新建立引用关系的。
    
```



###           模块原理

  exports和`module.exports`的一个引用： 

```
console.log(exports === module.exports);	//true

exports.foo = 'bar';

//等价于
module.exports.foo = 'bar';
当给exports重新赋值后，exports！= module.exports.
最终return的是module.exports,无论exports中的成员是什么都没用。
真正去使用的时候：
	导出单个成员：exports.xxx = xxx;
	导出多个成员：module.exports 或者 modeule.exports = {};
```

### ES6模块化

**export**指令用于导出变量

```
export let name = 'why'
export let age = 15
export let height = 1.88
```

以上代码的另外一种写法

```
let name = 'why'
let age = 15
let height  = 1.88

export { name, age, hegith}


```

**export**指令用于导出函数或类

```
export function test( content) {
console.log( content)
}
```

```
export class Person {
constructor (name,age) {
this.name = name
this.age = age
}
run() {

console.log(this.name+'在奔跑‘)
}
}
```

### webpack安装

webpack 依赖node.js， 所以安装webpack之前需要安装node.js ,Node.js自带了软件包管理工具npm

1.全局安装(指定版本)

npm install webpack@3.6.0  -g  

2.局部安装（本地安装）

npm install webpack@3.6.0  --save-dev

3.webpack 打包 js文件

dist文件夹：用于存放之后打包的文件

src文件夹：用于存放我们写的源文件

```
webpack  src/mian.js  dist/bundle.js (执行此命令以后bundle.js 会自动生成不需要提前建立文件)
```

只要 src 文件中源文件变化 ，就得要重新执行打包命令使用webpack的命令都需要写上入口和出口作为参数，就非常麻烦。所以为了不写入口函数和出口函数。可以配置一个webpack.config.js 来指定出口和入口路径。

```
//加载文件路径模块
var path = require （path）//所以在项目中就需要path包，通过npm来装  1、npm init  -y 

module.export = {
//入口，可以是字符串，数组，对象，这里如果一个就写字符串
entry;  './src/main.js ',

//出口，通常是一个对象，里面至少包含两个重要属性，path、filename
output : {
//	path: './dist ’          path是一个相对路径   这样是错误的 所以需要nodejs中的path路径操作模块来辅助所以需要加载path模块

    path : path.resolve(_dirname,'dist')  // path 相对路径

 		filename：'bundle.js'

}
```

接下来在控制台可以直接使用webpack命令进行打包不用加上入口文件和出口文件

注意

如果想用 命令 npm run build 代替 命令webpack 打包  需要进行映射即 在package.json文件中的代码块 "scripts"中增加 “build"="webpack" 执行命令优先从本地中找webpack而不是局部的

###  path路径操作模块

> 参考文档：https://nodejs.org/docs/latest-v13.x/api/path.html

- path.basename：获取路径的文件名，默认包含扩展名
- path.dirname：获取路径中的目录部分
- path.extname：获取一个路径中的扩展名部分
- path.parse：把路径转换为对象 
  - root：根路径
  - dir：目录
  - base：包含后缀名的文件名
  - ext：后缀名
  - name：不包含后缀名的文件名
- path.join：拼接路径
- path.isAbsolute：判断一个路径是否为绝对路径

### Node中路径的其它成员  (__dirname,__filename)

在每个模块中，除了`require`,`exports`等模块相关的API之外，还有两个特殊的成员：

- `__dirname`，是一个成员，可以用来**动态**获取当前文件模块所属目录的绝对路径

- `__filename`，可以用来**动态**获取当前文件的绝对路径（包含文件名）

- `__dirname`和`filename`是不受执行node命令所属路径影响的

  在文件操作中，使用相对路径是不可靠的，因为node中文件操作的路径被设计为相对于执行node命令所处的路径。

所以为了解决这个问题，只需要把相对路径变为绝对路径（绝对路径不受任何影响）就可以了。

就可以使用`__dirname`或者`__filename`来帮助我们解决这个问题

在拼接路径的过程中，为了避免手动拼接带来的一些低级错误，推荐使用`path.join()`来辅助拼接

```
var fs = require('fs');
var path = require('path');

// console.log(__dirname + 'a.txt');
// path.join方法会将文件操作中的相对路径都统一的转为动态的绝对路径
fs.readFile(path.join(__dirname + '/a.txt'),'utf8',function(err,data){
	if(err){
		throw err
	}
	console.log(data);
});
```

> 补充：模块中的路径标识和这里的路径没关系，不受影响（就是相对于文件模块）

> **注意：**
>
> 模块中的路径标识和文件操作中的相对路径标识不一致
>
> **模块中的路径标识就是相对于当前文件模块，不受node命令所处路径影响**



### webpack 中的loader

用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖。但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等。对于webpack本身的能力来说，对于这些转化是不支持的。webpack扩展对应的loader就可以啦。

loader使用过程：https://webpack.js.org/concepts/

1.安装

```
npm install --save-dev css-loader

npm install --save-dev style-oader
```

2.在webpack.config.js中配置

```
module.export = {
module: {
rules:[
{
test:/'\.css$/,
//css-loader负责css文件进行加载
//style-loader将样式添加到dom中   他们之间有顺序关系 从右向左执行
use:['style-loader','css-loader']
	}
		]
			}
				}
```

3.在webpack打包入口文件中加

require（’要打包的css文件‘）



### webpack中css的预处理器（less、scss等）文件处理

1.安装

```
npm install --save-dev less-loader less
```

2.在webpack.config.js中配置

```
 {
        test: /\.less$/,
        use: [{
          loader: "style-loader", 
        }, {
          loader: "css-loader"
        }, {
          loader: "less-loader", 
        }]
      },
```

3.在webpack打包入口文件中加

require（’要打包的less文件‘）





### webpack图片文件处理

1.安装

```
npm install --save-dev url-loader
npm install --save-dev file-loader 大于限制大小时
```

2.在webpack.config.js中配置

```
{
        test: /\.(png|jpg|gif|jpeg)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              // 当加载的图片, 小于limit时, 会将图片编译成base64字符串形式.
              // 当加载的图片, 大于limit时, 需要使用file-loader模块进行加载.
              limit: 13000,
              //图片的命名
              name: 'img/[name].[hash:8].[ext]'
            },
          }
        ]
      },
```

注意：（index.html 不在dist文件夹下）

​       超过限制大小以文件的形式，则不能能够完全打包在webpack.config.js中配置的文件出口output中加入publicPath: 'dist/' 这个只要以url开头的文件前面自动加上前缀面dist/

3.在webpack打包入口文件中加

require（’要打包的文件‘）





### webpack 将ES6转为ES5

1.安装

```
npm install --save-dev babel-loader@7 babel-preset-es2015
```

2.配置webpack .config.js

```
 {
        test: /\.js$/,
        // exclude: 排除
        // include: 包含
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['es2015']
          }
        }
      }
```

### webpack 中如何配置Vue

1.安装

```
npm install vue   -save
```

使用Vue进行开发

HTML文件中

```
<div id="app">  
<h2>{{message}}</h2>
</div>
```

main.js文件中

```
import Vue from 'vue'

const vm = new Vue ({

	el:"#app",
	data:{
		message:'hello webpack'

})。。。。。。。。。。。。。。。。。。报错
因为Vue不同版本构建，runtime-only（代码中，不可以有任何的template）和runtime-compiler（代码中，可以有templata，因为compiler 可以用于编译template）。
```



2.进行配置webpack.config.js

```
module.exports =  {
	resolve: {
				alias： {
					'vue$':    'vue/dist/vue.esm.js
}
}
}
```

3.打包

4.希望将data中的数据显示在界面中，就必须是修改index.html,但是html模板在之后的开发中，我并不希望手动的来频繁修改，

在前面的Vue实例中，我们定义了el属性，用于和index.html中的#app进行绑定，让Vue实例之后可以管理它其中的内容,这里，我们可以将div元素中的{{message}}内容删掉，只保留一个基本的id为div的元素，然后在定义一个我们可以再定义一个template属性，代码如下：

HTML文件中

```
<div id="app"></div>
```

main.js文件中

	import Vue from 'vue'
	const vm = new Vue ({
	el:"#app",
	template: `
	<div >
	<h2>{{message}}</h2>
	</div>`
	data:{
		message:'hello webpack'
		})

5.webpack终极使用vue

```
import Vue from 'vue'
import App from './vue/app.vue'
new Vue ({
el: '#app',
template: '<APP/>',
componnents: {
APP
}
})
```

app.vue中

```
<template>
  <div>
    <h2 class="title">{{message}}</h2>
    <button @click="btnClick">按钮</button>
    <h2>{{name}}</h2>
    <Cpn/>
  </div>
</template>

<script>
  import Cpn from './Cpn'

  export default {
    name: "App",
    components: {
      Cpn
    },
    data() {
      return {
        message: 'Hello Webpack',
        name: 'coderwhy'
      }
    },
    methods: {
      btnClick() {

      }
    }
  }
</script>

<style scoped>
  .title {
    color: green;
  }
</style>

```

进行上面是无法打包需要

1.安装vue-loader和vue-tenplate-compiler --save-dev

npm install vue-loader vue-tenplate-compiler --save-dev

2.配置webpack.config.js

```
 {
  test: /\.vue$/,
   use: ['vue-loader']
      }
```

注意

如果报错

在package.json

1、将'vue-loader': '^13.0.0'     2、npm install

### webpack中plugin

**1、添加版权的plugin**

1）配置webpack.config.js

```
 const webpack =  require('webpack')
 plugins: [
    new webpack.BannerPlugin('最终版权归aaa所有')
    ]


```

2）打包



2、**打包html的plugin**

在真实发布项目时，发布的是dist文件夹中的内容，但是dist文件夹中如果没有index.html文件，那么打包的js等文件也就没有意义了。所以，我们需要将index.html文件打包到dist文件夹中，这个时候就可以使用HtmlWebpackPlugin插件

HtmlWebpackPlugin插件可以为我们做这些事情：

- 自动生成一个index.html文件(可以指定模板来生成)
- 将打包的js文件，自动通过script标签插入到body中

1）安装HtmlWebpackPlugin插件

 npm install html-webpack-plugin --save-dev

2）.修改webpack.config.js

```
const HtmlWebpackPlugin = require('html-webpack-plugin')
 plugins: [
      new webpack.BannerPlugin('最终版权归aaa所有'),
      new HtmlWebpackPlugin({
        template: 'index.html'//此HTML文件中只有<div id='app'></div>
      }),
  ],
```

3）.打包

出现一个问题 需要将publicPath:'/dist'注释掉

**3、js压缩的Plugin**

对打包的js文件进行压缩，我们使用一个第三方的插件uglifyjs-webpack-plugin，并且版本号指定1.1.1，和CLI2保持一致

1.安装

npm install uglifyjs-webpack-plugin@1.1.1 --save-dev

2.修改webpack.config.js

```
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

plugins: [
      new webpack.BannerPlugin('最终版权归aaa所有'),
      new HtmlWebpackPlugin({
        template: 'index.html'
      }),
      new UglifyjsWebpackPlugin()
  ],
```

### 搭建本地服务器

压缩以后，还要改代码则可以搭建本地服务器，webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。

1、安装

npm install --save-dev webpack-dev-server@2.9.1

2、修改webpack.config.js文件

```
devServer: {
    contentBase: './dist',//contentBase：为哪一个文件夹提供本地服务，默认是根文件夹，我们这里要填写./dist

    inline: true
  }
```

3、package.json中在scripts加入 “dev”: “webpack-dev-server --open”

​      终端命令webpack-dev-server 







### webpack配置文件的分离

发布进行js文集压缩，不要在开发时压缩 、发布时不要配置本地服务器了，所以需要配置文件的分离

1.dev.config.js放生产的配置

```
module.exports = {
devServer: {
    contentBase: './dist'
    inline: true
  }
}
```

2.prod.config.js

```
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
plugins: [
      new UglifyjsWebpackPlugin()
  ],
}
```

3.baseconfig.js

将文件进行合并

1.安装

npm install webpack-merge --save-dev

2.每个webpackconfig导入

```
const WebpackMerge = reqire ('webpackmerge ')
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const baseConfig = require ('./baseconfig.js')

module.exports =WebpackMerge  (baseconfig.js,  {plugins: [
      new UglifyjsWebpackPlugin()
  ],
})
```

此处省略。。。。。

3.报错解决

package.json中在scripts加入 “build”: “webpack --config .build/prod.config.js”

package.json中在scripts加入 “dev”: “webpack-dev-server --open --config  ./build/dev.config”

```
module.export = {
entry;  './src/main.js ',


output : { path : path.resolve(_dirname,**'../dist'**)  

 		filename：'bundle.js'

}
```

