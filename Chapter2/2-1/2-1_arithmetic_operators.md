# 第2章：演算子

## 2-1. 算術演算子

---

## 📖 解説

### 算術演算子とは？

算術演算子とは、**数値の計算に使う演算子**です。  
Javaには以下の5種類の基本算術演算子があります。

| 演算子 | 意味 | 例 | 結果 |
|---|---|---|---|
| `+` | 加算 | `10 + 3` | `13` |
| `-` | 減算 | `10 - 3` | `7` |
| `*` | 乗算 | `10 * 3` | `30` |
| `/` | 除算 | `10 / 3` | `3` |
| `%` | 剰余（余り） | `10 % 3` | `1` |

---

### 整数の除算と小数の除算

Javaでは **整数同士の割り算は結果も整数** になります（小数点以下は切り捨て）。  
小数の結果が必要な場合は、どちらかを `double` にする必要があります。

```java
int a = 10;
int b = 3;

System.out.println(a / b);          // 3   ← 整数除算（切り捨て）
System.out.println(a % b);          // 1   ← 余り

System.out.println(10.0 / 3);       // 3.3333...  ← double で割る
System.out.println((double) a / b); // 3.3333...  ← キャストで double に変換
```

---

### 剰余演算子（%）の使いどころ

`%` は余りを求める演算子です。「偶数・奇数の判定」や「一定間隔の処理」によく使われます。

```java
System.out.println(10 % 2);  // 0  → 割り切れる（偶数）
System.out.println(11 % 2);  // 1  → 余りがある（奇数）
System.out.println(17 % 5);  // 2  → 17 ÷ 5 = 3 余り 2
```

---

### 演算子の優先順位

算術演算子には優先順位があります。数学と同じルールが適用されます。

```
高い ← * / %  >  + - → 低い
```

```java
System.out.println(2 + 3 * 4);    // 14  （3*4 が先）
System.out.println((2 + 3) * 4);  // 20  （括弧が最優先）
System.out.println(10 - 4 / 2);   // 8   （4/2 が先）
```

---

### 文字列と数値の + 演算

`+` は文字列との連結にも使われます。数値と文字列が混在する場合は注意が必要です。

```java
System.out.println("結果: " + 1 + 2);    // 結果: 12  ← 左から順に文字列連結
System.out.println("結果: " + (1 + 2));  // 結果: 3   ← 括弧で先に計算
System.out.println(1 + 2 + "が答え");    // 3が答え   ← 数値計算後に連結
```

---

### コード例

```java
public class ArithmeticExample {
    public static void main(String[] args) {
        int x = 17;
        int y = 5;

        System.out.println(x + y);           // 22
        System.out.println(x - y);           // 12
        System.out.println(x * y);           // 85
        System.out.println(x / y);           // 3  （整数除算）
        System.out.println(x % y);           // 2  （余り）
        System.out.println((double) x / y);  // 3.4（小数除算）
    }
}
```

**出力結果：**
```
22
12
85
3
2
3.4
```

---

## ✏️ 問題

### 問題 2-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int a = 20;
        int b = 6;

        System.out.println(a + b);
        System.out.println(a - b);
        System.out.println(a * b);
        System.out.println(a / b);
        System.out.println(a % b);
    }
}
```

<details>
<summary>答えを見る</summary>

```
26
14
120
3
2
```

**解説：**
- `20 / 6` は整数除算なので `3`（小数点以下切り捨て）。
- `20 % 6` は `20 ÷ 6 = 3 余り 2` なので `2`。

</details>

---

### 問題 2-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        System.out.println(2 + 3 * 4);
        System.out.println((2 + 3) * 4);
        System.out.println(10 / 3);
        System.out.println(10.0 / 3);
        System.out.println("答え: " + 1 + 2);
        System.out.println("答え: " + (1 + 2));
    }
}
```

<details>
<summary>答えを見る</summary>

```
14
20
3
3.3333333333333335
答え: 12
答え: 3
```

**解説：**
- `2 + 3 * 4` は `*` が優先なので `2 + 12 = 14`。
- `10 / 3` は整数除算で `3`。
- `10.0 / 3` は `double` 除算で `3.3333...`（末尾の `5` はdoubleの浮動小数点誤差）。
- `"答え: " + 1 + 2` は左から順に文字列連結され `"答え: 12"`。
- `"答え: " + (1 + 2)` は括弧内が先に計算され `"答え: 3"`。

</details>

---

### 問題 2-1-3（コード記述）　⭐⭐☆

次の条件を満たす `Calculator` クラスを作成してください。

**条件：**
- `int` 型の変数 `num1` に `100`、`num2` に `7` を代入する
- 以下の6項目を計算してすべて出力する

**期待される出力：**
```
足し算: 107
引き算: 93
掛け算: 700
割り算(整数): 14
余り: 2
割り算(小数): 14.285714285714286
```

> 💡 ヒント：小数の割り算は `(double) num1 / num2` のようにキャストします。

<details>
<summary>答えを見る</summary>

```java
public class Calculator {
    public static void main(String[] args) {
        int num1 = 100;
        int num2 = 7;

        System.out.println("足し算: "      + (num1 + num2));
        System.out.println("引き算: "      + (num1 - num2));
        System.out.println("掛け算: "      + (num1 * num2));
        System.out.println("割り算(整数): " + (num1 / num2));
        System.out.println("余り: "        + (num1 % num2));
        System.out.println("割り算(小数): " + (double) num1 / num2);
    }
}
```

**解説：**
- `num1 + num2` など計算式を `()` で囲むのは文字列連結との混在を防ぐためです。
- `(double) num1 / num2` で `num1` を `double` にキャストすることで小数の結果が得られます。
- `100 % 7` は `100 ÷ 7 = 14 余り 2` なので `2`。

</details>
