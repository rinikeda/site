# remove_copy
* algorithm[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  // (1)
  template<input_iterator I, sentinel_for<I> S, weakly_incrementable O, class T, class Proj = identity>
    requires indirectly_copyable<I, O> && indirect_binary_predicate<ranges::equal_to, projected<I, Proj>, const T*>
  constexpr remove_copy_result<I, O>
    remove_copy(I first, S last, O result, const T& value, Proj proj = {});

  // (2)
  template<input_range R, weakly_incrementable O, class T, class Proj = identity>
    requires indirectly_copyable<iterator_t<R>, O> && indirect_binary_predicate<ranges::equal_to, projected<iterator_t<R>, Proj>, const T*>
  constexpr remove_copy_result<borrowed_iterator_t<R>, O>
    remove_copy(R&& r, O result, const T& value, Proj proj = {});
}
```
* input_iterator[link /reference/iterator/input_iterator.md]
* sentinel_for[link /reference/iterator/sentinel_for.md]
* weakly_incrementable[link /reference/iterator/weakly_incrementable.md]
* indirectly_copyable[link /reference/iterator/indirectly_copyable.md]
* ranges::equal_to[link /reference/functional/ranges_equal_to.md]
* identity[link /reference/functional/identity.md]
* projected[link /reference/iterator/projected.md]
* indirect_binary_predicate[link /reference/iterator/indirect_binary_predicate.md]
* remove_copy_result[link ranges_in_out_result.md]
* input_range[link /reference/ranges/input_range.md]
* iterator_t[link /reference/ranges/iterator_t.md]
* borrowed_iterator_t[link /reference/ranges/borrowed_iterator_t.md]

## 概要
指定された要素を除け、その結果を出力の範囲へコピーする。

* (1): イテレータペアで範囲を指定する
* (2): 範囲を直接指定する

## 事前条件
- `[first,last)` と `[result,result + (last - first)` は重なってはならない。

## 効果
`[first,last)` 内にあるイテレータ `i` について、[`invoke`](/reference/functional/invoke.md)`(proj, *i) == value` でない要素を `result` へコピーする


## 戻り値
`{ .in = last, .out = result + (last - first) }`


## 計算量
正確に `last - first` 回の比較を行う


## 備考
安定。


## 例
```cpp example
#include <algorithm>
#include <iostream>
#include <vector>
#include <iterator>

int main() {
  std::vector<int> v = { 2,3,1,2,1 };

  // 1 を除去した結果を出力する
  std::ranges::remove_copy(v, std::ostream_iterator<int>(std::cout, ","), 1);
}
```
* std::ranges::remove_copy[color ff0000]

### 出力
```
2,3,2,
```


## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): 10.1.0
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2019 Update 10

## 参照
- [N4861 25 Algorithms library](https://timsong-cpp.github.io/cppwp/n4861/algorithms)
