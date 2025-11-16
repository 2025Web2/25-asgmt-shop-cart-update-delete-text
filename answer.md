---
sort: 4
---

# 課題の解答

以下は、課題の解答例です。
課題に合格されなかった方は、こちらのコードを参考にしてください。

**route/web.php**

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\ItemController;
use App\Http\Controllers\CartController;

Route::get('/', function () {
    return view('index');
})->name('index');

// 以前までのはコメントアウト
//Route::post('item', [ItemController::class, 'index'])->name('item.index');

Route::match(['get', 'post'], 'item/{genre?}', [ItemController::class, 'index'])->name('item.index');
Route::get('item/show/{item}', [ItemController::class, 'show'])->name('item.show');

Route::get('cart', [CartController::class, 'index'])->name('cart.index');
Route::post('cart', [CartController::class, 'store'])->name('cart.store');

// 以下を追加
Route::delete('cart/{cart}', [CartController::class, 'destroy'])->name('cart.destroy');
Route::patch('cart/{cart}',[CartController::class, 'update'])->name('cart.update');
```

**resources/views/cart/index.blade.php**

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
        <!-- 以下を追加 -->
        @if( count($carts) == 0 )
            <h3>カート内に商品はありません</h3>
            <a href="{{ route('index') }}">ジャンル選択に戻る</a>
        @else
        <!-- ここまで -->
            <h3>カート内の商品</h3>
            <table>
            <tr>
                <th>&nbsp;</th>
                <th>商品名</th>
                <th>メーカー・著者<br>アーティスト</th>
                <th>価格</th>
                <th>注文数</th>
                <th>金額</th>
                <th>削除</th>
            </tr>
            @php
                $total = 0;
            @endphp
            @foreach( $carts  as  $cart )
                <tr>
                    <td class="td_mini_img"><img class="mini_img" src="{{ asset('images/'.$cart->item->image )}}"></td>
                    <td class="td_item_name"> {{ $cart->item->name }} </td>
                    <td class="td_item_maker"> {{ $cart->item->maker }} </td>
                    <td class="td_right">&yen; {{  number_format( $cart->item->price) }} </td>
                    <!-- 既存の注文数はコメントアウト -->
                    <!-- <td class="td_right"> {{ $cart->quantity }} </td> -->
                    <!-- プルダウンと更新ボタンを追加したものに変更 -->
                    <!-- 以下を追加 -->
                    <td>
                        <form method="POST" action="{{ route('cart.update', ['cart' => $cart->ident]) }}">
                            @csrf
                            @method('PATCH')
                            <select name="quantity">
                                @for ( $i=1;  $i<=10;  $i++ )
                                    <option value="{{ $i }}"
                                    @if($i == $cart->quantity)
                                        selected
                                    @endif
                                    > {{ $i }} </option>
                                @endfor
                                &nbsp;
                                <input type="submit" value="変更">
                        </form>
                    </td>
                    <td class="td_right">&yen; {{ number_format( $cart->item->price * $cart->quantity) }}</td>
                    <td>
                        <form method="POST" action="{{ route('cart.destroy', ['cart' => $cart->ident]) }}">
                            @csrf
                            @method('DELETE')
                            <input type="submit" value="削除">
                        </form>
                    </td>
                    <!-- ここまで -->
                </tr>
                @php
                    $total += $cart->item->price * $cart->quantity;
                @endphp
            @endforeach
            <tr>
                <th colspan="5">合計金額</th><td class="td_right">&yen; {{ number_format($total) }}</td>
                <td>&nbsp;</td>
            </tr>
            </table>
            <br>
            <!-- 注文するリンクはまだ作成していないので、href属性は空にしています -->
            <a href="{{ route('index') }}">ジャンル選択に戻る</a>&nbsp;&nbsp;<a href="">注文する</a>
        @endif
    </body>
    </html>
```

**app/Http/Controllers/CartController.php**

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Cart;

class CartController extends Controller
{
    public function index()
    {
        $carts = Cart::with('item')->get();
        return view('cart.index', ['carts' => $carts]);
    }

    public function store(Request $request)
    {
        $cart = Cart::find($request->ident);
        if ($cart) {
            $new_quantity = $request->quantity + $cart->quantity;
            if ($new_quantity > 10) {
                $new_quantity = 10;
            }
            $cart->quantity = $new_quantity;
            $cart->update(['quantity' => $new_quantity]);
        } else {
            Cart::create([
                'ident' => $request->ident,
                'quantity' => $request->quantity,
            ]);
        }
        return redirect()->route('cart.index');
    }

    // --- 以下を追加 ---
    public function destroy(Cart $cart)
    {
        $cart->delete();
        return redirect()->route('cart.index');
    }

    public function update(Request $request, Cart $cart)
    {
        $cart->update(['quantity' => $request->quantity]);
        return redirect()->route('cart.index');
    }
    // --- ここまで ---
}
```




{% endraw %}



