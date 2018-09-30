# 在小程序中使用sass

## 在独立的页面目录下直接使用

### 小程序基本项目结构
|-- undefined
    |-- .gitignore
    |-- app.js
    |-- app.json
    |-- app.wxss
    |-- gulpfile.js
    |-- package-lock.json
    |-- package.json
    |-- project.config.json
    |-- README.md
    |-- components
    |   |-- star-score
    |       |-- star-score.js
    |       |-- star-score.json
    |       |-- star-score.scss
    |       |-- star-score.wxml
    |       |-- star-score.wxss
    |-- img
    |   |-- icon _ sign in.png
    |   |-- icon-arrow-down.png
    |   |-- icon-collect-red.png
    |   |-- icon-collect.png
    |   |-- icon-join-the-trip.png
    |   |-- icon-serve-car.png
    |   |-- icon-serve-clock.png
    |   |-- icon-serve-hot-water.png
    |   |-- icon-serve-left-luggage.png
    |   |-- icon-serve-meal-delivery.png
    |   |-- icon-serve-restaurant.png
    |   |-- icon-serve-safe-box.png
    |   |-- icon-serve-stop.png
    |   |-- icon-serve-wife.png
    |   |-- intro-location@1x.png
    |   |-- intro-phone@1x.png
    |   |-- star-bright-half.png
    |   |-- star-bright.png
    |   |-- star-gray.png
    |   |-- marker-freehand
    |   |   |-- clock in point@2x.png
    |   |   |-- eat point@2x.png
    |   |   |-- general point@2x.png
    |   |   |-- play point@2x.png
    |   |   |-- public service point@2x.png
    |   |   |-- public service point_small@2x.png
    |   |   |-- scenic spot@2x.png
    |   |   |-- shopping point@2x.png
    |   |   |-- sleep point@2x.png
    |   |   |-- trip point@2x.png
    |   |-- marker-normal
    |       |-- icon _ eat big.png
    |       |-- icon _ hotel big.png
    |       |-- icon _ landscape big.png
    |       |-- icon _ landscape small.png
    |       |-- icon _ piay big.png
    |       |-- icon _ route big.png
    |       |-- icon _ route small.png
    |       |-- icon _ service.png
    |       |-- icon _ shopping big.png
    |       |-- icon _ sign in.png
    |-- pages
    |   |-- homestayDetail
    |   |   |-- homestayDetail.js
    |   |   |-- homestayDetail.json
    |   |   |-- homestayDetail.wxml
    |   |   |-- homestayDetail.wxss
    |   |-- index
    |   |   |-- index.js
    |   |   |-- index.json
    |   |   |-- index.scss
    |   |   |-- index.wxml
    |   |   |-- index.wxss
    |   |-- logs
    |   |   |-- logs.js
    |   |   |-- logs.json
    |   |   |-- logs.wxml
    |   |   |-- logs.wxss
    |   |-- scenic
    |   |   |-- scenic.js
    |   |   |-- scenic.json
    |   |   |-- scenic.scss
    |   |   |-- scenic.wxml
    |   |   |-- scenic.wxss
    |   |-- test
    |       |-- test.js
    |       |-- test.json
    |       |-- test.scss
    |       |-- test.wxml
    |       |-- test.wxss
    |-- utils
        |-- util.js
