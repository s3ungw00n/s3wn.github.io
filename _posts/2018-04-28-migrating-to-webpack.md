---
layout: post
title: Migrating to Webpack
categories: [dev]
tags: [python]
---
<!-- $theme: gaia -->

---
# Plan

Current Project : Django + django template

To be : Django + webpack bundled frontend

---

# Pros and Cons When uses webpack

Pros
- my heart wants it (hipster)
- es6
- hot reloading


Cons
- setting time
- learning curve
- build time

---

Django
```
$python -m venv env
$source env/bin/activate
$pip install django django-webpack-loader
$django-admin startproject djangovue
$cd djangovue && ./manage.py migrate
```
---
settings.py
```
TEMPLATES = [
    ...
    'DIRS': [os.path.join(BASE_DIR, 'templates')],
    ...
]
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'assets'),
)

WEBPACK_LOADER = {
    'DEFAULT': {
        'BUNDLE_DIR_NAME': 'bundles/',
        'STATS_FILE': os.path.join(BASE_DIR, 'webpack-stats.json'),
    }
}

INSTALLED_APPS = (
 ...
 'webpack_loader',
)
```
---

Frontend
```
$npm install yarn
$yarn global add @vue/cli
$vue init webpack djangovue
$cd djangovue && yarn
$yarn add webpack-bundle-tracker
```

---
webpacks
```
 ...
 plugins: [
 	...,
        new BundleTracker({filename: './webpack-stats.json'})
 ]
```
---
django entry template ('PROJECT_ROOT/templates/index.html')
{% raw %}
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>djangovue</title>
  </head>
  <body>
    {% load render_bundle from webpack_loader %}
    <div id="app"></div>
    {% render_bundle 'app' %}
    <!-- built files will be auto injected -->
  </body>
</html>

```
{% endraw %}
---

url.py
```
...
url(r'^$', TemplateView.as_view(template_name='index.html'), name='index'),
```
---
