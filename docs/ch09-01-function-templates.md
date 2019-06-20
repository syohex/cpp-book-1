# 関数テンプレート

型やコンパイル時に定まる値をパラメータ化する機能をテンプレートといいます。
型に依存せず処理を共通化するために使用されます。

関数に対するテンプレートを関数テンプレートといいます。
関数テンプレートを定義するには次のようにします。

```cpp
template <typename T>
T Sum(T a, T b) {
    return a + b;
}
```

型をパラメータ化するには `typename` を使用します。
この例では `T` という型で2つの引数と戻り値をパラメータ化しています。
この関数テンプレートの関数を呼び出すには次のようにします。

```cpp
Sum<int>(1, 2);         // 3
Sum<double>(1.2, 3.4);  // 4.6
```

関数呼び出し時に `< ... >` で `T` の型を指定します。

## テンプレート実引数の省略

実引数から型パラメータ `T` を推論できる場合には
呼び出し時の `< ... >` を省略することができます。

```cpp
Sum(1, 2);      // 3
Sum(1.2, 3.4);  // 4.6
```

第1引数を `int` 、第2引数を `double` にすると `T` の型を推論できず、
呼び出し時の `< ... >` を省略した場合コンパイルエラーになります。

```cpp
Sum<double>(1, 2.3);  // 3.3
// Sum(1, 2.3);       // コンパイルエラー
```

## 複数のテンプレート引数

パラメータ化する型やコンパイル時に定まる値は
1つだけではなく複数にすることができます。

戻り値を `double` に固定して第1引数と第2引数を
パラメータ化するには次のようにします。

```cpp
template <typename T, typename U>
double Sum(T a, U b) {
    return a + b;
}
```

この場合には次の関数呼び出しもコンパイルエラーにはなりません。

```cpp
Sum(1, 2.3);  // 3.3
```

## 戻り値の型推論

パラメータ化した引数の型から戻り値の型を推論するには `decltype` を使用します。

```cpp
template <typename T, typename U>
auto Sum(T a, U b) -> decltype(a + b) {
    return a + b;
}
```

## コンパイル時に定まる値のパラメータ化

テンプレートでは型だけではなく
コンパイル時に定まる値をパラメータ化することができます。
コンパイル時に定まる値として配列のサイズなどを指定することができます。

```cpp hl_lines="1 3"
template <int N>
int Fibonacchi() {
    int a[N + 1];
    a[0] = 0;
    a[1] = 1;
    for (auto i = 2; i <= N; ++i) {
        a[i] = a[i - 1] + a[i - 2];
    }
    return a[N];
}
```

この例は [フィボナッチ数] を計算する処理です。
次のように呼び出すことで値を計算することができます。

```cpp
Fibonacchi<10>();  // 55
```

[フィボナッチ数]: https://ja.wikipedia.org/wiki/フィボナッチ数