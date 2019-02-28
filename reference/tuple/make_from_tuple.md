# make_from_tuple
* tuple[meta header]
* function template[meta id-type]
* std[meta namespace]
* cpp17[meta cpp]

```cpp
namespace std {
  template <class T, class Tuple>
  constexpr T make_from_tuple(Tuple&& t);
}
```

## 概要
tuple-likeな型`Tuple`のオブジェクトに含まれる値から型`T`のオブジェクトを構築する。

## 要件
型`T`のコンストラクタの内のいずれか一つが、型`Tuple`に含まれる全ての値をその順番通りに受け入れ可能であること。それができない場合はコンパイルエラーとなる。  
また、型`T`の初期化はそのコンストラクタで行われ集成体初期化は考慮されない（これは、型`T`は集成体（aggregate）であってはならない、という事と等値である）。

## 引数
- `t` -- tuple-likeな型`Tuple`のオブジェクト

tuple-likeな型とは主に[`std::tuple`](../tuple.md)の事であるが、[`std::pair`](/reference/utility/pair.md)や[`std::array`](/reference/array/array.md)のように[`std::tuple`](../tuple.md)と同じような扱いができる型のことである。  
より詳細には、[`std::get`](/reference/array/array/get.md)（インデックス指定）と[`std::tuple_size`](/reference/array/array/tuple_size.md)が適用可能である型である。

## 戻り値
`Tuple`に含まれる型の値をその順番通りに型`T`のコンストラクタに[`std::forward`](`/reference/utility/forward.md`)して構築された`T`のオブジェクト。

## 例外
`Tuple`に含まれる型の値を受け取る`T`のコンストラクタが例外を送出する可能性がある場合は、この関数も例外を送出しうる。

## 備考
構築したい型`T`は引数からの推論ができないので、明示的に指定する必要がある。

## 実装例
```cpp
template<class T, class Tuple, std::size_t... Index>
constexpr T make_from_tuple_impl(Tuple&& t, std::index_sequence<Index...>){
  return T(std::get<Index>(std::forward<Tuple>(t))...);
}

template <class T, class Tuple>
constexpr T make_from_tuple(Tuple&& t) {
  return make_from_tuple_impl(std::forward<Tuple>(t), std::make_index_sequence<std::tuple_size_v<std::decay_t<Tuple>>>{});
}
```
* std::forward[link /reference/utility/forward.md]
* std::make_index_sequence[link /reference/utility/make_index_sequence.md]
* std::index_sequence[link /reference/utility/index_sequence.md]
* std::decay_t[link /reference/type_traits/decay.md]

このような実装の場合、[値のコピー省略保証](/lang/cpp17/guaranteed_copy_elision.md)に対応するコンパイラであれば型`T`はムーブ可能である必要がない（戻り値の`T`のオブジェクトは呼び出し元で構築される）。

## 例

```cpp example
#include <tuple>
#include <array>
#include <iostream>

struct sample {
  sample(int a, int b, double c, const char* d) {
    std::cout << a << ", " << b << ", " << c << ", " << d << std::endl;
  }

  sample(int a, double b) {
    std::cout << a << ", " << b << std::endl;
  }

  sample(int a1, int a2, int a3, int a4) {
    std::cout << a1 << ", " << a2 << ", " << a3 << ", " << a4 << std::endl;
  }

  //あらゆるムーブ・コピー操作をdelete
  sample(sample&&) = delete;
  sample(const sample&) = delete;
  sample& operator=(sample&&) = delete;
  sample& operator=(const sample&) = delete;
};

int main()
{
  {
    auto t = std::make_tuple(0, 10, 20.0, "Hello world.");

    //std::tuple<int, int, double, const char*>からの構築
    auto s = std::make_from_tuple<sample>(std::move(t));
  }

  {
    auto p = std::make_pair(30, 40.0);

    //std::pari<int, double>からの構築
    auto s = std::make_from_tuple<sample>(std::move(p));
  }

  {
    std::array<int, 4> a = {1, 2, 3, 4};

    //std::arrayからの構築
    auto s = std::make_from_tuple<sample>(std::move(a));
  }
}
```
* std::make_from_tuple[color ff0000]

### 出力
```
0, 10, 20, Hello world.
30, 40
1, 2, 3, 4
```

## バージョン
### 言語
- C++17

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [Visual C++](/implementation.md#visual_cpp): 2017


## 関連項目
- [apply](../apply.md)

## 参照
- [C++1z タプルを任意の型のオブジェクトに変換するmake_from_tuple関数 - Faith and Brave - C++で遊ぼう](https://faithandbrave.hateblo.jp/entry/2016/08/19/173946)
- [P0209R2 make_from_tuple: apply for construction](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0209r2.pdf)