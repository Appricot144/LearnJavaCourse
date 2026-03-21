# 第4章：繰り返し処理

## 4-1. for文

---

## 📖 解説

### 繰り返し処理とは？

同じ処理を何度も実行したいとき、**繰り返し処理（ループ）** を使います。  
Javaには `for`・`while`・`do-while` の3種類があります。

---

### for文の基本構造

```
for (初期化式; 条件式; 更新式) {
    // 繰り返す処理
}
```

| 部分 | タイミング | 役割 |
|---|---|---|
| 初期化式 | ループ開始前に1回だけ | カウンタ変数の初期化 |
| 条件式 | 各繰り返しの前に評価 | `false` になったらループ終了 |
| 更新式 | 各繰り返しの後に実行 | カウンタ変数の更新 |

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

**出力結果：**
```
0
1
2
3
4
```

---

### 実行フロー

```
① int i = 0  （初期化）
② i < 5 ?  → true  → 処理実行 → ③ i++
② i < 5 ?  → true  → 処理実行 → ③ i++
   ・・・
② i < 5 ?  → false → ループ終了
```

---

### カウンタの使い方バリエーション

```java
// 1〜10 までカウントアップ
for (int i = 1; i <= 10; i++) {
    System.out.print(i + " ");
}
// → 1 2 3 4 5 6 7 8 9 10

// 10〜1 までカウントダウン
for (int i = 10; i >= 1; i--) {
    System.out.print(i + " ");
}
// → 10 9 8 7 6 5 4 3 2 1

// 2ずつ増加（偶数）
for (int i = 0; i <= 10; i += 2) {
    System.out.print(i + " ");
}
// → 0 2 4 6 8 10
```

---

### 拡張for文（for-each文）

配列やコレクションの全要素を順に処理するときに使う簡潔な書き方です。

```java
int[] numbers = {10, 20, 30, 40, 50};

for (int n : numbers) {
    System.out.println(n);
}
```

**出力結果：**
```
10
20
30
40
50
```

> 💡 拡張for文はインデックスが不要なときに便利です。  
> インデックスを使う処理には通常の `for` 文を使いましょう。

---

### ネストしたfor文

`for` 文の中にさらに `for` 文を書くことができます。  
九九表のような2次元的な処理に使います。

```java
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        System.out.print(i * j + " ");
    }
    System.out.println();
}
```

**出力結果：**
```
1 2 3
2 4 6
3 6 9
```

---

### コード例：合計と平均

```java
public class ForExample {
    public static void main(String[] args) {
        int sum = 0;

        for (int i = 1; i <= 10; i++) {
            sum += i;
        }

        System.out.println("合計: " + sum);           // 合計: 55
        System.out.println("平均: " + (sum / 10.0));  // 平均: 5.5
    }
}
```

**出力結果：**
```
合計: 55
平均: 5.5
```

---

## ✏️ 問題

### 問題 4-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            System.out.println(i * i);
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
1
4
9
16
25
```

**解説：**
- `i` が 1〜5 まで1ずつ増え、それぞれの二乗（`i * i`）が出力されます。
- `1*1=1`、`2*2=4`、`3*3=9`、`4*4=16`、`5*5=25`

</details>

---

### 問題 4-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int sum = 0;

        for (int i = 1; i <= 10; i++) {
            if (i % 2 == 0) {
                sum += i;
            }
        }

        System.out.println("偶数の合計: " + sum);

        for (int i = 3; i >= 1; i--) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
偶数の合計: 30
* * *
* *
*
```

**解説：**
- 1〜10の偶数（2・4・6・8・10）の合計は `2+4+6+8+10 = 30`。
- 外側のループ `i` が 3→2→1 と減り、内側のループ `j` が `1〜i` まで `*` を出力します。

</details>

---

### 問題 4-1-3（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q3 {
    public static void main(String[] args) {
        int[] scores = {85, 62, 90, 71, 55};
        int total = 0;
        int count = 0;

        for (int score : scores) {
            total += score;
            if (score >= 60) {
                count++;
            }
        }

        System.out.println("合計: "     + total);
        System.out.println("合格人数: " + count);
        System.out.println("平均: "     + (double) total / scores.length);
    }
}
```

<details>
<summary>答えを見る</summary>

```
合計: 363
合格人数: 4
平均: 72.6
```

**解説：**
- `scores` の合計は `85+62+90+71+55 = 363`。
- 60以上は `85・62・90・71` の4つ（55は不合格）。
- 平均は `363 / 5 = 72.6`。`scores.length` は配列の要素数で `5`。

</details>

---

### 問題 4-1-4（コード記述）　⭐⭐⭐

次の条件を満たす `MultiplicationTable` クラスを作成してください。

**条件：**
- ネストした `for` 文を使って、3の段・4の段・5の段の九九を出力する
- 各段は `"3 × 1 = 3"` の形式で出力する
- 各段の後に空行を入れる

**期待される出力：**
```
3 × 1 = 3
3 × 2 = 6
3 × 3 = 9
3 × 4 = 12
3 × 5 = 15
3 × 6 = 18
3 × 7 = 21
3 × 8 = 24
3 × 9 = 27

4 × 1 = 4
4 × 2 = 8
  〜省略〜
4 × 9 = 36

5 × 1 = 5
  〜省略〜
5 × 9 = 45

```

<details>
<summary>答えを見る</summary>

```java
public class MultiplicationTable {
    public static void main(String[] args) {
        for (int i = 3; i <= 5; i++) {
            for (int j = 1; j <= 9; j++) {
                System.out.println(i + " × " + j + " = " + (i * j));
            }
            System.out.println();  // 段の後に空行
        }
    }
}
```

**解説：**
- 外側のループ `i` が 3→4→5 と1つずつ増えます（段の選択）。
- 内側のループ `j` が 1〜9 まで繰り返し、各段の掛け算を出力します。
- `System.out.println()` を内側ループの外に置くことで段ごとに空行が入ります。

</details>
