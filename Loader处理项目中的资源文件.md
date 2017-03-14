<h1>Loader处理项目中的资源文件</h1>

<h2>1.新建相关文件夹</h2>
相关文件目录如下：
* Webpack
  * src
    * assets
      * aolei.png
    * components
      * layer
        * layer.js
        * layer.less
        * layer.tpl
        * modal.less
    * css
      * common.css
      * flex.css
    * app.js
  * index.html
  * webpack.config.js
  * postcss.config.js
  
<h2>2.相关loader</h2>
* babel-loader(处理ES6代码)
  * 安装：先在命令行中执行`npm install --save-dev babel-loader babel-core`，再执行`npm install --save-dev babel-preset-latest`
* postcss-loader(处理css代码)
  * 安装：在命令行中执行`npm install postcss-loader --save-dev`
  * 例：加入浏览器前缀:先安装autoprefixer，在命令行中执行`npm install autoprefixer --save-dev`
* less-loader(处理less代码)
  * 安装：在命令行中执行`npm install less-loader --save-dev`，若提示缺少less，执行`npm install less --save-dev`来安装less
* sass-loader(处理scss文件)
  * 安装：在命令行中执行`npm install sass-loader --save-dev`
* html-loader(处理html文件)
  * 安装：在命令行中执行`npm install html-loader --save-dev`
* ejs-loader(处理模块文件)
  * 安装：在命令行中执行`npm install ejs-loader --save-dev`
* file-loader(处理图片文件)
  * 安装：在命令中执行`npm install file-loader --save-dev`
* url-loader(处理图片文件,通过设置图片大小上限来处理图片)
  * 安装：在命令行中执行`npm install url-loader --save-dev`
* image-webpack-loader(压缩图片文件)
  * 安装：在命令行中执行`npm install image-webpack-loader --save-dev`
  
<h2>3.相关源码及解释</h2>
* webpack.config.js
```
var htmlWebpackPlugin = require('html-webpack-plugin');  //引入html-webpack-plugin插件
var path = require('path');  //引入path方法

module.exports = {
	entry:'./src/app.js',
	output:{
		path:'./dist',
		filename:'js/[name].bundle.js'
	},
	module:{
		loaders:[          //loader数组，其中的每一项都是一个规则
		    {
		    	test:/\.js$/,                //正则检测以.js结尾的文件
		    	loader:'babel-loader',       //给符合检测的文件匹配babel-loader
		    	exclude:path.resolve(__dirname,'node_modules'),    //选择不用打包的文件
		    	include:path.resolve(__dirname,'src'),             //选择需要打包的文件
		       query:{
		    		presets:['latest']   //指定baber-loader插件		
    	            }
		    },
		    {
		    	test:/\.html$/,
		    	loader:'html-loader'
		    },
		    {
		    	test:/\.tpl$/,
		    	loader:'ejs-loader'
		    },
		    {
		    	test:/\.css$/,
		    	 loader:'style-loader!css-loader?importLoaders=1!postcss-loader'    //`css-loader?importLoaders=1`用来处理某个css文件`@import`其他css文件的问题
		    },
		    {
		    	test:/\.less$/,
		    	 loader:'style-loader!css-loader!postcss-loader!less-loader'   //当某个less文件`@import`其他less文件时，不必像上述处理css文件一样添加`css-loader?importLoaders=1`
		    },
		    {
		    	test:/\.scss/,
		    	 loader:'style-loader!css-loader!postcss-loader!sass-loader'  //处理方式同less
		    },
		    /*{
		        test:/\.(png|jpg|gif|svg)$/i,
			    loader:file-loader     //处理图片的相对路径
		    },*/
		    {
		    	test:/\.(png|jpg|gif|svg)$/i,      //正则检测以.png或.jpg或.gif或.svg结尾的文件，不区分大小写
		    	loaders:[    //当需要使用多个loader进行处理时，可考虑使用loaders数组，处理时将从后向前处理
                    'url-loader?limit=10000&name=assets/[name]-[hash:5].[ext]',   //将图片大小限制为10K并将并将图片输出至assets文件夹，同时命名为“（图片名+hash值的5倍）.原来图片的后缀”，当图片大小在10K以下时，图片将不会打包，而是转化为编码
                    'image-webpack-loader'  //配合url-loader中的limit使用，将图片大小进行压缩
		    	]
		    }

		]
	},
	plugins:[
	    new htmlWebpackPlugin({
		   filename:'index.html',
		   template:'index.html',
		   inject:'body'
	    })
	]
}
  ```
  
* postcss.config.js
```
//用来配置postcss-loader
module.exports = {

plugins: [

       require('autoprefixer')({ browsers: ["last 5 versions"] })  //使用postcss-loader的autoprefixer插件

  ]

}
```

* app.js
```
import './css/common.css';  //引入css文件
import Layer from './components/layer/layer.js';  //从js中载入Layer


const App = function(){
	var dom = document.getElementById("app");   //获取id为'app'的节点
	var layer = new Layer();

	dom.innerHTML=layer.tpl({    //改变节点的内容
		name:'john',
		arr:['apple','xiaomi','oppo']
	});

}

new App();
```

* common.css
```
@import './flex.css';  //某个css文件中引入其他css文件，在使用postcss-loader进行处理时，需要采用`style-loader!css-loader?importLoaders=1!postcss-loader`的形式

html,body{
	padding:0;
	margin:0;
	background-color: red;
}

ul,li{
	padding:0;
	margin:0;
	list-style:none;
}
```

* layer.js
```
import tpl from './layer.tpl';  //引入模板
import './layer.less';   //引入less文件

function layer(){
	return {     //函数返回一个对象
		name:'layer',
		tpl:tpl
	};
}

export default layer;  //输出layer
```

* layer.less
```
@import './modal.less';  //某个less文件中引入其他less文件，在使用postcss-loader进行处理时，直接采用`style-loader!css-loader!postcss-loader`即可(注意同css的区别)

.layer{
	width:600px;
	height:200px;
	background-color:green;


  > div{
	  width:400px;
	  height:100px;
	  background:url('../../assets/aolei.png');  //在less或css文件中引用图片时，可直接使用相对路径，(注意同在组件中引用图片的区别)
  }
}
```

* layer.tpl
```
<div class="layer">
    <img src="${require('../../assets/aolei.png')}"/>   //在某个组件中引用图片时，如果使用相对路径，需要使用${require(相对路径)}进行处理
	<div>this is <%= name %> layer</div>
	<% for(var i=0;i<arr.length;i++) { %>
	    <%= arr[i] %>   //输出数组的值
	<% } %>
</div>
```
