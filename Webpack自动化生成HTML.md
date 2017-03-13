<h1>Webpack自动化生成HTML(以windows为例)</h1>
<h2>1.安装webpack</h2>
在命令行中执行`npm install webpack --save-dev`进行安装

<h2>2.新建相关文件夹</h2>
* 在命令行中依次执行`mkdir src`和`mkdir dist`
* 打开项目，在src文件下新建script和style文件夹

<h2>3.新建相关文件</h2>
* 在项目根目录下新建webpack.config.js
* 在项目根目录下新建index.html
* 在script文件夹下新建main.js,a.js,b.js,c.js等四个JS文件

<h2>4.安装html-webpack-plugin插件</h2>
在命令行中执行`npm install html-webpack-plugin --save-dev`进行安装

<h2>5.各文件源码及解释</h2>
* webpack.config.js

```
var htmlWebpackPlugin = require('html-webpack-plugin');  //引入html-webpack-plugin插件
module.exports = {
        
	//资源输入	
        entry:{
            main:'./src/script/main.js',
	    a:'./src/script/a.js',
	    b:'./src/script/b.js',
	    c:'./src/script/c.js'
	},
	
	//资源输出	
        output:{
		path:'./dist',  //输出路径		
                filename:'js/[name]-[chunkhash].js',  //输出文件名
		publicPath:'http://cdn.com/'  //发布之后的引用路径
		},
		
	//插件设置	
	plugins:[
	    /*new htmlWebpackPlugin({
		   filename:'index.html',          //输出文件名
		   template:'index.html',          //模板文件名		
		   inject:false,                   //代码注入位置		
		   title:'webpack is good',        //输出网页名称		
		   date:new Date(),                //输出当前时间
		   minify:{                        //压缩文件大小			
		          removeComments:true,     //删除注释
			  collapseWhitespace:true  //删除空格
			  }
	    }),*/
	    new htmlWebpackPlugin({
		    filename:'a.html',
		    template:'index.html',
		    inject:false,
		    title:'this is a.html',
		    excludeChunks:['b','c']    //除了设置的chunk之外,其余的chunk都会加载	
            }),
	    new htmlWebpackPlugin({
		    filename:'b.html',
		    template:'index.html',
		    inject:false,
		    title:'this is b.html',
		    excludeChunks:['a','c']
	    }),
	    new htmlWebpackPlugin({
		    filename:'c.html',
		    template:'index.html',
		    inject:false,
		    title:'this is c.html',
		    excludeChunks:['a','b']
	    }),
	]
}
```
