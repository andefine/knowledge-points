# 在小程序中使用sass

## 在独立的页面目录下直接使用

### 小程序项目基本结构
```
|-- andefine
    |-- app.js
    |-- app.json
    |-- app.wxss
    |-- gulpfile.js
    |-- package.json
    |-- project.config.json
    |-- components
    |   |-- miao
    |       |-- miao.js
    |       |-- miao.json
    |       |-- miao.scss
    |       |-- miao.wxml
    |       |-- miao.wxss
    |-- pages
    |   |-- index
    |   |   |-- index.js
    |   |   |-- index.json
    |   |   |-- index.scss
    |   |   |-- index.wxml
    |   |   |-- index.wxss
```

### gulpfile.js
```javascript
const gulp = require('gulp')
const sass = require('gulp-sass')
const rename = require('gulp-rename')

/**
 * 项目中使用scss。将pages下每个页面的scss文件通过gulp-sass编译成css文件，然后将后缀名改为.wxss
 */
// pages目录下scss文件的编译及rename
gulp.task('sass-pages', () => {
  return gulp.src('./pages/**/*.scss')
    .pipe(sass({outputStyle: 'expanded'}).on('error', sass.logError))
    .pipe(rename((path) => {
      path.extname = '.wxss'
    }))
    .pipe(gulp.dest('./pages'))
})

// components目录下scss文件的编译及rename
gulp.task('sass-components', () => {
  return gulp.src('./components/**/*.scss')
    .pipe(sass({outputStyle: 'expanded'}).on('error', sass.logError))
    .pipe(rename((path) => {
      path.extname = '.wxss'
    }))
    .pipe(gulp.dest('./components'))
})

// 监控pages下每个页面的scss文件的改动，并实时执行 'sass' 任务
gulp.task('sass:watch', () => {
  gulp.watch('./pages/**/*.scss', ['sass-pages']) // 监控 pages 下的scss文件变化
  gulp.watch('./components/**/*.scss', ['sass-components']) // 监控 components下的文件变化
})

// 开发环境下直接在项目目录的控制台输入 gulp 即可监控scss文件变化
gulp.task('default', ['sass:watch'], () => {
  console.log('gulp running...')
})
```

### 更进一步，生产环境下将`wxml` `wxss` `json` `js`文件进行压缩处理。思考中( • ̀ω•́ )✧ 喵~
