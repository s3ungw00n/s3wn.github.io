Rapid Web Prototyping
---
Prerequisite

Vue + Django + Webpack + Docker

`yarn global add @vue/cli`

---
Start

`vue create awesomeproject`
`cd awesomeproject`
`yarn add webpack-bundle-tracker`
`(venv)pip install django django-webpack-loader`
`(venv)django-admin startproject awesomeproject .`

---
vue.config.js

```
const BundleTracker = require('webpack-bundle-tracker')

module.exports = {
    devServer: {
        headers: {
          'Access-Control-Allow-Origin': '*'
        }
    },
    configureWebpack: {
        output: {
            publicPath: process.env.NODE_ENV === 'development' ? 'http://localhost:8080/' : '/static/'
        },
        plugins: [
            new BundleTracker({ filename: './webpack-stats.json' })
        ]
    }
}
```

---
settings.py

```
ALLOWED_HOSTS = ['web']

INSTALLED_APPS = [
    ...
    'webpack_loader',
]


```

