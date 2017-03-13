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
    *app.js
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
