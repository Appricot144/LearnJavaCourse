# 第1章：変数とデータ型

## 1-2. 型変換（キャスト）

---

## 📖 解説

### 型変換とは？

異なるデータ型の値を別の型に変換することを **型変換（キャスト）** といいます。  
Javaには2種類の型変換があります。

```
【自動型変換】小さい型 → 大きい型（自動で行われる）
  byte → short → int → long → float → double

【強制型変換】大きい型 → 小さい型（明示的に指定が必要）
  double → float → long → int → short → byte
```

---

### 自動型変換（暗黙的キャスト）

データの範囲が小さい型から大きい型への変換は、Javaが**自動で**行います。  
データが失われる心配がないため、キャスト演算子は不要です。

```java
int i = 100;
long l = i;       // int → long（自動）
double d = i;     // int → double（自動）

System.out.println(l);  // 100
System.out.println(d);  // 100.0
```

---

### 強制型変換（明示的キャスト）

大きい型から小さい型への変換は、**データが失われる可能性**があるため、  
`(型名)` を使って明示的に指定する必要があります。

```java
double d = 3.99;
int i = (int) d;    // double → int（強制キャスト）

System.out.println(i);  // 3（小数点以下が切り捨てられる）
```

> ⚠️ **注意：切り捨てであって四捨五入ではない**  
> `3.99` を `int` にキャストすると `4` ではなく `3` になります。

---

### 数値と文字列の変換

#### 数値 → 文字列

```java
int n = 42;

// 方法1：String.valueOf()
String s1 = String.valueOf(n);    // "42"

// 方法2：空文字列と結合
String s2 = n + "";               // "42"

// 方法3：Integer.toString()
String s3 = Integer.toString(n);  // "42"
```

#### 文字列 → 数値

```java
String s = "123";

// String → int
int i = Integer.parseInt(s);       // 123

// String → double
double d = Double.parseDouble(s);  // 123.0
```

---

### 型変換まとめ表

| 変換の種類 | 方向 | 構文 | データ損失 |
|---|---|---|---|
| 自動型変換 | 小 → 大 | そのまま代入 | なし |
| 強制型変換 | 大 → 小 | `(型名) 値` | あり（切り捨て） |
| 数値→文字列 | — | `String.valueOf(n)` | なし |
| 文字列→数値 | — | `Integer.parseInt(s)` など | — |

---

## ✏️ 問題

### 問題 1-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int a = 50;
        double b = a;         // 自動型変換
        long c = a;           // 自動型変換

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
    }
}
```

<details>
<summary>答えを見る</summary>

```
50
50.0
50
```

**解説：**
- `int → double` の自動変換では小数点以下に `.0` が追加されます。
- `int → long` の自動変換では見た目は変わらず `50` のままです。

</details>

---

### 問題 1-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        double x = 9.99;
        int y = (int) x;

        float f = 3.14f;
        int z = (int) f;

        System.out.println(y);
        System.out.println(z);
        System.out.println((int) 100.1);
        System.out.println((int) -3.9);
    }
}
```

<details>
<summary>答えを見る</summary>

```
9
3
100
-3
```

**解説：**
- `double → int` の強制キャストでは小数点以下が**切り捨て**られます（四捨五入ではない）。
- `9.99` → `9`、`3.14` → `3`、`100.1` → `100`
- 負の数の場合も同様に切り捨てで、`-3.9` → `-3`（`-4` にはならない）となります。

</details>

---

### 問題 1-2-3（コード記述）　⭐⭐⭐

以下の条件を満たすプログラムを作成してください。

**条件：**
- `double` 型の変数 `price` に `1980.75` を代入する
- `price` を `int` 型にキャストして変数 `priceInt` に代入する
- `int` 型の変数 `count` に `3` を代入する
- `count` を `double` 型に自動変換して変数 `countDouble` に代入する
- `String` 型の変数 `label` に `"合計金額："` を代入する
- 以下の形式で出力する

**期待される出力：**
```
元の価格: 1980.75
切り捨て後: 1980
個数(double): 3.0
合計金額：5942.25
```

> 💡 ヒント：合計金額は `price * count` で計算できます。

<details>
<summary>答えを見る</summary>

```java
public class PriceCalc {
    public static void main(String[] args) {
        double price = 1980.75;
        int priceInt = (int) price;
        int count = 3;
        double countDouble = count;
        String label = "合計金額：";

        System.out.println("元の価格: " + price);
        System.out.println("切り捨て後: " + priceInt);
        System.out.println("個数(double): " + countDouble);
        System.out.println(label + (price * count));
    }
}
```

**解説：**
- `(int) price` で `1980.75` の小数点以下が切り捨てられ `1980` になります。
- `double countDouble = count;` は `int → double` の自動変換で `3.0` になります。
- `price * count` は `double × int` の演算で、結果は `double` になります（`5942.25`）。

</details>
