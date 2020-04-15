# gulp构建工具

## gulp flow


定义任务 》输入文件 》处理文件 》输出


处理文件中又细分很多其他的处理方式


适合小项目，快速的形成自动化编译

首先初始化项目 **npm init -y**
**
安装gulp 插件 **cnpm i gulp -D**
**
在package.json 中 定义打包命令
![image.png](http://qiniu.simplesky12.cn/img/1585480530585-15374560-cd3a-4e9c-aa51-e5e8edb1f74a.png)


安装完成后，可以使用**npx gulp -v**查看gulp的版本号


这样我们就可以通过 **npm run build** 来执行gulp的打包命令

## gulp基本配置

### 新建gulpfile.js 文件

**gulpfile.js**


```bash
// 压缩，混淆  js
function js() {
    console.log('this is js script task');
}

// 对 scss 编译，输出 css 文件
function css() {
    console.log('this is styles script task');
}

// 监听文件的变化
function watcher() {

}

// 清除 dist 目录中的内容
function clean() {

}

exports.scripts = js;
exports.styles = css;
exports.default = function () {
    console.log('hello  gulp')
};
```

执行命令 **npx gulp --tasks**
**![image.png](https://cdn.nlark.com/yuque/0/2020/png/656731/1585482862269-534f140b-396f-4bb3-ac26-cce852c5201a.png)**
**
执行命令 **npx gulp scripts**
![image.png](http://qiniu.simplesky12.cn/img/1585482950912-a730cf47-b595-4537-adc4-cfcd37461fb6.png)




## 插件
[gulp 的 plugin 地址](https://gulpjs.com/plugins/)


### 常用插件
**gulp-uglify **js代码混淆
**gulp-rename** 重命名
**del** 清空输出目录 dist 的内容
**gulp-autoprefixer **样式的预处理, 处理浏览器的前缀问题，解决样式的兼容性
**gulp-sass**  可以使用scss文件编写样式
**gulp-loader**  使我们在引用插件时更加的方便


```javascript
const plugins = require('gulp-load-plugins')();

plugins.jshint = require('gulp-jshint');
```


### 安装插件
```bash
cnpm i gulp-sass gulp-autoprefixer gulp-load-plugins gulp-uglify del -D
```



## gulp打包配置


### js配置
在gulpfile.js 中引入插件使用


```bash
/*
* src 表示入口文件所在位置
* dest 表示输出的文件目录
* series 表示执行方式 是串联
* watch 表示开启监听
* pipe 表示下一个执行环节
* */
const {src, dest, series, watch} = require('gulp');

const plugins = require('gulp-load-plugins')();

// 压缩，混淆  js
function js() {
    src('js/*.js')
        .pipe(plugins.uglify())
        .pipe(dest('./dist/js'))
}

exports.scripts = js;
```


执行 **npx gulp scripts** 命令，来只执行 **js()**  方法
### 处理tasks not complete警告
![image.png](http://qiniu.simplesky12.cn/img/1585530512354-49fb81cb-1b37-4038-af6f-97bf3fd8116d.png)
上面的红色警告是表示我们没有结束** js()** 的gulp flow 工作流 ，需要在js方法中传入 回调函数 **cd()**，然后在 最后一个**pipe**方法后 调用 cb(), 这里 cb 是callback的简称


![image.png](http://qiniu.simplesky12.cn/img/1585530712615-92a7ed61-34af-472e-a5ef-2d50d42c3514.png)

再次执行 命令 **npx gulp scripts, **就没有刚刚的红色报错了。
![image.png](http://qiniu.simplesky12.cn/img/1585530807505-129d96a9-f697-4c03-8797-a6647ad6c417.png)


### clean配置


clean 的功能主要是每次编译前，清除dist目录


**gulpfile.js**
```javascript
const del = require('del');
// 清除 dist 目录中的内容
function clean(cb) {
    del('./dist');
    cb();
}
exports.clean = clean;
```


执行命令 **npx gulp clean**
根目录下的**dist **目录就会被删除


### CSS配置
**gulpfile.js**


```javascript
const {src, dest, series, watch} = require('gulp');
const plugins = require('gulp-load-plugins')();

function css(cb) {
    src('scss/*.scss')
        // 压缩输出的 样式文件的 代码
        .pipe(plugins.sass({outputStyle: 'compressed'}))
        .pipe(plugins.autoprefixer({
            casecade: false,
            remove: false
        }))
        .pipe(dest('./dist/css'))
    cb();
}

exports.styles = css;
```


css打包时还需要配置一下 **package.json**


```javascript
{
  "browserslist": [
    "last 2 Version",
    "> 2%"
  ]
}
```

执行命令 **npx gulp styles**
**![image.png](https://cdn.nlark.com/yuque/0/2020/png/656731/1585559075724-db0c85c0-f2b2-4611-b0a7-6ba7fd9983fc.png)**
**

### watch配置
**gulpfile.js**


```javascript
function watcher() {
    watch('js/*.js', js);
    watch('css/*.scss', css);
}

exports.default = series([
    clean,
    js,
    css,
    watcher
]);
```


这次使用命令** npm run build**  来执行 **exports.default **命令


## 热编译，浏览器自动刷新
首先安装依赖  browser-sync


```javascript
cnpm i browser-sync -D
```


在**gulpfile.js** 中引入并初始化** browser-sync**


```javascript
const browserSync = require('browser-sync').create();

// server 任务
function server(cb) {
    browserSync.init({
        server: {
            baseDir: './'  // 定义根目录位置，不需要编写 ./dist 
        }
    });
    cb();
}
```



![image.png](http://qiniu.simplesky12.cn/img/1585556733539-284e5656-72ff-4414-9282-e490f20bd44a.png)


### 热更新


```javascript
const reload = browserSync.reload;
```


![image.png](http://qiniu.simplesky12.cn/img/1585556568582-355b7290-fafa-47d6-a01f-116b00ca69a6.png)


![image.png](http://qiniu.simplesky12.cn/img/1585559148483-21b44d83-62bb-4793-bca0-18781f69939a.png)
