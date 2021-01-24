# Herokuへのデプロイ

Herokuにデプロイするためには本番環境用の設定をする必要がある。その設定をする際やデプロイをする際にいろいろつまづいたので備忘録として残しておく。

手始めに下記の記事を参考にして設定をコードを書いた。  
[DjangoアプリをHerokuにデプロイする方法 \- Qiita](https://qiita.com/frosty/items/66f5dff8fc723387108c)

**herokuでは、requirements.txtかpipfileが無いとpythonアプリだと認識してくれないらしい。**  
**gunicornというのは、pythonのwebサーバーであり、wsgiがwebアプリとwebサーバーを接続する**

[【GoogleHomeでメモ帳アプリを作る】3\. 実際にHerokuでアプリを作ってみる \- ludwig125のブログ](https://ludwig125.hatenablog.com/entry/2018/03/31/233708)


この記事では、DBの設定をいきなりpostgresqlに変更しているので、ローカルでsqLiteでアプリを作成した自分ではエラーが出てしまった。そこで本番環境用と開発用で
DBを分けた。（実際の開発でもDBは分ける気がする。）下記の記事参考にした。  
[初心者向け：HerokuにDjangoをデプロイする際のPostgresの設定 \- Qiita](https://qiita.com/norifumi92/items/4ba835234762289b24d0)

この記事通りにデプロイすると、エラー文
```
 Collecting anaconda-client==1.6.14 (from -r /tmp/build_xxx/requirements.txt (line 2))
```
みたいなエラー文がでた。これはrequire.txtファイルのpip freezeコマンドでローカルにインストールしたパッケージを全て出力するからダメらしい。 **仮想環境
を作成してその中でrequirements.txtを作成したらできた。** この時、仮想環境に必要なライブラリはインストールしておく。
参考記事  
[HerokuにPython Djangoをデプロイするときエラー「No matching distribution found for anaconda\-client==1\.6\.\.\.」 \- Qiita](https://qiita.com/mizoe@github/items/0f7898fe026fa4cefe9d)  
[Python, pip list / freezeでインストール済みパッケージ一覧を確認 \| note\.nkmk\.me](https://note.nkmk.me/python-pip-list-freeze/)

この後herokuにpushしたが又エラーが出てきた。  
エラー分切り取り
```
Error while running '$ python manage.py collectstatic --noinput'.
remote:        See traceback above for details.
remote:
remote:        You may need to update application code to resolve this error.
remote:        Or, you can disable collectstatic for this application:
remote:
remote:           $ heroku config:set DISABLE_COLLECTSTATIC=1
remote:
remote:        https://devcenter.heroku.com/articles/django-assets
remote:  !     Push rejected, failed to compile Python app.
remote:
remote:  !     Push failed
```
**$ heroku config:set DISABLE_COLLECTSTATIC=1**とあるので、環境変数にセットする。  
[HerokuにDjangoアプリをデプロイするときのエラーについての対処法 \- Qiita](https://qiita.com/Kohey222/items/82f3a76e0247b2d387db)

この辺りでherokuへのpushは成功したと思うが、画面はまだ表示されない。

ログから下記のエラー文が出てきた。
```
import django_heroku ModuleNotFoundError: No module named 'django_heroku'
```
これは**仮想環境状のパッケージとrequirements.txt上のパッケージに差異があったため**起きたエラーだと考えられる。調べたら、requirements.txtにdjango_heroku
がなかったため、記入。  
下記記事参考  
[【GoogleHomeでメモ帳アプリを作る】3\. 実際にHerokuでアプリを作ってみる \- ludwig125のブログ](https://ludwig125.hatenablog.com/entry/2018/03/31/233708)

次はこんなエラーが出た。（一部抜粋）
```
 File "<frozen importlib._bootstrap>", line 994, in _gcd_import
  File "<frozen importlib._bootstrap>", line 971, in _find_and_load
  File "<frozen importlib._bootstrap>", line 955, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 665, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/app/sample_my_app/settings.py", line 35, in <module>
    django_heroku.settings(locals())
  File "/app/.heroku/python/lib/python3.6/site-packages/django_heroku/core.py", line 99, in settings
    config['MIDDLEWARE'] = tuple(['whitenoise.middleware.WhiteNoiseMiddleware'] + list(config['MIDDLEWARE']))
KeyError: 'MIDDLEWARE'
```
調べたところ、django_herokuのREADMEに**very bottomに配置しろ**と書いてあったので、settings.pyの一番下にdjango_herokuを移動させた。

[Django \- DjangoをHerokuにデプロイした後にmanage\.py migrateで例外エラーが発生する｜teratail](https://teratail.com/questions/169838)  
[heroku/django\-heroku: A Django library for Heroku apps\.](https://github.com/heroku/django-heroku)  

この後、本番環境でmigrateして表示することができた。長かったー。

この記事でも参考にしてもデプロイできそう。  
[\[Django\] Heroku デプロイ方法 2018年版 \- Qiita](https://qiita.com/okoppe8/items/76cdb202eb15aab566d1)












