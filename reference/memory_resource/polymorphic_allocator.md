# polymorphic_allocator
* memory_resource[meta header]
* class[meta id-type]
* std::pmr[meta namespace]
* cpp17[meta cpp]

```cpp
namespace std::pmr {
  template <class Tp>
  class polymorphic_allocator;
}
```

## 概要
`polymorphic_allocator`は任意の[`memory_resource`](memory_resource.md)実装によりメモリ確保・解放戦略に関わる実際の処理を動的に切り替えることのできるアロケータである。この様な設計は一般にStrategyパターンというデザインパターンの一つとして知られている。 

このクラスと[`memory_resource`](memory_resource.md)の利用により、同じ静的型`polymorphic_allocator<Tp>`で実行時に異なるメモリの確保・解放戦略をとるアロケータの利用が可能になる。

## メンバ関数

| 名前            | 説明           | 対応バージョン |
|-----------------|----------------|----------------|
| [`(constructor)`](polymorphic_allocator/op_constructor.md) | コンストラクタ | C++17 |
| `operator=(const polymorphic_allocator& rhs) = delete;`     | コピー代入演算子（コピー禁止）     | C++17 |
| [`allocate`](polymorphic_allocator/allocate.md) | メモリを確保する | C++17 |
| [`deallocate`](polymorphic_allocator/deallocate.md) | メモリを解放する | C++17 |
| [`construct`](polymorphic_allocator/construct.md) | 指定された領域にオブジェクトを構築する | C++17 |
| [`destroy`](polymorphic_allocator/destroy.md) | 指定された領域のオブジェクトを破棄する | C++17 |
| [`select_on_container_copy_construction`](polymorphic_allocator/select_on_container_copy_construction.md) | コンテナのコピー構築時に新しい`polymorphic_allocator<Tp>`を取得する | C++17 |
| [`resource`](polymorphic_allocator/resource.md) | 使用している`memory_resource`を取得する | C++17 |

## メンバ型

| 名前            | 説明           | 対応バージョン |
|-----------------|----------------|----------------|
| `value_type` | 確保・解放を行う対象の型 | C++17 |

## 非メンバ関数

| 名前            | 説明           | 対応バージョン |
|-----------------|----------------|----------------|
| [`operator==`](polymorphic_allocator/op_equal.md) | 等値比較 | C++17 |
| [`operator!=`](polymorphic_allocator/op_not_equal.md) | 非等値比較 | C++17 |

## バージョン
### 言語
- C++17

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): 9.1
- [Visual C++](/implementation.md#visual_cpp): 2017 update 6

## 関連項目
- [`memory_resource`](memory_resource.md)


## 参照
- [C++1z 多相アロケータとメモリプール - Faith and Brave - C++で遊ぼう ](https://faithandbrave.hateblo.jp/entry/2016/08/08/170454)
- [memory_resourceについて - 本の虫](https://cpplover.blogspot.com/2015/09/memoryresource.html)
- [Polymorphic Allocator in C++17 - Qita](https://qiita.com/MitsutakaTakeda/items/48980faa9498c46b15b2)
- [P0220R1 Adopt Library Fundamentals V1 TS Components for C++17 (R1)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0220r1.html)
- [P0337r0 | Delete operator= for polymorphic_allocator](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0337r0.html)
- [Working Draft, C++ Extensions for Library Fundamentals, Version 2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.synop)