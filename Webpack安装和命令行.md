<h2>Webpack安装</h2>
在命令行中进入指定文件夹，执行`npm install webpack --save-dev`进行安装

<h2>文件打包</h2>
* 在命令行中执行`webpack hello.js hello.bundle.js`
* 注：非全局安装时，Windows系统下会提示“webpack”不是内部命令，此时应执行`.\node_modules\.bin\webpack hello.js hello.bundle.js`

<h2>安装loader</h2>
.css文件不能直接打包，直接打包会报错，需要先安装style-loader以及css-loader，在命令行中执行`npm install css-loader style-loader --save-dev`

<h2>css文件打包</h2>
* 在需要进行引用的文件顶部输入`require('style-loader!css-loader!./style.css');`
* 或者直接在命令中执行`webpack hello.js hello.bundle.js --module-bind "css=style-loader!css-loader"`;同样的，非全局安装时，执行`.\node_modules\.bin\webpack hello.js hello.bundle.js --module-bind "css=style-loader!css-loader"`

<h2>自动打包</h2>
在命令行中执行`webpack hello.js hello.bundle.js --module-bind "css=style-loader!css-loader" --watch`;同样的，非全局安装时，执行`.\node_modules\.bin\webpack hello.js hello.bundle.js --module-bind "css=style-loader!css-loader" --watch`
