# 最近のLaravelで学んだこと

### 連番をつけるには

**foreach($items as $index => $item)** と書くことで、連番を一覧画面に表示することができる。$indexは、idを表示するわけではないので必ず0から始める
ことができる。

[Laravel Bladeテンプレートの使い方まとめ \| 実践的Web開発メソッド](https://blog.hiroyuki90.com/articles/laravel-blade/#foreach_for_while)

### 三項演算子

三項演算子は下記のように書くことができる。  
```
条件式? 式１:式２
$task = status ? 0:1;
```
条件式がtrueの場合は、式１の値が返され、falseの場合は、式２が返される。

[三項演算子 \- 条件分岐 \- PHP入門](https://www.javadrive.jp/php/if/index8.html)

### $guarded

$fillableをモデルに書くことで、フォームから受け取る値を指定することができる。逆にguardedは更新したくない値を指定することができる。（idなど）

[僕がLaravelのEloquentに$fillableでなく$guardedを指定する理由 \- Qiita](https://qiita.com/toro_ponz/items/b33c757cb7ba5bb48ed4)

### boolean型

MySQLでは、boolean型で指定した場合、trueまたはfalseの値でDBに保存することができない。DBに保存するときは、Trueなら１、Falseなら0で保存することができない。  
[LaravelでBoolean型のカラムににTrueやFalseが入らない原因と対策 \| 40代からプログラミング！](https://biz.addisteria.com/laravel_boolean_tinyint/)


### 型キャスト

```php
class User extedns Model
{
  protected $casts = [
    'is_admin' => 'boolean'
  ];
}
```
このように書くことで、0や1などをboolean型に変換することができる。この変換をしない場合は、上記でも述べたように0,1でDBに保存される。型キャストをすることで、
曖昧比較することができなくなるので、あまり使わなくてもいいかもしれない。

[Laravel Eloquent でハマらないために知っておきたいこと \- Qiita](https://qiita.com/ttaka/items/ab7cb9e4022756ad8206)  
[Eloquent：ミューテタ 5\.6 Laravel](https://readouble.com/laravel/5.6/ja/eloquent-mutators.html)  

### statusの切り替え

[MySQL \- \[laravel\]タスク別に真偽値を使用してボタン内の表示内容を変更したい｜teratail](https://teratail.com/questions/236629)
