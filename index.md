# 【課題】カート内の商品画面の修正（削除・更新機能）

## 事前準備

1. [こちらのページ]()から、ソースコードを`C:¥sys_dev_exe`へcloneする
2. VSCode上で、`Ctrl+Shift+P`(Macの場合は`Cmd+Shift+P`)を押し、コンテナを起動する
3. VSCode上で、`Ctrl+J`(Macの場合は`Cmd+J`)を押し、ターミナルを表示する
4. `composer create-project laravel/laravel .` を実行し、Laravel環境を構築する
5. 過去に作成した以下のコードを、上記「1.」でcloneしたソースコードと同じ場所に上書きする

    ```text
    app
    ├── Http
    │   └── Controllers
    │   │   ├── CartController.php
    │   │   └── ItemController.php
    ├── Models
    │   ├── Cart.php
    │   └── Item.php
    │
    途中省略
    │
    database
    ├── migrations
    │   ├── 20XX_XX_XX_XXXXXX_create_items_table.php
    │   └── 20XX_XX_XX_XXXXXX_create_cart_table.php
    ├── seeds
    │   ├── DatabaseSeeder.php
    │   └── ItemTableSeeder.php
    │
    途中省略
    │
    public
    ├── css
    │   └── minishop.css
    ├── images
    │   └── xxx.png(15個の画像ファイル)
    │
    resources
    ├── views
    │   ├── cart
    │   │   └── index.blade.php
    │   ├── item
    │   │   ├── index.blade.php 
    │   │   └── show.blade.php    
    │   └── index.blade.php
    routes
    ├── web.php
    │
    途中省略
    │
    .env
    ```

## 本章の狙い

- [CRUD機能を作ろう！(UPDATE、DELETE編)](../shop_cart_delete_update/README.md)で学んだ知識を定着させる
- カート内の商品画面(削除・更新機能)を再構築する



## ③カートの更新機能を実装

次に、カート内の商品の注文数を変更する機能を実装します。

### ③-1 ルーティングの設定

---

`routes/web.php`に以下のルーティングを追加します。

- `PATCH`リクエストを受け付けるルーティングであること
- URLが`cart/{xxxx}`であること(※ヒント:`xxxx`にはルートモデルバインディングを利用するための文字列が入ります)
- `CartController`の`update`メソッドを呼び出すこと
- ルーティング名を`cart.update`に設定すること

### ③-2 更新ボタンの追加

`resources/views/cart/index.blade.php`を以下のように修正します。

{% raw %}
```php
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="{{ asset('css/minishop.css')}}">
<title>ショッピングサイト</title>
</head>
<body>
    
    <!-- 途中省略 -->
        @foreach( $carts  as  $cart )
            <tr>
                <td class="td_mini_img"><img class="mini_img" src="{{ asset('images/'.$cart->item->image )}}"></td>
                <td class="td_item_name"> {{ $cart->item->name }} </td>
                <td class="td_item_maker"> {{ $cart->item->maker }} </td>
                <td class="td_right">&yen; {{  number_format( $cart->item->price) }} </td>
                <!-- 既存の注文数はコメントアウト -->
                <!-- <td class="td_right"> {{ $cart->quantity }} </td> -->
                
                <!-- 以下を追加 -->
                <td>
                    <!-- action属性にルーティングを設定する(穴埋め) -->
                    <form method="POST" action="{{                                                 }}">
                        <!-- CSRF保護(穴埋め) -->
                        @
                        <!-- PATCHソッドを使うことを指定する(穴埋め) -->
                        @
                        <select name="quantity">
                            <!-- forディレクティブを使って、1から10までの選択肢を生成する(穴埋め) -->
                            @for (                      )
                                <!-- value属性にループ変数$iを設定する(穴埋め) -->
                                <option value="       "
                                <!-- ループ変数$iとカート内の商品の注文数が一致したらselectedを設定する(穴埋め) -->
                                @if(                    )
                                    selected
                                @endif
                                > {{ $i }} </option>
                            @endfor
                        </select>
                            &nbsp;
                            <input type="submit" value="変更">
                    </form>
                </td>
                <!-- ここまで、以下省略 -->
```
{% endraw %}

### ③-3 コントローラに更新機能を実装

---

`app/Http/Controllers/CartController.php`を以下のように修正します。

```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Cart;

class CartController extends Controller
{
    // 途中省略

    // --- 以下を追加 ---
    // 引数にリクストのオブジェクトとカートのオブジェクトを受け取る(穴埋め)
    public function update(                            )
    {
        // バリデーションを行う(穴埋め)
        $validated = $request->validate([
            
        ]);
        // カートの注文数を更新する(穴埋め)
        $
        // カート内の商品一覧画面にリダイレクトする(穴埋め)
        return 
    }
    // --- ここまで ---
}
```

### ③-4 動作確認(更新機能)

---

以下のようにカート内の商品画面で注文数を変更し、更新ボタンを押すと、注文数が変更されることを確認してください。

![](./images/cart_update1.png)
![](./images/cart_update2.png)
![](./images/cart_update3.png)

phpMyAdminでもデータの整合性を確認してみましょう。
![](./images/phpmyadmin_update.png)

## 課題の提出について

提出した課題はGitHub上で自動採点されます。
従来通りGitHub上にpushすれば完了で、自動採点がはじまります。

### 課題の合格基準

---

以下を合格基準とします。

1. カート内の特定の商品を削除できる
2. カート内の特定商品の注文数を変更できる
3. カート内の商品がなくなると、`カート内に商品はありません`と表示される

### 合格確認方法

---

1. 本課題の[課題ページ](https://classroom.github.com/a/RJox63YJ)に再度アクセスする
2. 画面上部にある`Actions`をクリックする<br>
![](./images/acions.png)
1. **一番上**の行に、緑色のチェックが入っていればOK<br>
![](./images/pass.png)
