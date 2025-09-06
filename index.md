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

- [CRUD機能を作ろう！(UPDATE、DELETE編)](https://2025web2.github.io/05-crud-update-delete/)で学んだ知識を定着させる
- カート内の商品画面(削除・更新機能)を再構築する
