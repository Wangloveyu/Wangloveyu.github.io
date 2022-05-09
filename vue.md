# vue篇

## webpack

### 前端工程化

#### 实际工作中的前端开发

模块化（js的模块化、css的模块化、资源的模块化）

组件化（复用现有的UI结构、样式、行为）

规范化（目录结构的划分、编码规范化、接口规范化、文档规范化、Git分支管理）

自动化（自动化构建、自动部署、自动化测试）

#### 前端工程化

前端工程化指的是在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。

企业中的vue项目和react项目都是基于工程化的方式进行开发的

前端工程化的好处：前端开发自成体系，有一套标准的开发方案和流程

#### 前端工程化的解决方案

早期的前端工程化解决方案（了解即可）

grunt、gulp

目前主流的前端工程化解决方案

webpack（https://www.webpackjs.com）

parcet（https://zh.parcetjs.org）

### webpack的基本使用

#### 什么是webpack

webpack是前端项目工程化的具体解决方案

主要功能：提供了友好前端模块化开发支持以及代码压缩混淆、处理浏览器端JavaScript的兼容性、性能优化等功能

好处：让程序员将工作中心放到具体功能的实现上，提高了前端开发效率和项目的可维护性

注意：目前vue、react等前端项目，基本都是基于webpack进行工程化开发的

#### 项目初始化步骤

1、新建项目空白目录，运行npm init -y命令，初始化包管理配置文件package.json

2、新建src源代码目录

3、在src中新建index.html和index.js文件

4、初始化首页基本的结构

5、运行npm i jquery -s命令，安装jQuery（-s即--save表示明确告诉npm需要将包的参数记录到package.json文件中）

6、通过ES6模块化的方式导入jq，实现列表隔行变色

ES6方法：import $ from ‘jquery’

##### webpack安装以及配置

###### 安装webpack

在终端中运行如下命令，安装webpack相关的两个包

npm install [webpack@5.42.1](mailto:webpack@5.42.1) [webpack-cli@4.7.2](mailto:webpack-cli@4.7.2) -D

```
npm install webpack@5.42.1 webpack-cli@4.9.0 -D
```

-D是--save-dev的缩写，表示将包记录到devDependencies节点中（即只在开发阶段用到的包）

-S是--save的缩写，表示将包记录到Dependencies节点中（在开发和部署阶段用到的包）

###### 在项目中配置webpack

1）在项目根目录中，创建名为`webpack.config.js`的webpack配置文件，并初始化如下的基本配置：

```
modul.exports = {
	mode:'development'	// mode 用来指定构建模式
}
```

> mode的可选值有两个，分别是：
>
> 1. development
>    - 开发环境
>    - 不会对打包生成的文件进行代码压缩和性能优化
>    - 打包速度快，适合在开发阶段使用
> 2. production
>    - 生产环境
>    - 会对打包生成的文件进行代码压缩和性能优化
>    - 打包速度很慢，仅适合在项目发布阶段使用
>
> 因此一定要视情况选择mode的值

> webpack.config.js是webpack的配置文件。webpack在真正开始打包构建之前，会先读取这个配置文件，从而基于给定的配置对项目进行打包
>
> 由于webpack是基于node.js开发出来的打包工具，因此在它的配置文件中，支持使用node.js相关的语法和模块进行webpack的个性化配置

2）在pack.json的scripts节点下，新增dev脚本如下：

```
“scripts":{
	"dev":"webpack"	// script节点下的脚本可以通过npm run执行，例如npm run dev，dev只是一个代码，可以更改，但是后面的webpack字符串不能改，因为运行这串代码即表示你利用webpack打包
}
```

使用`npm run dev`后在根目录下就会生成相关的文件，生成的文件既包含了源文件，还包含了被导入的包的内容

###### 自定义webpack打包的入口与出口

> 在webpack 4.x 和 5.x 的版本中，有如下的默认约定
>
> 1. 默认的打包入口文件为 src->index.js
> 2. 默认的输出文件路径为 dist->main.js
>
> 如果webpack找不到打包入口文件则会报错
>
> 可以在webpack.config.js中修改打包的默认约定

在webpack.config.js配置文件中，通过entry节点指定打包的入口。通过output节点指定打包的出口

```javascript
const path = require('path')

module.exports = {
	entry: path.join(__dirname, './src/入口文件名.js'),
	output: {
		path:path.join(__dirname, './dist'),	// 指定输出文件的存放路径
        // 一般将该文件保存到dist文件夹下的js文件夹中
		filename: './js/输出文件的名称.js'
	}
}
```

### 安装webpack中的插件

通过安装和配置第三方的插件，可以拓展webpack的能力，从而让webpack用起来更方便。最常用的webpack插件有如下两个：

1. webpack-dev-server
   - 类似于node.js阶段用到的nodemon工具
   - 每当修改了源代码，webpack会自动进行项目的打包和构建
2. html-webpack-plugin
   - webpack中的html插件（类似于一个模板引擎插件）
   - 可以通过此插件复制index.html到一个新的地址
3. clean-webpack-plugin
   - 该插件能够使webpack在每次打包的时候自动清理调dist目录中的旧文件
   - webpack5可以直接设置clean: true

##### webpack-dev-server

- 安装webpack-dev-server

运行如下命令，即可在项目中安装插件：

```
npm install webpack-dev-server@3.11.2 -D 
```

- 配置webpack-dev-server

1. 修改package.json -> scripts 中的dev命令如下

```
"scripts": {
	"dev": "webpack serve"
}
```

2. 再次运行`npm run dev`命令，重新进行项目的打包
3. 在浏览器中访问`http://localhost:8080`地址，然后进入src文件夹可以查看js的自动打包对网页造成的效果

> 打开网页中的文件夹会默认进入该文件夹中的index.html页面

> webpack-dev-server会在根目录生成一个输出文件，但是该文件是被存放在内存中的，便于修改，也避免频繁读写磁盘对磁盘造成损坏，并不会直接显示在文件夹或者网页中，但是你可以通过`http://localhost:8080/输出文件名.js`的url查看该文件

> webpack-dev-server并不是在本地打包，而是启动一个实时打包的http服务器在内存中进行模拟打包

- devServer节点

在webpack.config.js配置文件中，可以通过devServer节点对webpack-dev-server插件进行更多的配置

```
devServer: {
	open: true,	// 初次打包完成后，自动打开浏览器
	host: '127.0.0.1',	// 实时打包所使用的的主机地址
	port: 8080,	// 实时打包所使用的的端口号
}
```

> 该节点还是写在module.exports对象中
>
> 凡是修改了webpack.config.js配置文件或者package.json配置文件，必须重启实时打包的服务器，否则最新的配置文件无法生效

##### html-webpack-plugin

- 安装html-webpack-plugin

运行如下命令，即可在项目中安装插件：

```
npm install html-webpack-plugin@5.3.2 -D 
```

- 配置html-webpack-plugin

在webpack.config.js中配置如下信息

```
// 导入HTML插件，得到一个构造函数
const HtmlPlugin = require('html-webpack-plugin')

// 创建HTML插件的实例对象
const htmlPlugin = new HtmlPlugin({
	template: './src/index.html',// 指定源文件的存放路径
	filename: './index.html'	// 指定生成文件的存放路径
})

module.exports = {
	mode: 'development',
	plugins:[htmlPlugin]	// 通过plugins节点使htmlPlugin插件生效，注意这个节点中的p是小写的，最后有s，否则会报错
}
```

> 这个插件也是将复制的页面存到内存中的，并不会真实复制

该插件既会帮我们复制页面，也会帮我们自动引入内存中的输出文件（上个插件输出的）

##### clear-webpack-pilugin

- 安装clean-webpack-plugin

```
npm i clean-webpack-plugin@3.0.0 -D
```

- 在webpack.config.js中配置插件

```
// 导入插件，得到一个构造函数
const {CleanWebpackPlugin} = require('clean-webpack-plugin')

// 创建HTML插件的实例对象
const cleanPlugin = new CleanWebpackPlugin()

module.exports = {
	mode: 'development',
	plugins:[cleanPlugin]	// 通过plugins节点使插件生效
}
```

### webpack中的loader

#### loader概述

- 背景

在实际开发过程中，webpack默认只能打包处理以.js后缀名结尾的模块。其他的非.js后缀名结尾的模块，webpack默认处理不了，需要调用loader加载器才可以正常打包，否则会报错

- loader的作用

协助webpack打包处理特定的文件模块，例如

1. css-loader可以打包处理.css相关的文件
2. less-loader可以打包处理.less相关的文件
3. babel-loader可以打包处理webpack无法处理的高级JS语法
4. url-loader可以打包处理样式表中与url路径相关的文件（例如jpg、png、gif）

![image-20220406212112873](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220406212112873.png)

> 注意：在webpack中，一切皆模块，都可以通过ES6导入语法进行导入和使用，因此我们可以在js文件中使用`import './css/index.css'`语句导入样式，也可以使用`import logo from './logo.jpg'`语句引入图片，这里logo的值是图片路径
>
> 注意：当在js中引入css样式后，如果直接使用webpack打包该js会报错，需要使用css-loader进行加载

#### 如何使用loader

- 安装loader

```
// 安装css加载器，注意，一共有两个，style-loader和css-loader
npm i style-loader@3.0.0 css-loader@5.2.6 -D

// 安装less加载器，注意，之所以安装后面的less是因为less加载器依赖于less运行的，所以必须先加载该环境
npm i less-loader@10.0.1 less@4.1.1 -D

// 安装url路径相关加载器，file-loader也是一个内置依赖项，在下面也不需要导入
npm i url-loader@4.1.1 file-loader@6.2.0 -D

// 安装高级js加载器，注意后两个是依赖
npm i babel-loader@8.2.2 @babel/core@7.14.6 @babel/plugin-proposal-decorators@7.14.5 -D
```

- 配置loader

在webpack.config.js的module.exports->module->rule数组中，添加loader规则如下

> webpack会在处理不了文件时查看该数组判断是否配置了对应的loader加载器

```
module.exports = {
	module:{
        rules:[	// 文件后缀名的匹配规则
            {
                test: /\.css$/,
                use: [
                	// css加载器
                    'style-loader',
                    'css-loader',
                ]
            },
            {
                test: /\.less$/,
                use: [
                	// 注意此处三个加载器都必须运行，因为less会转化成css，还需要css加载器进行解析
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
            },
            {
            	// 除了下面三种类型，还可以是其他的图片类型
                test: /\.jpg|png|gif$/,
                use: ['url-loader'],
                // options中的内容也可以在use中改为查询字符串的形式，例如use: ['url-loader?limit=22229&outputPath=images']
                options:{
                	// 限定图片转化大小，单位是字节，只有大小不大于这个限制的才会被转化为base64格式
                	limit=22229,
                	// 指定图片文件的输出路径，此时该路径为dist/image/
                	outputPath:'images'
                }
            },
            {
                test: /\.js$/,
                use: ['babel-loader'],
                // 注意：必须使用exclude指定排除项，因为node_modules目录下的第三方包不需要被打包且兼容性不需要我们关心
                exclude: /node_modules/
            }
        ]
    }
}
```

> 注意rules是写在module.exports->module中！！！不是写在module.exports或者module中！！！
>
> test表示匹配的文件类型，use表示对应要调用的loader
>
> url-loader加载器的原理是将图片文件转化成base64字符串
>
> 注意：base64一般会比原图片大，因此只推荐小图片使用，大图片不推荐使用，可以用limit进行限制
>
> 注意：
>
> 1. use数组中指定的loader顺序是固定的
> 2. 多个loader的调用顺序是**从后往前**调用
> 3. 每个loader处理完文件后，会将文件转交给下一个loader
> 4. 当所有文件都被处理好后，webpack会把style-loader处理的结果合并到输出文件中，最终生成打包好的文件

- 配置babel-loader

在项目根目录下创建名为babel.config.js的配置文件，定义babel的配置项如下：

```
module.exports = {
	plugins:[['@babel/plugin-proposal-decorators',{legacy:true}]]
}
```

### 打包发布

#### 为什么要打包发布

项目开发完成后，需要使用webpack对项目进行打包发布，主要原因如下：

1. 开发环境下，打包生成的文件存放于内存中，无法获取到最终打包生成的文件
2. 开发环境下，打包生成的文件不会进行代码压缩和性能优化

为了让项目能够在生产环境中高性能的运行，因此需要对项目进行打包发布

#### 配置webpack的打包发布

在packagejson文件的scripts节点下，新增build命令如下

```
"scripts": {
    // 开发环境中执行dev命令即可
    "dev": "webpack serve",
    // 项目发布时运行build命令
    "build": "webpack --mode production"
},
```

> --mode是一个参数项，用来指定webpack的运行模式。production代表生产环境，会对打包生成的文件进行代码压缩和性能优化
>
> 注意：通过--mode 指定的参数项，会覆盖webpack.config.js中的mode选项

#### 企业级项目的打包发布

企业级项目的打包发布流程为：

1. 生成打包报告，根据报告分析具体的优化方案
2. Tree-Shaking
3. 为第三方库启用CDN加载
4. 配置组件的按需加载
5. 开启路由懒加载
6. 自定制首页内容

> 更多的内容见项目

### Source Map

#### 什么是Source Map

Source Map就是一个信息文件，里面储存着位置信息，也就是说Source Map文件中存储这压缩混淆后的代码所对应的转换前的位置

有了它，在出错的时候出错工具将直接显示原始代码，而不是转化后的代码，能够极大的方便后期的调试

#### 默认Source Map的问题

开发环境下默认生成的Source Map，记录的是生成后代码的位置，会导致运行时报错的行数与源代码的行数不一致的问题

#### 解决默认Source Map的问题

开发环境下，推荐在webpack.config.js中添加如下的配置，即可保证运行时报错的行数与源代码的行数保持一致

```
module.exports = {
	mode: 'development',
	devtool: 'eval-source-map'
}
```

> 注意：在发布项目的时候处于安全考虑要关掉该选项

在生产环境下，如果只想定位报错的具体行数而不暴露源码。此时可以将devtool的值设置为`nosources-source-map`

> 采用此选项后：应该将服务器配置为不允许普通用户访问source map文件



在生产环境下，如果定位报错的具体行数且暴露源码。此时可以将devtool的值设置为`source-map`

### 小拓展

建议使用@符号表示src源代码目录，从外往里查找，不要使用../从里往外查找

> 注意：虽然@好用，但是必须先在webpack.config.js中配置，配置方式见下

```
module.exports = {
	resolve: {
        alias: {
        	// 告诉webpack，程序员写的代码中@符号表示src这一层目录
            '@': path.join(__dirname, './src')
        }
    }
}
```

### 实际开发中不需要自己配置webpack！！！

- 实际开发中会使用命令行工具意见生成带有webpack的项目
- 该项目开箱即用，所有的webpack配置项都是现成的
- 但是我们还是需要了解webpack中的基本概念

### 在chrome中安装vue-devtools调试工具

#### 安装vue-devtools

直接翻墙到谷歌商店里面下载吧，其他地方的经常报错

#### 配置插件

在插件中详情面板中（通过拓展管理进入），打开允许访问文件网址，即配置完成

## vue

### vue简介

#### 什么是vue

vue是一套用于构建用户界面的前端框架

> 构建用户界面即用vue往html页面中填充数据，非常方便
>
> 框架是一套现成的解决方案，程序员只能遵守框架的规范去编写自己的业务功能，学习vue就是在学习vue框架中规定的语法

#### vue的特性

1. 数据驱动视图
   1. 在使用了vue的页面中，vue会监听数据的变化，从而自动重新渲染页面的结构
   2. 好处：当页面数据发生变化时，页面会自动重新渲染
   3. 注意：数据驱动视图时单向的数据绑定
2. 双向数据绑定
   1. 在填写表单时，双向数据绑定可以辅助开发者在不操作DOM的前提下，自动把用户填写的内容同步到数据源中
   2. 好处：开发者不再需要手动操作DOM元素来获取表单元素最新的值

#### MVVM

MVVM是vue实现数据驱动视图和双向数据绑定的核心原理

MVVM指的是Model、View和ViewModel，它把每个HTML页面都拆分成了这三个部分

在MVVM概念中：

Model表示当前页面渲染时所依赖的数据源

View表示当前页面所渲染的DOM结构

ViewModal表示vue的实例，它是MVVM的核心

#### MVVM的工作原理

ViewModel作为MVVM的核心，是它把当前页面的数据源（Modal）和页面的结构（View）连接在了一起

当数据源发生变化时，会被ViewModal监听到，VM会根据罪行的数据源自动更新页面的结构

当表单元素的值发生变化时，也会被VM监听到，VM会把变化过后最新的值自动同步到Model数据源中

### vue的基本使用

#### 基本使用步骤

1. 导入vue.js的script脚本文件
2. 在页面中声明一个将要被vue所控制的DOM区域
3. 创建vm实例对象

实例

```html
<body>
    <!--2、在页面中声明一个将要被vue所控制的DOM区域-->
    <div id="app">
        {{username}}
    </div>
    <script src="./vue路径"></script>
    <script>
        // 3、创建vm实例对象
    	const vm = new Vue({
            // 3.1、指定当前vm实例要控制页面的哪个区域，el属性是固定的写法，表示当前vm实例要控制页面上的哪一个区域，接受的值是一个选择器
            el: '#app',
            // 3.2、指定Model数据源，data对象就是要渲染到页面上的数据
            data: {
                username: 'zs'
            }
        })
    </script>
</body>
```



### vue的调试工具

略

### vue的指令与过滤器

#### 指令

##### 指令的概念

指令是vue为开发者提供的模板语法，用于辅助开发者渲染页面的基本结构

vue中的指令按照不同的用途可以分为如下6大类

1. 内容渲染指令
2. 属性绑定指令
3. 事件绑定指令
4. 双向绑定指令
5. 条件渲染指令
6. 列表渲染指令

> 指令是vue开发中最基础、最常用、最简单的知识点

##### 内容渲染指令

内容渲染指令是用来辅助开发者渲染DOM元素的文本内容的

常用的内容渲染指令有如下3个

- v-text
- {{}}
- v-html

###### v-text

- 用法示例

```vue
<div id="app">
    <p v-text="username"></p>
    <!--下方的性别会被覆盖-->
    <p v-text="gender">性别</p>
</div>

...	<!--其余配置见上-->
```

> v-text会将标签中原本的内容覆盖
>
> v-text无法渲染html标签，而是会直接渲染成普通字符串

###### {{}}

> vue提供的{{}}语法是专门用来解决v-text会覆盖默认文本内容的指令
>
> {{}}也无法渲染html标签，而是会直接渲染成普通字符串
>
> 插值表达式只能用于内容节点中，不能用于属性节点

这种{{}}语法的专业名称是**插值表达式**（Mustache）

使用方法略

###### v-html

> v-html指令是专门用来解决v-text指令和插值表达式只能渲染纯文本内容，无法渲染html标签的问题的

- 用法示例

```html
<div id="app">
    <p v-html="discription"></p>
</div>

<script src="..."></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            description:"<a>这是一个超链接</a>"
        }
    })
</script>
```

##### 属性绑定指令

如果需要为元素的属性动态绑定属性值，则需要使用`v-bind`属性绑定指令

> vue规定v-bind指令可以简写为:即可
>
> v-bind属性中写的是js，因此如果要绑定一个固定字符串值的话需要在""中将字符串用''隔起来

- 用法示例

```html
<div id="app">
    <!--下面两种写法都允许，推荐第一种写法，更简洁-->
	<input type="text" :placeholder="tips"/>
	<input type="text" v-bind:placeholder="tips"/>
</div>
<script src="./lib/vue.js"></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            tips:"请输入"
        }
    })
</script>
```

> 指令除了直接使用外，还支持在指令中进行简单的运算或者使用JavaScript表达式的运算
>
> 例如：
>
> {{number+1}}、message.split('')、v-bind id="'list' + id"

##### 事件绑定指令

> vue提供了v-on:事件绑定指令，用来辅助程序员为DOM元素绑定事件监听
>
> v-on:可以简写为@

- 修改对象属性

在被绑定的事件中，可以使用`this.属性名`或者`vue对象名.属性名`的的方式查看或修改变量内容，其中第二种方式不推荐

- 事件传参

可以使用函数名(值)的形式传递参数

其中，如果没有传参的时候，函数默认有一个参数e，e中存放的是触发当前事件的dom元素，但如果传参了，则会覆盖e参数

> 如果不传参一定要写成add的形式，不能写成add()

可以在html中使用`$event`作为参数传入事件中，`$event`是vue提供的原生的DOM的事件对象

> $event在实际开发中并不是很常用

- 事件修饰符

vue提供了事件修饰符来使程序员更方便的对事件的触发进行控制

使用语法： `@事件名.prevent="需要绑定的函数"`

例如： `@click.prevent="add"`

常用的5个事件修饰符

| 事件修饰符 |                        说明                        |
| :--------: | :------------------------------------------------: |
|  .prevent  |    阻止默认行为(例如a标签的跳转，表单的提交等)     |
|   .stop    |                    阻止事件冒泡                    |
|  .capture  |          以捕获模式触发当前的事件处理函数          |
|   .once    |                绑定的事件只触发1次                 |
|   .self    | 只有在event.target是当前元素自身时触发事件处理函数 |

- 按键修饰符

vue提供了按键修饰符来监听键盘事件，使得我们不需要判断详细的按键

语法：`@keyup.esc="函数名"`表示用户输入了esc键时才触发该事件

> 连续使用多个按键修饰符为组合键功能

- 用法示例

```html
<div id="app">
    <p>
        count的值是：{{count}}
    </p>
    <!-- 不传参，一定要写成如下形式才有e参数，如果写成add()则不会有该参数-->
    <button @click="add(2,$event)">+1</button>
    <!--
	还可以写成如下形式
	<button v-on:click="add(2)">+1</button>
	-->
</div>


<script src="./lib/vue-2.6.12.js"></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            count:0
        },
        // methods的作用就是定义事件处理函数
        methods:{
            add:function(n, e){
                console.log(e)
                this.count+=n
                // vm.count+=n // 此方法不推荐
            }
            /*
            在ES6中还可以写成：
            add(n)
        	{
        		this.count+=n
    		}
    		*/
        }
    })
</script>
```

##### 双向绑定指令

vue提供了`v-model`双向数据绑定指令，用来辅助开发者在不操作DOM的前提下，快速获取表单的数据

> 注意：只有在表单元素中才能使用双向绑定
>
> 注意：select标签也是表单元素，双向绑定指令要绑定在select标签中，而不是绑定在option标签中，其值为option中value的值

- `v-model`修饰符 

为方便对用户输入的内容进行处理，vue为v-model指令提供了3个修饰符，分别是

| 修饰符  |              作用              |
| :-----: | :----------------------------: |
| .number | 自动将用户的输入值转为字符类型 |
|  .trim  | 自动过滤用户输入的首位空白字符 |
|  .lazy  | 在“change"时而非"input"时更新  |

- 用法示例

```html
<div id="app">
    <p>用户的名字是：{{usename}}</p>
	<input type="text" v-model="username"/>
</div>
<script src="./lib/vue.js"></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            username:"张三"
        }
    })
</script>
```

##### 条件渲染指令

条件渲染指令用来辅助开发者按需控制DOM元素的显示与隐藏。条件渲染指令有如下两个：

1. v-if
2. v-show

v-if具有配套指令`v-else-if`和`v-else`，具体用法略

> v-show的原理是动态为元素添加display：none样式来实现元素的显示和隐藏
>
> v-if的原理是每次动态创建或移除元素，来实现元素的显示和隐藏
>
> 如果要频繁切换元素的显示状态用v-show更好
>
> 如果刚进入页面的时候，某些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示，此时v-if性能更好

- 用法示例

```html
<div id="app">
    <p v-if="test === 200">显示</p>
	<p v-show="test === 200">显示</p>
</div>
<script src="./lib/vue.js"></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            test: 200
        }
    })
</script>
```

##### 列表渲染指令

vue提供了v-for列表渲染指令，用来辅助开发者基于一个数组来循环渲染一个列表结构

v-for指令需要使用item in items形式的特殊语法，其中

1. items是待循环的数组
2. item是被循环的每一项

> v-for指令还支持一个可选的第二个参数，即当前项的索引项，语法为(item, index) in items
>
> 注意index是从0开始的
>
> item和index还可以在属性中使用

> 官方建议：只要用到了v-for指令，那么一定要绑定一个`:key`属性，而且尽量把id作为key的值

- key值的注意事项

1. key的值只能是字符串或数字类型
2. key的值必须具有唯一性
3. 建议把数据项id属性的值作为key值（因为id属性值具有唯一性）
4. 使用index的值当做key没有任何意义（因为index的值不具有唯一性，唯一性指的不是不重复，而是值和内容有一个强制绑定关系）
5. 使用v-for指令时一定要指定key的值（既能提升性能，又能防止列表状态紊乱（主要在于状态更新的时候能够绑定更新状态）

- 用法示例

```html
<ul id="app">
    <li v-for="(item, index) in list" :key="item.id">第{{index}}个名字是{{item.name}}</li>
</ul>
<script src="./lib/vue.js"></script>
<script>
	const vm = new Vue({
        el: "#app",	// 这个vm示例只会控制第一个匹配的标签
        data:{
            list: [
            {
                id:1,
                name:"zs"
            },{
                id:2,
                name:"ls"
            },{
                id:3,
                name:"ww"
            }]
        }
    })
</script>
```

#### 过滤器（vue3已将其淘汰）

> 注意：在vue3中已经将过滤器模块删除了，因此过滤器只能在vue2中使用，因此此部分内容仅仅介绍基本内容

##### 过滤器基础

过滤器（Filters）是vue为开发者提供的功能，常用于文本的格式化。过滤器可以用在两个地方：插值表达式和v-bind属性绑定

过滤器应该被添加在js表达式的尾部，由**“管道符”**进行调用

> 过滤器本质上是一个函数，过滤器使用之前要先在filters中进行定义。
>
> 过滤器使得最终网页中显示的结果为函数的返回值
>
> 过滤器中一定要有返回值！！！

- 用法示例

```html
<div id="app">
    <!-- 在插值表达式中通过“管道符”调用capitalize过滤器，对message的值进行格式化 -->
    <p>
        {{message | capitalize}}
    </p>

	<!-- 在插值表达式中通过“管道符”调用formatId过滤器，对rawId的值进行格式化 -->
    <div v-bind:id="rawId | formatId"></div>
</div>
<script src="./lib/vue-2.6.12.js"></script>
<script>
const vm = new Vue({
    el: "#app",	// 这个vm示例只会控制第一个匹配的标签
    data:{
        message: "hello vue.js"
    },
    filters: {
        // 注意：过滤器函数形参前面的val永远都是管道符前面的那个值
        capitalize(val){
            // 字符串的charAt方法接受索引值，表示从字符串中获取索引对应的字符
            const first = val.charAt(0).toUpperCase()
            // 字符串的slice可以截取字符串
            const other = val.slice(1)	
            // 过滤器中一定要有返回值!!!
            return first + other
        }
    }
})
</script>
```

##### 私有过滤器和全局过滤器

- 私有过滤器

私有过滤器即上面所展示的过滤器，在局部的vue作用域中定义，仅作用域局部的vue区域

- 全局过滤器

全局过滤器则独立于每个vm实例之外，可以利用vue.filter()方法构建

用法示例

```
Vue.filter('过滤器名',(参数名)=>{
	return 返回的内容
})
```

##### 过滤器注意点

1. 过滤器要定义到filters节点下，本质是一个函数
2. 在过滤器函数中，一定要有return值
3. 在过滤器的形参中第一个参数一定是管道符前面待处理的那个值，如果要传参则参数会从形参列表第二个形参开始传递
4. 如果全局过滤器和私有过滤器名字一致，私有过滤器会覆盖全局过滤器
5. 过滤器可以连续调用，即将多个过滤器用管道符连接在一起，表示前面处理的结果作为形参传递给下一个过滤器

### 侦听器

#### 什么是watch侦听器

watch侦听器允许开发者监视数据的变化，从而针对数据的变化做特定的操作

> 侦听器本质上是一个函数，要侦听数据的变化，就把数据名改为方法名即可
>
> 所有的侦听器都应该被定义到watch节点下
>
> 侦听器中第一个参数是新值，第二个参数是旧值

- 用法示例

```javascript
const vm = new Vue({
    el: "#app",	// 这个vm示例只会控制第一个匹配的标签
    data:{
        username: "hello vue.js"
    },
    watch: {
		username(newVal, oldVal){
            console.log(newVal, oldVal)
        }
	}
})
```

#### 侦听器的格式

1. 方法格式的侦听器
   - 缺点1：无法在刚进入页面的时候自动触发
   - 缺点2：如果侦听的是一个对象，如果对象中属性发生了变化，不会触发侦听器
2. 对象格式的侦听器
   - 好处1：可以通过immediate选项让侦听器自动触发
   - 好处2：可以通过deep选项让侦听器深度侦听对象中属性的变化

> 深度侦听的概念：如果侦听的是对象格式的数据，那么对象里面属性的变化无法被侦听器侦听到

#### 对象格式的侦听器

```
const vm = new Vue({
    el: "#app",	// 这个vm示例只会控制第一个匹配的标签
    data:{
        info: {
        	username: "admin"
    	}
    },
    watch: {
		info:{
			// 侦听器的处理函数，一定为handler函数
            handler(newVal, oldVal){
            	 console.log(newVal.username, oldVal.username)
            },
            immediate:true,	// 表示一进入页面就触发一次
            // 开启深度侦听，只要对象中任何一个属性发生变化了，都会开启侦听器
            deep: true	
        },
        // 如果要侦听的是对象子属性的变化，还可以采用如下方法，注意必须包裹一层单引号
        'info.username'(newVal){
			console.log(newVal)	
        }
	}
})
```

### 计算属性

- 什么是计算属性

计算属性是指通过一系列运算之后最终得到的一个属性值

这个动态计算出来的属性值可以被模板结构或methods方法使用

- 特点

1. 计算属性在定义的时候要被定义为方法
2. 在使用计算属性的时候，当普通的属性使用即可

- 好处

1. 实现了代码的复用
2. 只要计算属性中依赖的数据源变化了，则计算属性会自动重新求值

- 用法示例

```javascript
const vm = new Vue({
    el: "#app",	// 这个vm示例只会控制第一个匹配的标签
    data:{
        r:0,
		g:0,
		b:0
    },
	computed: {
        rgb(){return `rgb(${this.r},${this.g},${this.b})`}
    },
    methods: {
        show(){
            console.log(this.rgb)
        }
    }
})
```

### 单页面应用程序

单页面应用程序（single page application）简称SPA，指的是一个web网站中只有唯一的一个HTML页面，所有的功能与交互都在这唯一的一个页面内完成

#### 如何快速创建vue的SPA项目

vue官方提供了两种快速创建工程化的SPA项目的方式

1. 基于vite创建SPA项目
2. 基于vue-cli创建SPA项目

|                            |       vite       |   vue-cli    |
| :------------------------: | :--------------: | :----------: |
|       支持的vue版本        |   仅支持vue3.x   | 支持3.x和2.x |
|      是否基于webpack       |        否        |      是      |
|          运行速度          |        快        |     较慢     |
|         功能完整度         | 小而巧(主键完善) |    大而全    |
| 是否建议在企业级开发中使用 |    目前不建议    |     建议     |

### vue-cli

#### 什么是vue-cli

vue-cli是vue.js开发的标准工具，它简化了程序员基于webpack创建工程化的vue项目的过程

vue-cli使得程序员可以专注在撰写应用上，而不必花好几天去纠结webpack配置的问题

中文官网：https://cli.vuejs.org/zh/

#### vue-cli的安装和使用

vue-cli是npm上的一个全局包，使用如下命令可以方便的把它安装到自己的电脑上

```
npm i -g @vue/cli
```

> 使用`vue -V`命令可以检测是否成功安装

#### 解决windows PowerShell不识别vue命令的问题

默认情况下，在powershell中执行vue --version命令会报错

解决方案如下

1. 以管理员身份运行powershell
2. 执行 `set-ExecutionPolicy RemoteSigned`命令，输入字符Y，回车即可

#### 基于vue-cli快速生成工程化的vue项目

在需要创建工程的目录下输入如下命令创建工程

```
vue create 项目名称
或
vue ui // 基于可视化面板创建vue项目
```

之后会弹出选择界面，如图所示

![image-20220408212130903](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408212130903.png)

表示需要程序员选择一个预设，使用上下箭头切换，其中前两个预设只要选择后回车就会立即创建项目，第三项表示手动选择要安装的功能

> 实际开发中建议选择第三项，定制功能

选择第三项后会出现如下界面

![image-20220408212343245](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408212343245.png)

在这个界面中按空格选择，按enter继续下一步

- babel为解决高级语法兼容性的中间件，一定要装

- TypeScript
- Progressive Web App Support 渐进式web框架支持
- Router 路由
- Vuex
- CSS Pre-processors CSS预处理器，即less
- Linter / Formatter 即ESLint用来约束团队代码风格，初学者尽量别选，否则经常报错
- Unit Testing 对组件进行单元测试
- E2E Testing 

> 初学者建议只装Babel、CSS Pre-processors

确定后需要选择vue的版本，选择之后会出现如下页面

![image-20220408213132150](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408213132150.png)

这个界面是让你选择css预处理器的，目前选择less即可，然后出现如下界面

![image-20220408213217307](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408213217307.png)

该界面询问第三方插件的创建方式

默认选择第一项（独立配置），如果选择第二项，package.json会很杂很乱，因此一定要选择第一项

最后会跳出这最后一个框

![image-20220408213335976](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408213335976.png)

这个页面询问是否将这个预设保存，视情况选择即可

进行完如上步骤后，vue-cli就正式创建了，并自动下载需要的包

> 注意：如果窗口意外被冻结，可以使用ctrl+c进行解冻

项目创建完毕后将出现如下页面

![image-20220408213704590](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20220408213704590.png)

其中`test`为自定义的项目名称，`npm run serve`就是自动打包js文件的快捷键

### Vite

#### 创建vite项目

按照顺序执行如下命令即可基于Vite创建vue3.x的工程化项目

```
npm init vite-app 项目名称

cd 项目名称
npm install
npm run dev
```

### vue中项目的构成

两种方式创建出来的vue项目中内容的构成均相同

在src目录下有四个文件/文件夹

- assets文件夹：该文件夹中存放在项目中用到的静态资源文件，例如css样式表，图片资源
- components文件夹：程序员封装的、可复用的组件都要放到components目录下
- main.js文件是项目的入口文件，整个项目的运行要先执行main.js文件
- App.vue是项目的根组件

> render函数中渲染的是哪一个组件，哪一个组件就叫做根组件

在public目录下有index.html

### vue项目的运行流程

在工程化的项目中，vue要做的事情很单纯：通过main.js把App.vue渲染到index.html的指定区域汇总

其中

1. App.vue用来编写带渲染的模板结构
2. index.html中需要预留一个el区域
3. main.js把App.vue渲染到了index.html所预留的区域中

### 组件的基本使用

- vue2.x中

```vue
// 导入vue包，得到Vue构造函数
import Vue from 'vue'
// 导入App.vue根组件，将来要把App.vue中的模板结构渲染到HTML页面中
import App from './App.vue'

// 创建Vue的实例对象
new Vue({
	// 把render函数指定的组件渲染到HTML页面中
	render: h =>h(App)
}).$mount('#app')
```

> vue中render渲染方法本质上是直接将组件替换掉所选择的html上的标签

> vue实例具有$mount()方法，其作用和el属性完全一样，即el与%mount()方法可以互相代替

> vue中很少使用html文件，而是直接使用工程化文件进行渲染

- vue3.x中

```
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

### Vue组件

#### 组件化开发

组件化开发指的是：根据封装的思想，把页面上可重用的UI结构封装为组件，从而方便项目的开发和维护

#### vue中的组件化开发

vue是一个支持组件化开发的前端框架

vue中规定：组件的后缀名是`.vue`

#### vue组件中的三个组成部分

每个.vue组件都由3部分构成，分别是：

- template->组件的模板结构
- script->组件的javascript行为
- style->组件的样式

> 三个组成部分尽量按照template、script、style的顺序依次存放

> 每一个vue文件中的模板并不是直接生成html的，而是先交给JavaScript，由JavaScript进行渲染

> 在最终的界面上所呈现的组件都是独立的实例，而vue中的仅仅只是模板结构，可以理解为构造函数与对象的区别

下方是一个完整的.vue文件的示例，请注意观察其中注释的讲解

```html
// 在template中，只能由一个div标签作为根节点，即不能同时有两个div在最外层，该标签也被称为唯一根节点
<template>
	<div class="test-box">
        <h3>
            这是用户自定义的Test.vue --- {{ username }}
        </h3>
        <button @click="changeName">
            修改用户名
        </button>
    </div>
</template>
<script>
// 默认导出，注意，这是一个固定写法!!!
export default {
    name: '组件名称', // 注意，如果在编写组件模板的时候没有指定组件名称，则组件在使用的时候使用注册时候的名称即可，但如果指定，则一定是name中的名称
    /* 
    data数据源
    注意：.vue组件中的data不能像之前一样指向对象
    组件中的data必须是一个函数，其返回值为一个数据对象，页面的数据应该在该返回值对象中定义
    */
    data() {
        return {
            username: 'admin'
        }
    },
    // 在这里定义当前组件中的方法
    methods: {
        changeName() {
            // 此处的this为组件的实例而不是Vue对象的实例，此处并没有定义Vue对象
            this.username = "测试用户名"
        }
    },
    watch() {
		// 在此处书写当前组件中的侦听器
    },
    computed() {
		// 在此处书写当前组件中的计算属性
    },
    filters() {
		// 在此处书写当前组件中的过滤器
    }
}
</script>


// 在style标签中书写样式，其中lang="less"表示启用了less语法，如果不写则默认为css语法
<style lang="less">
    .test-box {
		margin: 0 auto;
        background-color: pink
    }
</style>
```

#### 组件之间的父子关系

组件在被封装好之后，彼此之间是相互独立的，不存在父子关系

在使用组件的时候，根据彼此的嵌套关系才会形成父子关系、兄弟关系

#### 在组件中使用组件的步骤

##### 注册私有子组件

> 通过components注册的组件是私有子组件

1. 在组件的script标签中使用import语法导入需要的组件
2. 在`export default`对象中使用components节点注册组件
3. 以标签形式使用刚才注册的主键

- 用法示例

在父组件中注册私有子组件

```html
<template>
	<h3>
        测试导入组件
    </h3>
    <div>
        // 以标签形式使用刚刚注册的组件
        <left></left>
        <right></right>
    </div>
</template>

<script>
    // vue-cli默认帮我们配置好了@符号
	import Left from '@/components/Left.vue'
    import Right from '@/components/Right.vue'
    export default {
        components: {	// 注意：键值可以省略
            'Left': Left,
            'Right': Right
        }
    }
</script>
```

##### 注册全局组件

- vue2.x中

在vue项目的main.js入口文件中，通过`Vue.component()`方法注册全局组件

```js
import Vue from 'vue'
import App from './App.vue'
import Left from './components/Left.vue'

Vue.component('Left', Left)

new Vue({
  render: h => h(App)
}).$mount('#app')
```

- vue3.x

```js
import { createApp } from 'vue'
import App from './App.vue'
import Left from './components/Left.vue'

const app = createApp(App)

app.component('Left', Left)

app.mount('#app')
```



- 用法示例

直接在main.js中写入一下内容

```
// 导入需要全局注册的组件
import Count from '@/components/Count.vue'

// 参数1，字符串格式，表示组件的注册名称
// 参数2，需要被全局注册的那个组件
Vue.component('MyCount', Count)
```

> 注册组件的时候组件名称尽量都是大写开头

##### 组件注册时名称的大小写

在进行组件的注册时，定义组件注册名称的方式有两种

1. 使用 kebab-case 命名法(俗称短横线命名法，例如 my-swiper)
   - 特点：必须严格按照短横线名称使用
2. PascalCase命名法(俗称大驼峰命名法，例如MySwiper)

> 更推荐使用大驼峰命名法

##### 组件的注册名称和name名称的区别

组件的注册名称为**注册时**所使用的**键值**或使用import导入时所赋值的名称(默认情况)，该名称应用于以**标签的形式**将组件渲染到页面结构中

> 如果组件定义了name，那么组件的注册名称一定是name属性的值

组件的name名称指的是在定义组件模板的时候所指定的名称，该名称的使用场景为

1. 结合`<keep-alive>`标签实现组件缓存功能
2. **调试工具中**显示的组件名称

#### 组件的props

组件封装的基本原则

为了提高组件的复用性，在封装vue组件时要遵守如下原则

1. 组件的DOM结构、style样式要尽量复用
2. 组件中要展示的数据尽量由组件使用者提供

props是组件的自定义属性，在封装通用组件的时候，合理地使用props可以极大的提高组件的复用性

> 如果直接写`自定义属性="值"`的形式，传入的值为字符串，如果使用v-bind绑定，即`:自定义属性="值"`的写法时，传入的是js值，即数字如果没有用''隔起来，则表示的是数值

> props中的属性是只读的，不能直接修改

> props中的属性可以被赋一个默认值

- 语法格式

vue2.x

```javascript
// 下方写法表示传入的是字符串
<组件名 自定义属性A="传入的值"></组件名>
或者
// 下方写法表示传入的是数值
<组件名 :自定义属性A="传入的值"></组件名>

export default {
	// 组件的自定义属性props，允许使用者通过自定义属性为当前组件指定初始值，这些传入的值会直接被绑定到组件的vue对象身上，可以使用this调用
    
    // 数组写法,只能接受数据，不能设置默认值
	/*props: ["自定义属性A", "自定义属性B", "其他自定义属性"],*/
    
    // 对象写法，接受数据并设置默认值
    props: {
        自定义属性A:{
            /*配置选项*/
            // 配置自定义属性A的默认值,default和validator选择其中一个即可
            default: 0,
           	// validator是一个自定义的验证函数，可以对prop属性的值进行更加精确的控制和校验
            validator(value) {
            	// propD 属性约束，必须匹配下列字符串中的一个
                return ['success', 'warning', 'danger'].indexOf(value) !== -1
            }
            
            // 用type属性定义属性值的类型
            // 如果传递过来的值不符合此类型，则会在终端报错
            // type的可选值有Number、Boolean、String、Array、Object、Date、Function、Symbol(符号类型)
            // 指定可能的选项
            type: [Number,Object],
            // 定义此属性是否为必填项
            require: true,
        },
        自定义属性B:{
            /*配置选项*/
        }
    }
	// 组件的私有数据
    data() {
		return {
            // 将自定义属性A的值赋值给count
            count: this.自定义属性A
        }
    }
}
```

#### class与style绑定

在实际开发中经常会遇到动态操作元素样式的需求，因此vue允许开发者通过v-bind属性绑定指令，为元素动态绑定class属性的值和行内的style样式

```html
<!--注意：原有样式仍然存在-->
<!--数组方式绑定-->
<div class="class1" :class="[change2?'class2':'', change1?'class3':'']">
    测试
</div>

<!--对象方式绑定-->
<div class="class1" :class="classObj">
    测试
</div>

// classObj内容
data() {
	return {
		classObj: {
			// 格式: 样式名: 布尔值
			// 通过控制布尔值来控制前面的样式是否成立
			italic: false	
		}
	}
}
```

> 注意：上面的change是定义在data中的自定义数据

#### 以对象语法绑定内联的style

`:style`的对象看着像CSS，但是是一个js对象

CSS属性名可以用驼峰式，也可以用短横线式

> 短横线式一定要使用引号括起来

```
<div :style="{color: active, fontSize: 15px}">
    测试
</div>
```



#### 解决组件之间的样式冲突问题

##### 组件之间的样式冲突问题

默认情况下，写在.vue组件中的样式会全局生效，因此很容易造成多个组件之间的样式冲突问题

导致组件之间样式冲突的根本原因是：

1. 单页面应用程序中，所有组件的DOM结构都是基于唯一的index.html页面进行呈现的
2. 每个组件中的样式都会影响整个index.html页面中的DOM元素

##### 利用scoped解决样式冲突

在组件中的style标签中末尾添加scoped即可

- 用法示例

```html
<style lang="less" scoped>
    
</style>
```

- scoped的原理

scoped是利用了属性选择器解决样式冲突，在生成每一个标签的时候会自动为每一个标签配备一个自定义属性

> 如果父组件中也使用了scoped，则无法直接在父组件中修改子组件的样式，原因在于scoped会自动将style中的选择器添加自定义属性，可以使用deep解决该问题

- /deep/的使用（以修改h3标签为例）

```
<style lang="less" scoped>
    /deep/ h3 {
    	color: white
    }
</style>
```

> /deep/的原理是将属性选择器置于指定选择器之前，这样所有的子代元素都会被选中

当使用第三方组件库的时候，如果有修改组件默认样式的需求，一般需要用到/deep/

### 生命周期与数据共享

#### 组件的生命周期

##### 生命周期&生命周期函数

- 生命周期

生命周期（Life Cycle）是指一个组件从创建 -> 运行 -> 销毁的整个过程，强调的是一个时间段

- 生命周期函数

生命周期函数是由vue框架提供的内置函数，会伴随组件的生命周期自动按次序执行

> 即使不使用，所有的生命周期函数也会被依次执行

- vue2.x的生命周期图

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

##### 组件生命周期函数的分类

- 组件创建阶段

```
beforeCreate	组件被创建前(该阶段组件的props/data/methods尚未被创建，都处于不可用阶段)
created			组件被创建后(props/data/methods已被初始化，都处于可用阶段，但是组件的模板结构尚未生成(即不能操作dom)，在这个阶段可以使用ajax请求获取数据)
beforeMount		组件被渲染前(注意：这里的渲染指的是渲染到浏览器中，实际上模板已经存储在内存中了，这个方法很少用)
mounted			组件被渲染后(从此时开始才可以操作浏览器的dom结构)
```

- 组件运行阶段

```
注意：组件被更新指的是data中的数据发生了改变并将其渲染到页面中
注意，created中如果修改了data中的数据以下两个生命周期函数也会被触发
注意：组件被更新前指的是data中的数据已经被修改，但是还未被渲染到页面的阶段
beforeUpdate	组件被更新前(此时的dom结构是旧的dom结构)
updated			组件被更新后(此时的dom结构是新的dom结构)
```

- 组件销毁阶段

```
一般用if控制组件的销毁或存在，这个阶段用的较少
beforeDestroy	组件被销毁前(vue2.x)
destroyed		组件被销毁后(vue2.x)

beforeUnmount	组件被销毁前(vue3.x)
unmounted		组件被销毁后(vue3.x)
```

##### 组件主要的生命周期函数及其用途

| 生命周期函数 |           执行时机           | 所属阶段 | 执行次数  |        用途        |
| :----------: | :--------------------------: | :------: | :-------: | :----------------: |
|   created    |    组件在内存中创建完毕后    | 创建阶段 | 唯一一次  | 发ajax请求初始数据 |
|   mounted    |  组件初次在页面中渲染完毕后  | 创建阶段 | 唯一一次  |    操作DOM元素     |
|   updated    | 组件在页面中被重新渲染完毕后 | 运行阶段 | 0次或多次 |         -          |
|  unmounted   |   组件被销毁后(页面和内存)   | 销毁阶段 | 唯一一次  |         -          |

##### 使用生命周期函数

生命周期函数的使用位置与methods对象平级，直接书写生命周期函数即可

```
methods: {
	show(){
		console.log("调用了show方法");
	}
},
created()
{
	this.show();	
}
```

#### 组件之间的数据共享

##### 父组件向子组件共享数据

- 父组件向子组件共享数据使用**自定义属性**即可
  - 注意：
    1. 这个自定义属性是定义到子组件中的，在父组件中利用属性绑定的方式调用即可
    2. 尽量不要在子组件中直接修改props的数据，如果直接修改的话是将其中的变量指向一个新的对象，并不会对父组件中的值进行修改

##### 子组件向父组件共享数据

- 子组件向父组件共享数据使用**自定义事件**

- 使用方式

  - ```html
    父组件中
    <Son @numchange="getNewCount"></Son>
    // 注意：上方的numchange是自定义事件，名字也是自己起的
    export default {
    data(){
    	return {countFromSon: 0}
    },
    methods: {
    	getNewCount(val) {
    		this.countFromSon = val;
    	}
    }
    }
    
    子组件中
    export default {
    data(){
    	return {count: 0}
    },
    methods: {
    	add() {
    		this.count++;
    		// 修改数据时，通过$emit()触发自定义事件,第一个参数是自定义事件名称，第二个参数是需要传递回去的参数
    		// 注意，$emit方法调用时，父组件的自定义事件一定会被触发
    		this.$emit('numchange', this.count)
    	}
    }
    }
    ```

##### 使用v-model实现父子传值(vue3.x)

父向子传值

1. 父组件通过 `v-model:` 绑定的形式将数据传递给子组件
2. 子组件中通过props接收父组件传递过来的数据

子向父传值

1. 子组件在 `v-bind:` 指令之前添加v-model指令
2. 在子组件中声明emits自定义事件，格式为update: xxx
3. 调用

```html
在父组件中
<template>
  <h3>父组件中{{ count }}</h3>
  <!--注意：这里的指令是v-model:,number是-->
  <MyCount v-model:number="count"></MyCount>
</template>

<script>
import MyCounter from './components/test.vue'

export default {
  name: 'App',
  data() {
    return {
      count: 1
    }
  },
  components: {
    MyCount: MyCounter
  }
}
</script>

<style></style>

在子组件中
<template>
  <div>
    <p>子组件中的值是: {{ number }}</p>
    <button @click="add">+1</button>
  </div>
</template>

<script>
export default {
  name: 'MyCounter',
  props: ['number'],
  emits: ['update:number'],	// 此处记得添加，不加也可以？但是不清楚会不会有问题
  methods: {
    add() {
      this.$emit('update:number', this.number + 1)	// 此处用于指定需要改变的值及其属性
    }
  }
}
</script>
```

> 这个用法与vue2.x**并不相同**

##### 兄弟组件之间的数据共享

- 在vue2中，兄弟组件之间的数据共享方案是`EventBus`，还可以借助于第三方的包`mitt`来创建eventBus对象

- 使用方式

  - 以兄弟A向兄弟B发送数据为例

  - 该方法需要引入一个`eventBus.js`文件

  - ```js
    在兄弟组件A中
    import bus from './eventBus.js'
    
    export default {
    	data() {
    		return {
    			msg:'hello vue.js'
    		}
    	},
    	methods: {
    		sendMsg() {
    			bus.$emit('share', this.msg)
    		}
    	}
    }
    
    
    在eventBus.js中
    import Vue from 'vue'
    // 向外共享Vue的实例对象
    export default new Vue()
    
    使用mitt的方式(vue3.0)
    import mitt from 'mitt'
    // 创建eventBus的实例对象
    const bus = mitt()
    export default bus
    
    
    在兄弟组件B中
    import bus from './eventBus.js'
    
    export default {
    	data() {
    		return {
    			msgFromLeft:''
    		}
    	},
    	created() {
            bus.$on('share', val=>{
            	sthis.msgFromLeft = val
            })
    	}
    }
    ```
    

- 主要原理是利用了Vue对象的\$emit()方法和\$on方法

##### 后代关系组件之间的数据共享(vue3.x)

后代关系组件之间共享数据指的是父节点的组件向其子孙组件共享数据。此时组件之间的嵌套关系比较复杂，可以使用`provide`和`inject`实现后代关系组件之间的数据共享

- 父节点通过provide共享数据

父节点的组件可以通过provide方法对其子孙共享数据

> 注意：如果要共享响应式数据的话需要结合computed函数

```js
import {computed} from 'vue'	// 从vue中导入computed函数
export default {
	data(){
        return {
            color: 'red'	// 定义父组件要先子孙组件共享的数据
        }
    },
    provide() {	// provide 函数return的对象中包含了要向子孙组件共享的数据
        return {
            // 使用computed函数可以把要共享的数据包装为响应式数据
            color: computed(()=>{this.color})
            // 下方为不使用computed函数的示例，如果不使用该函数，则如果在父组件中修改值，子组件中的值不会被修改
            // color: this.color
        }
    }
}
```

子孙节点可以使用inject数组接收父级节点向下共享的数据

```html
<template>
	<!--响应式数据必须以.value的形式进行使用，否则无法做到响应-->
    <h>子孙组件 --- {{color.value}}</h>
</template>

<script>
export default {
	inject: ['color']
}
</script>

```

##### vuex

vuex是终极的组件之间的数据共享方案

在企业级的vue项目开发中，vuex可以让组件之间的数据共享变得高效、清晰且易于维护

#### ref引用

##### 什么是ref引用

ref能够使开发者在不依赖于JQuery的情况下获取DOM元素或组件的引用

在每一个vue的组件实例上，都包含一个、\$refs对象，里面存储着对应的DOM元素或组件的引用。默认情况下，组件的\$refs指向一个空对象

##### ref用法

可以通过在vue中的模板标签中的ref属性上添加值从而在$refs上挂载属性，从而获取dom元素

> 注意：ref属性的值不能冲突，如果冲突的话后面的dom元素会覆盖前面的dom元素
>
> $refs既可以挂载dom元素，也可以挂载组件(即挂载组件的实例，即this指针)，使用方法同dom元素

```html
<template>
    <div class="app-container">
        <!--为h1标签绑定ref属性-->
        <h1 ref="myh12">标签将被vue调用</h1>
	</div>
    <Child ref="child"></Child>
</template>

<script>
export default {
    methods: {
        showThis() {
            // 直接利用$refs中的dom元素进行修改
            this.$refs.myh12.style.color = "red";
            this.$refs.child.msg=0;// msg为子组件身上挂载的一个data属性
        }
    }
}
</script>
```

##### `this.$nextTick(callback)`方法

如果需要用ref引用一个**具有v-if属性**的标签时，需要注意的是如果标签是刚刚被创建(即v-if="true"时)，页面中是无法拿到该标签的dom元素，因为该标签此时还**未被渲染**，无法立即执行该标签的dom操作

解决方法：利用`this.$nextTick(callback)`方法

`this.nextTick(callback)`方法表示在页面重新被渲染后才调用其中的回调函数，这样能够保证其中的回调函数一定能够成功调用dom元素

> 注意：不能使用updated，因为如果v-if="false"时，无法获取dom元素，浏览器会直接报错

### 动态组件

#### 什么是动态组件

动态组件指的是动态切换组件的显示与隐藏

#### 如何实现动态组件的渲染

vue提供了一个内置的`<component>`组件，专门用来实现动态组件的渲染

> 可以将该组件认为一个占位符，所要显示的组件由is属性来决定
>
> 动态组件的实现原理是组件的销毁与创建

示例

```html
<template>
    <div>
        <h1>
       		App根组件
        </h1>
        <!--注意，component组件一定要有is属性，否则会报错-->
        <component :is="comName"></component>
    	<button @click="comName='Right'">
            点击切换为Right组件
        </button>
    </div>
</template>

<script>
import Left from '@/components/Left.vue'
import Right from '@/components/Right.vue'
export default {
    data() {
        return {
            comName:'Left'
        }
    }
    components: {
        Left,Right
    }
}    
</script>
```

#### keep-alive

由于动态组件切换实质是组件的销毁与创建，因此正常情况下原有数据无法被保存，此时可以利用`<keep-alive>`标签避免组件的销毁与创建

> keep-alive是将内部的组件进行缓存而不是销毁组件，隐藏组件时是将组件状态调整为未激活状态

- 示例

```html
<keep-alive>
    <component :is="comName"></component></keep-alive>
```

##### keep-alive对应的生命周期函数

当组件被缓存时会自动触发组件的`deactivated`生命周期函数

当组件被激活时会自动触发组件的`activated`生命周期函数

> 当组件第一次被创建的时候既会执行created，也会执行activated
>
> 但当组件被激活的时候只会触发activated，而不再触发created

> 注意：这两个生命周期函数是在子组件中进行定义，而非父组件中进行定义，即这两个生命周期函数是每个组件的可选生命周期函数

##### keep-alive的include属性和exclude属性

keep-alive默认会将标签内的所有组件均进行缓存，如果我们只需要某些组件被缓存，可以使用include属性或exclude

include属性用来指定**需要**被缓存的**组件名**，名称匹配的组件**会**被缓存，多个组件名之间使用英文逗号进行分隔

exclude属性用来指定**不需要**被缓存的**组件名**，名称匹配的组件**不会**被缓存，多个组件名之间使用英文逗号进行分隔

> include属性和exclude属性不能同时使用
>
> 注意这两个属性中的组件名是指组件中的name属性，当name属性没有被指定的时候，name属性默认为注册时的名称

### 插槽

#### 什么是插槽

插槽(slot)是vue为组件的封装者提供的能力，允许开发者在封装组件时，把不确定的、希望由用户指定的部分定义为插槽

#### 创建插槽

vue官方规定每一个slot插槽都要有一个name名称(即具名插槽)，如果省略的该名称，则其值会被设为"default"(默认插槽)

> 注意：插槽中间可以放置默认内容(官方术语为后备内容)，如果用户传入了插槽，该内容将会被覆盖

- 示例

```html
在Left组件中
<template>
	<div>
        <h3>
            Left组件
        </h3>
        <!--声明一个插槽区域-->
        <slot name="default">这是default标签的后备内容</slot>
        <slot name="another">这是another标签的后备内容</slot>
    </div>
</template>
```

#### 使用插槽

默认情况下，使用组件时，提供的内容都会被填充到名字为default的插槽中

如果需要指定需要填充到哪一个插槽中，则需要使用`v-slot`指令

##### `v-slot`指令用法

`v-slot`指令的简写形式是`#`

- 填充到默认区域写法

```html
在父组件中
<Left>
    <p>
        这是填充到default插槽的内容
    </p>
</Left>
```

- 填充到指定区域写法

> v-slot指令一定要写在template标签上，否则会报错

```html
在父组件中
<Left>
    <!--注意：以下的template标签仅起到包裹的作用，并不会被渲染为真实元素-->
    <template v-slot:another>
        <p>
            这是填充到another插槽的内容
        </p>
    </template>
    <!--以下写法也行-->
    <template #another>
        <p>
            这是填充到another插槽的内容
        </p>
    </template>
</Left>
```

#### 作用域插槽的基本用法

作用域插槽指的是在封装组件时为预留的slot提供属性的值，这种用法叫做作用域插槽

在父组件中可以使用子组件的作用插槽中的自定义属性，使用方法见下

示例

```html
在Left子组件中
<!--注意：msg中的值为固定的值，user中的值为变量，使用变量可以实现子向父传值-->
<slot name="another" msg="hello vue.js" :user="userinfo"></slot>

在父组件中
<Left>
    <!--普通写法-->
    <template #another="scope">
        <p>
            这是填充到another插槽的内容
        </p>
        <p>
        	{{scope.msg}}
        </p>
        <p>
            {{scope.user}}
        </p>
    </template>
    <!--解构赋值写法-->
    <template #another="{msg, user}">
        <p>
            这是填充到another插槽的内容
        </p>
        <p>
        	{{msg}}
        </p>
        <p>
            {{user}}
        </p>
    </template>
</Left>
```

### 自定义指令

vue除了提供常用指令外，还允许用户自定义指令

#### 自定义指令的分类

vue中的自定义指令分为

- 私有自定义指令
- 全局自定义指令

#### 私有自定义指令

在每个vue组件下，可以在directives节点下声明私有自定义指令

同时自定义属性还可以进行传值，通过binding.value获取传入的数据

如果bind和update函数中的逻辑完全相同，则对象格式的自定义指令可以简写成函数形式

> 注意：vue2与vue3自定义指令的区别如下
>
> bind -> mounted
>
> updated -> update

示例

```html
<p v-color="'red'">这是被color指令绑定的元素</p>
<p v-color='color'>这是被color指令绑定的元素</p>
<button @click="color='red'">
    改变第二个p标签的属性
</button>
</p>

<script>
export default {
data(){
	return {
		color: 'blue'
	}
},
directives:{
	// 定义名为color的自定义指令，指向一个配置对象
	color:{
        // 当指令首次被绑定到元素上时，会立即触发bind函数
		// 第一个形参el是绑定了此指令的原生DOM对象
		// 第二个形参binding包含了该指令的信息，其中的value属性是传入的值，expression属性是=号后面的表达式，此处为"'red'"，该表达式会使用js进行解析
		bind(el, binding) {	
            el.style.color =binding.value
        },
        // 在DOM更新时会触发update函数
        update(el,binding) {
            el.style.color =binding.value
        }
    }
    函数形式
    // 在bind和update时会触发相同的业务逻辑
    color(el,binding) {
            el.style.color =binding.value
        }
	}
}
</script>
```

> 注意：如果直接写v-color="red"，则表示red是一个变量，vue会自动在所有变量中查找该变量

#### 全局自定义指令

- vue2.x

全局共享的自定义指令需要在main.js中通过`Vue.directive()`进行声明

格式如下

```
// 参数1：字符串，表示全局自定义指令的名字
// 参数2：对象，用来接收指令的参数值，也可以使用对象代替
Vue.directive('color', function(el,binding){
	el.style.color = binding.value
})
```

- vue3.x

在main.js中

```
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.directive('focus', {
	// 在这里定义指令focus的规则
})
app.mount('#app')

```



## 路由

### 前端路由的概念与原理

#### 什么是路由

路由(router)就是对应关系

#### SPA与前端路由

##### SPA

SPA(单页面应用程序)指的是一个web网站只有唯一的一个HTML页面，所有组件的展示与切换都在这唯一的一个页面内完成，此时不同组件之间的切换需要通过前端路由来实现

结论：在SPA项目中，不同功能之间的切换要依赖于前端路由来完成

##### 前端路由

简单来说就是Hash地址与组件之间的对应关系

> 在前端中Hash地址可以使用锚链接来实现
>
> 例如：
>
> `<a href="#/home"></a>`
>
> 锚链接不会导致页面刷新，但是会产生浏览记录

### 前端路由的工作方式

1. 用户点击了页面上的路由链接
2. URL地址栏中的Hash值发生了变化
3. 前端路由监听到了Hash地址的变化
4. 前端路由把当前Hash地址对应的组件渲染到浏览器中

> 可以利用window对象的`onhashchange`事件监听浏览器哈希地址是否变化
>
> 实际上项目中不会自己封装路由，而是使用第三方插件进行创建

### vue-router

#### 什么是vue-router

vue-router是vue.js官方给出的路由解决方案，它只能结合vue项目进行使用，能够轻松管理SPA项目中组件的切换

> vue-router目前由3.x的版本和4.x的版本，其中
>
> vue-router 3.x只能结合vue2.x进行使用
>
> vue-router 4.x只能结合vue3.x进行使用

#### 基本使用

##### 安装和配置步骤

1. 安装vue-router包

   - ```
     npm i vue-router -S
     ```

2. 创建路由模块

   - **vue2.x**

   - 在**src**源代码目录下新建`router/index.js`路由模块，并初始化如下代码

   - ```js
     import Vue from 'vue'
     import VueRouter from 'vue-router'
     
     // 调用Vue.use()函数，把VueRouter安装为Vue的插件
     Vue.use(VueRouter)
     
     // 创建路由的实例对象
     const router = new VueRouter({
         // routes是一个数组，用于定义hash地址与组件之间的对应关系，称为路由规则
         // routes格式如下，注意#号不用写
         routes: [
             {path: '/home', component: 要展示的组件},
             {path: '/about', component: 要展示的组件}
         ]
     })
     
     // 向外共享路由的实例对象
     export default router
     ```

   - **vue3.x**

   - 在**src**源代码目录下新建`router.js`路由模块，并初始化如下代码

   - ```js
     // 按需导入方法
     import {createRouter, createWebHashHistory} from 'vue-router'
     
     // 创建路由的实例对象
     const router = createRouter({
         history: createWebHistory(),
         routes: [
             {path: '/home', component: 要展示的组件},
             {path: '/about', component: 要展示的组件}
         ]
     })
     
     // 向外共享路由的实例对象
     export default router
     ```

     

3. 导入并挂载路由模块

   - 之后在main.js中对路由进行挂载

   - vue2.x

   - ```js
     import router from '@/router/index.js' 
     
     new Vue({
     	render: h => h(App),
     	// 在Vue项目中，要想使用路由，必须把路由实例对象通过下面的方式进行挂载
     	router: router
     	或者直接简写为router也可以
     }).$mount('#app')
     ```

   - vue3.x

   - ```js
     import router from './src/router.js' 
     
     const app = createApp(App)
     app.use(router)	// 挂载路由模块
     
     app.$mount('#app')
     ```

4. 声明**路由链接**和**占位符**

   - 在需要使用路由的组件中利用如下方式使用组件

   - ```html
     <template>
     	<div>
             <!--当安装和配置了vue-router后，就可以使用router-link组件来替代a链接-->
             <!--注意：在router-link中不用写前面的#-->
             <router-link to="/home"></router-link>
             <router-link to="/about">关于</router-link>
             <!--只要在项目中安装和配置了vue-router，就可以使用router-view组件，该组件用于占位，即占位符，会自动展示hash地址对应的组件-->
             <router-view></router-view>
         </div>
     </template>
     ```

#### 常见用法

以下写法均为vue2.x版本，vue3.x版本仅需做少量修改即可

##### 路由重定向

路由重定向指的是用户在访问地址A的时候，强制用户跳转到地址C，从而展示特定的组件页面。通过路由规则的redirect属性指定一个新的路由地址，可以很方便地设置路由地重定向

```
import Vue from 'vue'
import VueRouter from 'vue-router'

// 调用Vue.use()函数，把VueRouter安装为Vue的插件
Vue.use(VueRouter)

// 创建路由的实例对象
const router = new VueRouter({
    // routes是一个数组，用于定义hash地址与组件之间的对应关系，称为路由规则
    // routes格式如下，注意#号不用写
    routes: [
        {path: '/', redirect: '/home'},
        {path: '/home', component: 要展示的组件},
        {path: '/about', component: 要展示的组件}
    ]
})

// 向外共享路由的实例对象
export default router
```

##### 路由高亮

可以通过如下两种方式将激活后的路由链接进行高亮显示

1. 使用某人的高亮class类

   - 被激活的路由连接默认会应用一个叫做`router-link-active`的类名，开发者可以使用此类名选择器为激活的路由链接设置高亮的样式

2. 自定义路由高亮的class类

   - 在创建路由的实例对象时，开发者可以基于linkActiveClass属性自定义路由链接被激活时**所应用**的类名

   - ```
     const router = createRouter({
         history: createWebHashHistory(),
         // 自定义类名
         linkActiveClass: 'router-active',
         routes: [
             {path: '/', redirect: '/home'},
             {path: '/home', component: Home},
             {path: '/about', component: About}
         ]
     })
     
     // 向外共享路由的实例对象
     export default router
     ```
     

##### 嵌套路由

通过路由实现组件的嵌套展示叫做嵌套路由

即父组件中需要使用路由链接显示子组件，子组件汇总又有子级路由链接，点击子级路由链接

嵌套路由的定义方式与普通路由类似，区别在于在router/index.js中需要用children属性声明

例如要在父组件about的子组件中使用子路由可使用如下方式定义

```
import Vue from 'vue'
import VueRouter from 'vue-router'

// 调用Vue.use()函数，把VueRouter安装为Vue的插件
Vue.use(VueRouter)

// 创建路由的实例对象
const router = new VueRouter({
    // routes格式如下，注意#号不用写
    routes: [
        {path: '/', redirect: '/home'},
        {path: '/home', component: home},
        {
        	path: '/about', 
        	component: about,
        	redirect: '/about/tab1'// 使该模块显示的时候就自动显示Tab1组件
        	// 通过children属性嵌套声明子级路由规则，注意子路由规则不能以斜线开头，即下方的tab1不能写成/tab1
        	// 默认子路由：如果children数组中，某个路由规则的字符串中为空，则这条规则叫做默认子路由，默认子路由可以代替上方的写法
        	children: [
        		{path: 'tab1', component: Tab1} // 访问/about/tab1时显示Tab1组件
        	]
        }
    ]
})

// 向外共享路由的实例对象
export default router
```

##### 动态路由

动态路由指的是把Hash地址中可变的部分定义为参数项，从而提高路由规则的复用性

在vue-router中使用英文的冒号`:`来定义路由的参数项

> 这种方式定义的参数叫做路径参数

```
{path: 'movie/: id', component: Movie}

表示将以下路由规则合并成了一个，提高了路由规则的复用性
{path: 'movie/1', component: Movie}
{path: 'movie/2', component: Movie}
{path: 'movie/3', component: Movie}
......
```

##### 动态路由获取路径参数方式

路径参数即形如`movie/: mid`的

- 方式1

在利用动态路由显示的组件中可以使用`this.$route.param` 获取路由参数

- 方式2

可以在index.js中对应的路由规则中添加`props: true`字段开启props传参

```
{path: 'movie/: mid', component: Movie,props: true}
```

之后可以在props中定义mid属性来接收动态参数

##### 动态路由获取查询参数

在利用动态路由显示的组件中可以使用`this.$route.query` 获取路由参数

> `this.$route.fullpath`获取完整路径(含查询字符串)
>
> `this.$route.path`获取路径(不含查询字符串)

##### 声明式导航

在浏览器中，点击链接实现导航的方式叫做声明式导航，例如

- 普通网页中点击`<a>`链接、vue项目中点击`<router-link>`都属于声明式导航

##### 编程式导航

在浏览器中，通过API方法实现导航的方式叫做声明式导航，例如

- 普通网页中调用location.href跳转到新页面

##### vue-router中的编程式导航API

vue-router提供了许多编程式导航的API，其中最常用的导航API分别是

1. `this.$router.push('hash地址')`
   - 跳转到指定hash地址并增加一条历史记录
2. `this.$router.replace('hash地址')`
   - 跳转到指定的hash地址并替换掉当前的历史记录
3. `this.$router.go(数值 n)`
   - 调用go方法可以在浏览器历史中前进和后退
   - 简化用法
     - `this.$router.back()`
       - 后退到上一个页面
     - `this.$router.forward()`
       - 前进到上一个页面

> `this.$router`是导航对象

##### 命名路由

通过name属性为路由规则定义名称的方式叫做命名路由

- 示例

```
{
    path: '/movie', 
    name: 'mov',
    component: Movie,
	props:true
}
```

> 注意：命名路由的name值不能重复，必须保证唯一性

- 命名路由的作用

> 命名路由主要用于取代名称较长的哈希地址，在较短的哈希地址中不需要使用命名路由

1. 命名路由可以实现声明式导航(了解即可)

   - 为`<router-link>`标签动态绑定to属性的值，并通过name属性指定要跳转到的路由规则，期间还可以用params属性指定跳转期间要携带的路由参数

   - 示例

   - ```html
     <template>
         <h3>
             MyHome组件
         </h3>
         <route-link :to="{name: 'mov',params:{id:3}}"></route-link>
     </template>
     <script>
     export default {
         name: 'Myhome',
     }
     </script>
     ```

2. 命名路由还可以实现编程式导航

   - 调用push函数期间指定一个配置对象，name是要跳转到的路由规则、params是携带的路由参数

   - 示例

   - ```html
     <template>
         <h3>
             MyHome组件
         </h3>
         <button @click="gotoMovie(3)">
             go to Movie
         </button>
     </template>
     <script>
     export default {
         name: 'Myhome',
         methods: {
             gotoMovie(id){
                 this.$router.push({name: 'mov',params:{id:3}})
             }
         }
     }
     </script>
     ```

##### 导航守卫

导航守卫可以控制路由的访问权限

##### 全局前置守卫

每次发生路由的导航跳转时都会触发全局前置守卫，因此在全局前置守卫中，程序员可以对每个路由进行访问权限的控制

使用示例

```js
在index.js中
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

// 创建路由的实例对象
const router = new VueRouter({...})	// vue2.x
// const router = createRouter({...})	// vue3.x
                              
// 调用路由示例对象的beforeEach方法即可声明全局前置守卫
// 每次发生路由导航跳转的时候，都会触发其中的回调函数
// 全局前置守卫的回调函数中接收3个形参，格式见下

router.beforeEach((to,from,next)=>{
	// to 是将要访问的路由的信息对象
	// from 是将要离开的路由的信息对象
	// next是一个函数，调用next()表示允许此次路由导航
	if(to.path === '/main')
	{
		const token = localStorage.getItem('token')
		if(token) {
			next()	// token值存在则放行 
		}else{
			next('/login')	// token值不存在则强制跳转回登录页
		}
	}else{
		next()	// 不是main页面，直接放行
	}
})
```

> 注意：
>
> 1. 在守卫方法中如果不声明next形参，则默认允许用户访问每一个路由

##### next函数的3种调用方式

当前用户拥有目标页面的访问权限，直接放行`next()`

当前用户没有目标页面的访问权限，强制其跳转到登录页面: `next('/login')`

当前用户没有目标页面的访问权限，不允许跳转到后台主页，但是不强制其调整转到登录页：next(fal)



## Vuex

### Vuex概述

#### 组件之间共享数据的方式

- 父向子传值：v-bind属性绑定

- 子向父传值：v-on事件绑定

- 兄弟组件之间共享数据：EventBus
  - $on    接收数据的那个组件
  - $emit 发送数据的那个组件

> 这三种方式只适合小范围内共享数据

#### Vuex是什么

Vuex是实现组件全局状态(数据)管理的一种机制，可以方便的实现组件之间的数据共享

#### 使用Vuex统一管理状态的好处

1. 能够在vuex中集中管理的数据，易于开发和后期维护
2. 能够高效地实现组件之间的数据共享，提高开发效率
3. 存储在vuex中的数据都是响应式的，能够实时保持数据与页面的同步

#### 什么样的数据适合存储到Vuex中

一般情况下，只有组件之间共享的数据才有必要存储到vuex中，对于组件中私有的数据，依旧存储在组件自身的data中即可

### vuex的基本使用

1. 安装vuex依赖包

   - ```
     npm install vuex --save
     ```

   - 或者直接在新建项目的时候选择vuex

2. 导入vuex包

   - ```
     import Vuex from 'vuex'
     Vue.use(Vuex)
     ```

3. 创建store对象

   - ```
     const store = new Vuex.store({
     	// state 中存放的是全局共享的数据
     	state: {count: 0}
     })
     ```

4. 将store对象挂载到vue实例中

   - ```
     new Vue({
     	el: '#app',
     	render: h => h(app),
     	router,
     	// 将创建的共享数据对象挂载到Vue实例中
     	// 所有的组件就可以直接从store中获取全局的数据了
     	store
     })
     ```

### Vuex的核心概念

#### 核心概念概述

Vuex中的主要核心概念如下

- State
- Mutation
- Action
- Getter

#### State

State提供唯一的公共数据源，所有共享的数据都要统一放到Store的State中进行存储

```
// 创建store数据源，提供唯一公共元素
const store = new Vuex.Store({
	state: {count: 0}
})
```

##### 组件中访问State中的数据的方式

- 第一种方式

```
this.$store.state.全局数据名称
```

- 第二种方式

```
// 从vuex中按需导入mapState函数
import {mapState} from 'vuex'

通过mapState函数将当前组件需要的全局数据映射为当前组件的computed计算属性：
// 将全局数据映射为当前组件的计算属性
computed: {
	...mapState(['count'])	// 三个点是展开运算法
}
```

#### Mutation

> vuex中不允许组件直接修改全局数据，即==禁止==直接写成this.$store.state.count++的形式，即使这种写法能够实现想要的效果

Mutation用于变更Store中的数据

1. vuex中只能通过mutation变更Store数据，不可以直接操作Store中的数据
2. 通过这种方式虽然操作起来稍微繁琐一些，但是可以集中监控所有数据的变化

##### 定义Mutation的方式

- 第一种方式

```
// 定义Mutation
const store = new Vuex.Store({
	state: {
		count: 0
	},
	mutations: {
		add(state,step) {	// step是一个自定义的参数
			// 变更状态
			state.count+= step
		}
	}
})

// 触发mutation
methods: {
	handle1() {
		// 触发mutations的第一种方式
		this.$store.commit('add',3)	// 3是传递给函数的参数，可以使用step进行读取
	}
}
```

- 第二种方式

```
// 从vuex中按需导入mapMutations函数
import {mapMutations} from 'vuex'

// 将指定的mutations函数映射为当前组件的methods函数
methods: {
	...mapMutations(['add'])
}
```

#### Action

Action用于处理异步任务

如果通过异步操作变更数据，必须通过Action，而不能==直接==使用Mutation的方式间接变更数据，而是应该在Action**中**通过Mutation变更数据

```
// 定义Action
const store = new Vuex.store({
	mutations: {
		// mutation中只允许写同步代码
		add(state) {
			state.count++
		}
	},
	actions: {
		// 异步代码必须写到Action中
		// context相当于new出来的Vuex.Store实例对象
		addAsync(context,step) {
			setTimeout(() +> {
				context.commit('add',step)
			},1000)
		}
	}
})

触发Action的方式
方式1，在具体的某个组件中使用如下的方式调用action
this.$store.dispatch('addAsync',5)

方式2，将指定的action函数映射为某个组件的方法
```

#### Getter

Getter用于对Store中的数据进行加工处理形成新的数据，注意Getter==不会修改==Store中的数据

1. Getter可以对Store中已有的数据加工处理之后形成新的数据，类似Vue的计算属性
2. Store中数据发生变化，Getter的数据也会跟着变化

```
// 定义Getter
const store = new Vuex.Store({
	state: {
		count: 0
	},
	getters: {
		showNum: state => {
			return '当前最新的数量是【' + state.count +'】'
		}
	}
})
```

##### 使用Getter的方式

第一种方式

```
this.$store.getters.名称
```

第二种方式

```
import {mapGetters} from 'vuex'

computed: {
	...mapGetters(['showNum'])
}
```



## ESLint

ESLint是一个可组装的js和JSX检查工具，主要用来约束代码风格

### eslintrc.js配置文件中rules节点的规则

```
 rules: {
 	// process.env.NODE_ENV表示node是打包模式还是发布模式
 	// no-console是ESLint的规则，下述指令表示当项目发布的时候会检查代码中是否有console，如有则报错
    'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
    // no-debugger是ESLint的规则，下述指令表示当项目发布的时候会检查代码中是否有debugger，如有则报错
    'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off'
  }
```

> 注意：在ESLint报错后直接至ESLint官网搜索对应的报错信息意义

### 配置VScode辅助插件

VScode中有三个插件可以辅助我们进行代码规范化

1. ESLint
2. Prettier - Code formatter
3. vetur

在VScode中安装三个插件后打开VScode的settings.json文件，在settings.json文件中添加如下信息即可完成配置

```json
  // ESLint插件的配置
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  },
  "eslint.alwaysShowStatus": true,

  // prettier配置
  "prettier.trailingComma": "none",
  "prettier.semi": false,
  "prettier.printWidth": 300, // 每行文字个数超出字限制将会被迫换行
  "prettier.singleQuote": true, // 使用单引号替换双引号
  "prettier.arrowParens": "avoid",

  // vetur配置
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  "vetur.ignoreProjectWarning": true,
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      "traillingComma": "none",
      "semi": false,
      "singleQuote": true,
      "arrowParens": "avoid",
      "printWidth": 300
    },
    "js-beautify-html": {
      "wrap_attributes": false
    }
  },
```

> 注意：
>
> 1. 可以使用右键修改格式化文档的默认配置
> 2. vue文件的默认格式化方式应该是prettier而不是vetur
> 3. 使用ESLint插件时，需要打开vue项目，而不应该打开无关方式



## axios

axios是一个专注于网络请求的库，是前端圈最火的专注于数据请求的库

中文官网：http://www.axios-js.com

英文官网：https://www.npmjs.com/package/axios

### axios的安装与基本使用

- 下载方式

```
npm i axios
```

- axios的基本语法

```javascript
axios({
	// 请求方式
    method: '请求的类型',
    // 请求的地址
    url: '请求的URL地址',
    // URL中的查询参数，可选
    params:{},
    // 请求体参数，可选
    data:{}
}).then((result)=>{
    // result中存储了请求成功后被axios封装后的结果
})

// 利用async、await修饰的写法
async ()=>{
    // 以下写法是解构了返回值并将其重命名的写法
    // 获取返回值中data对象的数据，并将其重命名为res
    const {data: res} = axios({
        // 请求方式
        method: '请求的类型',
        // 请求的地址
        url: '请求的URL地址',
        // URL中的查询参数，可选
        params:{},
        // 请求体参数，可选
        data:{}
    })
    console.log(res)
}
// 还可以直接使用axios.get("url",{params对象参数})和axios.post("url",{data对象参数})的方式发送请求
```

> 调用axios方法得到的返回值是Promise对象
>
> 如果调用某个方法的返回值是Promise实例，则前面可以添加await，但是要注意在外部函数前需要添加async声明，添加了await修饰符的Promise实例返回的不再是Promise对象，而是其中的参数，避免了使用.then的麻烦
>
> 注意，axios在请求到数据之后会在真实数据外部进行一层包装，将真实数据添加到大对象中的data对象中，然后还会在大对象中添加config对象、headers对象、request对象和status、statusText

### 在vue中使用axios

#### 方式1：在每一个需要axios的组件中使用import引入axios

缺点：太麻烦，不推荐

#### 方式2：把axios挂载到Vue原型上并配置请求路径

缺点：无法实现api接口复用

##### 具体样例

在main.js中添加如下代码挂载axios

```js
import Vue from 'vue'
import axios from 'axios'

// 全局配置axios的请求根路径
axios.defaults.baseURL = '在整个项目中要用到的请求的根路径'
// 把axios挂载到Vue原型上，供每一个.vue组件的实例使用
Vue.prototype.$http = axios

// 此后此时在每一个组件中调用axios只需使用`this.$http`即可
// 同时可以简化路径写法为
在需要使用axios的组件中

async ()=>{
    const {data: res} = this.$http({
        method: '请求的类型',
        // 如果该请求的路径是根路径的话直接写相对路径接口
        url: '/接口名称',	
        params:{},
        data:{}
    })
    console.log(res)
}
```

### 方式3：路由

