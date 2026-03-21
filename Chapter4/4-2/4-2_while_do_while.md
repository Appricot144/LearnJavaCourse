# 第4章：繰り返し処理

## 4-2. while / do-while文

---

## 📖 解説

### while文とは？

`while` 文は **条件が `true` の間、処理を繰り返す**構文です。  
繰り返し回数が事前に決まっていないときに適しています。

```
while (条件式) {
    // 条件が true の間繰り返す処理
}
```

```java
int i = 0;

while (i < 5) {
    System.out.println(i);
    i++;
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

### for文 vs while文

同じ処理でも `for` と `while` の両方で書けます。  
使い分けの目安は「**繰り返し回数が決まっているか**」です。

```java
// for文（回数が決まっているとき）
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// while文（同じ処理を while で書いた場合）
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

| 使い分け | 推奨 |
|---|---|
| 繰り返し回数が決まっている | `for` 文 |
| 条件が満たされるまで繰り返す | `while` 文 |
| 最低1回は必ず実行したい | `do-while` 文 |

---

### while文の注意点：無限ループ

条件式が常に `true` になると、ループが終わらない **無限ループ** になります。

```java
int i = 0;

while (i < 5) {
    System.out.println(i);
    // i++ を忘れると無限ループ！
}
```

> ⚠️ ループ変数の更新を忘れずに記述しましょう。

---

### do-while文とは？

`do-while` 文は **処理を先に実行してから条件を評価**します。  
そのため **必ず1回は処理が実行される**のが特徴です。

```
do {
    // 最低1回は必ず実行される処理
} while (条件式);
```

```java
int i = 0;

do {
    System.out.println(i);
    i++;
} while (i < 5);
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

### while と do-while の違い

最初から条件が `false` の場合の動作が異なります。

```java
int x = 10;

// while：最初から条件が false → 1回も実行されない
while (x < 5) {
    System.out.println("while: " + x);  // 実行されない
}

// do-while：最初から条件が false → それでも1回は実行される
do {
    System.out.println("do-while: " + x);  // "do-while: 10" が出力される
} while (x < 5);
```

**出力結果：**
```
do-while: 10
```

---

### while文の典型的な使用例

```java
// 入力値の検証（正の整数が入力されるまで繰り返す想定）
int value = -1;
int count = 0;

while (value <= 0) {
    value = count + 1;  // 仮の入力値として count+1 を使用
    count++;
    System.out.println("試行 " + count + ": value = " + value);
    if (value > 0) break;
}
System.out.println("有効な値: " + value);
```

---

### コード例：数当てゲームのシミュレーション

```java
public class WhileExample {
    public static void main(String[] args) {
        int secret = 7;
        int guess  = 1;
        int tries  = 0;

        while (guess != secret) {
            tries++;
            System.out.println("試行 " + tries + ": " + guess + " → はずれ");
            guess++;
        }

        tries++;
        System.out.println("試行 " + tries + ": " + guess + " → 正解！");
    }
}
```

**出力結果：**
```
試行 1: 1 → はずれ
試行 2: 2 → はずれ
試行 3: 3 → はずれ
試行 4: 4 → はずれ
試行 5: 5 → はずれ
試行 6: 6 → はずれ
試行 7: 7 → 正解！
```

---

## ✏️ 問題

### 問題 4-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int n = 1;

        while (n <= 16) {
            System.out.println(n);
            n *= 2;
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
1
2
4
8
16
```

**解説：**
- `n` が `1` から始まり、ループのたびに `2` 倍になります（1→2→4→8→16）。
- `n = 32` のとき `n <= 16` が `false` になりループ終了。

</details>

---

### 問題 4-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int i = 5;

        // while
        while (i < 3) {
            System.out.println("while: " + i);
            i++;
        }
        System.out.println("while後: " + i);

        // do-while
        do {
            System.out.println("do-while: " + i);
            i++;
        } while (i < 3);
        System.out.println("do-while後: " + i);
    }
}
```

<details>
<summary>答えを見る</summary>

```
while後: 5
do-while: 5
do-while後: 6
```

**解説：**
- `while (i < 3)` は最初から `5 < 3` が `false` なので1回も実行されません。`i` はそのまま `5`。
- `do-while` は条件に関わらず先に処理が実行されるため、`"do-while: 5"` が出力されます。
- 処理後に `i++` で `i = 6` になり、`6 < 3` は `false` なのでループ終了。

</details>

---

### 問題 4-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `DigitCounter` クラスを作成してください。

**条件：**
- `int` 型の変数 `number` に `123456` を代入する
- `while` 文を使って `number` を10で割り続け、桁数を数える
- 各ステップで現在の `number` の値を出力する
- 最後に合計の桁数を出力する

**期待される出力：**
```
123456
12345
1234
123
12
1
桁数: 6
```

> 💡 ヒント：`number` を `10 / 10` で割り続け、`0` になったらループを終了します。  
> 桁数のカウントは `while` ループの外で初期化し、ループ内でカウントアップします。

<details>
<summary>答えを見る</summary>

```java
public class DigitCounter {
    public static void main(String[] args) {
        int number = 123456;
        int digits = 0;

        while (number > 0) {
            System.out.println(number);
            number /= 10;
            digits++;
        }

        System.out.println("桁数: " + digits);
    }
}
```

**解説：**
- `number > 0` の間ループが続きます。
- `number /= 10` で桁を1つずつ削り（整数除算）、`digits` をカウントアップします。
- `123456 → 12345 → 1234 → 123 → 12 → 1 → 0` で6回繰り返されるため桁数は `6`。

</details>
