---

layout: post
title: 'Django+Vue.js+Webpack4 Boilerplate 만들기'
categories: [dev]
tags: [web]

---

# 목적

웹 프로젝트를 시작할 때, 바로 가져다 쓸 수 있는 형태의 템플릿을 만들어 보려고 한다. Django 프로젝트를 만들고나서 프론트엔드를 React, Vue.js가 제공하는 SPA 템플릿을 같이 사용하려고할 때 port설정이라던지, production build후 serving을 어떻게 할지 등 귀찮은게 좀 있는데, 그 부분을 해결하려고 한다.
Webpack4 빌드시간이 어마어마하게 빨라졌다는데 좀 느껴보고 싶기도하고..

---

# 시작
- ## Start Django Project
  `pip install django django-webpack-loader`
  `django-admin startproject django_vue_webpack4`
  `pip freeze > requirements.txt`
       * `django-webpack-loader`는 `webpack-bundle-tracker`로 생성되는 stat 파일을 읽어 django template에서 bundle을 사용할 수 있게 해준다.

- ## Start Vue Project
  `vue cli`가 제공하는 `vue init webpack [project]`를 사용하고, webpack4로 업그레이드 하는 게 가장 빠른 길일 것 같다. 찾아보니 `vue cli`도 열심히 3.0/webpack4 버전을 준비 중 인 것 같다. 일로 한다면 지체없이 사용했겠지만, 어차피 재미로 하는 거라서 밑바닥부터 하나씩 추가하면서 webpack4향을 좀 맡아보려한다.

---

# Front-end 바퀴 다시 만들기

`yarn init` // main은 삭제, private는 true
`yarn add webpack webpack-cli --dev` // 웹팩을 로컬로 까는게 best practice라고 함
`yarn add vue`

/src/index.js
```
const Vue = require('vue').default

new Vue({
    el: '#app',
    render: function (h) {
        return h('div', 'Hello Django-Vue-Webpack4')
    }
})
```

/static/index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>DVW</title>
    </head>
    <body>
        <div id="app" />
        <script src="./bundle.js"></script>
    </body>
</html>
```

/webpack.config.js
```
const path = require('path')

module.exports = {
    mode: 'production',
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'static')
    }
}
```

/package.json
```
	...,
	"scripts": {
    	"build": "webpack --mode production"
    }
```

`yarn run build`

흠.. 2.68초.. 최초 빌드 후로는 1초대로 수렴. 느리잖아ㅠ

---
# Webpack, Babel, vue-loader설정

- ## webpack
  우선 매번 빌드하기 귀찮으니 develop 설정하고 watch 걸자
  /package.json
  ```
      ...,
      "scripts": {
          "build": "webpack --mode development"
      }
  ```

- ## babel
	- 필요한 재료들은 아래와 같다.
	- `@babel/core` // 바벨 본체
    - `@babel/preset-env` //문법 preset. 기존에는 년도별로 `babel-preset-es2015` 이런식으로 설정해줬었는데, 현재는 바벨팀에서 이걸로 쓰라고 권장한다.
    - `@babel/preset-stage-2` // class properties 쓰고싶어서 추가
    - `babel-loader@^8.0.0-beta` // webpack과의 연결고리. 현재 babel7을 쓰려면 loader beta버전을 써야한다.
    
	./.babelrc
    ```
    {
        "presets": [
            "@babel/preset-env", 
            ["@babel/preset-stage-2", { decoratorsLegacy: true }]
        ]
    }
    ```
	
    ./webpack.config.js
    ```
    ...,
    module: {
        rules: [{
            test: /\.js$/,
            exclude: '/node_modules/',
            use: {
                loader: 'babel-loader'
            }
        }]
    }
    ```
    
    ./index.js
    ```   
    ...
    class BabelTestClass {
        classVar = 0
    }
    ...
    ```
    
    `yarn run dev`
    음. 번역가가 일을 잘 하는군
    
- ## vue-loader
.vue 파일로 single file component를 import해서 쓸 수 있게 해준다. `vue-template-compiler`는 template, `css-loader`는 style tag를 담당한다.
	
`yarn add vue-template-compiler css-loader vue-loader --dev`

./webpack.config.js
    
```
const path = require('path')

const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'static')
    },
    module: {
        rules: [{
            test: /\.js$/,
            exclude: '/node_modules/',
            use: {
                loader: 'babel-loader'
            }
        }, {
            test: /\.vue$/,
            loader: 'vue-loader'
        }, {
            test: /\.css$/,
            use: [
              'vue-style-loader',
              'css-loader'
            ]
        }]
    },
    plugins: [
        new VueLoaderPlugin()
    ]
}
```

./src/index.js
```
import Vue from 'vue'

import App from './App.vue'

new Vue({
    el: '#app',
    components: { App },
    render: (h) => h(App)
})
```

./src/App.js
```
<template>
    <div class="message">{{ message }}</div>
</template>

<script>
export default {
    data: () => ({
        message: 'HELLO, LOADERS'
    })
}
</script>

<style>
.message {
    color: red;
}
</style>
```

하다보니까 `vue init`이랑 너무 똑같아져서 자괴감이 오지만 계속 해보자.

---
# Hot Module Replacement
dev server 띄우고,  HMR을 켜주기만 하면 된다.

`yarn add webpack-dev-server --dev`

./package.json
```
...,
"scripts": {
    "dev": "webpack-dev-server --mode development",
    ...
}
```

./webpack.config.js
```
...
const webpack = require('webpack')
...
	devServer: {
        contentBase: './static',
        hot: true
    },
    ...,
    plugins: [
		...,
        new webpack.NamedModulesPlugin(),
        new webpack.HotModuleReplacementPlugin()
    ]
```

`yarn run dev`

그럭저럭 끝난 것 같다. 필요할 게 생길 때 마다 추가해보자.

---
# Django와 연결

Django쪽에서는 [django-webpack-loader 세팅](https://github.com/owais/django-webpack-loader#settings)을 해주면, 특정폴더에서 webpack stat file을 읽어서 보여준다.

