# Django Vue Template

![Vue Logo](/src/assets/logo-vue.png "Vue Logo")
![Django Logo](/src/assets/logo-django.png "Django Logo")

VueおよびDjangoを使用する為の最小限の例

このプロジェクトでは、VueとDjangoが明確に分離している。

* Vue & Webpack
  - すべてのフロントエンドロジックとバンドル評価を処理

* Django & Django REST framework
  - データモデル、Web APIを管理し、静的ファイルを提供する

エンドポイントを追加して`Django`でレンダリングされたhtml応答を提供することも可能だが、
このプロジェクトでは主にバックエンドに`Django`を使用し、ビューのレンダリングとルーティングを行い
`Vue` + `Vue Router`が単一ページアプリケーション（SPA）として処理している。

Out of the box, Django will serve the application entry point (`index.html` + bundled assets) at `/` ,
data at `/api/`, and static files at `/static/`. Django admin panel is also available at `/admin/` and can be extended as needed.

The application templates from Vue CLI `create` and Django `createproject` are kept as close as possible to their
original state, except where a different configuration is needed for better integration of the two frameworks.

* [元記事](https://github.com/gtalarico/django-vue-template)

### Demo

[Live Demo](https://django-vue-template-demo.herokuapp.com/)

### Includes

* Django
* Django REST framework
* Django Whitenoise, CDN Ready
* Vue CLI 3
* Vue Router
* Vuex
* Gunicorn
* Configuration for Heroku Deployment


### プロジェクト構成


| Location             |  Content                                   |
|----------------------|--------------------------------------------|
| `/backend`           | Django Project & Backend Config            |
| `/backend/api`       | Django App (`/api`)                        |
| `/src`               | Vue App .                                  |
| `/src/main.js`       | JS Application Entry Point                 |
| `/public/index.html` | Html Application Entry Point (`/`)         |
| `/public/static`     | Static Assets                              |
| `/dist/`             | Bundled Assets Output (generated at `npm run build`) |

## 構築に必要なもの

参考記事一覧:

- [X] Node.js / npm - [Node.js / npm をインストール (Mac環境)](https://qiita.com/PolarBear/items/62c0416492810b7ecf7c)
- [X] Vue CLI 3 - [Vue CLI 3 インストール](https://qiita.com/kamitomo/items/34451b11caaf51bd2498)
- [X] Python 3 - [Python3のインストール](https://www.python.jp/install/windows/install_py3.html)
- [X] Pipenv - [Pipenvを使ったPython開発](https://qiita.com/y-tsutsu/items/54c10e0b2c6b565c887a)

## 構築手順

Clone
```
$ git clone https://github.com/gtalarico/django-vue-template
$ cd django-vue-template
```

Setup
```
$ npm ci
$ pipenv install --python 3.6 && pipenv shell
$ python manage.py migrate
```

## 開発サーバーの立ち上げ

```
$ python manage.py runserver
```

manage.pyとpackage.jsonが同じ階層にあるので
もう一つタブを開き、同じディレクトリで:

```
$ npm run serve
```

Vueアプリケーションは[`localhost:8080`](http://localhost:8080/)から提供され、
Django APIおよび静的ファイルは[`localhost:8000`](http://localhost:8000/)から提供されている。

デュアル開発サーバーのセットアップにより、webpackの開発サーバーをホットモジュール交換で活用

プロキシ設定は[`vue.config.js`](/vue.config.js)に記述
ポート8000​​でdjangoのAPIにリクエストをルーティングするために使用

単一の開発サーバーを実行する場合は、Djangoの開発サーバーでのみ実行できるが、
最初にVueアプリをビルドする必要があり、変更時にページがリロードされない。

```
$ npm run build
$ python manage.py runserver
```

## 環境変数

```
PYTHONUNBUFFERED=1;
DJANGO_SETTINGS_MODULE=backend.settings.dev
```

## 静的ファイル

このプロジェクトでは、Whitenoise Djangoで静的ファイルを配信している
なので個別の静的サーバーを必要としない(Amazon S3, nginxなど)

詳細 => [WhiteNoise Documentation](http://whitenoise.evans.io/en/stable/django.html)
