# 第5章：配列

## 5-1. 一次元配列の宣言・操作

---

## 📖 解説

### 配列とは？

配列とは、**同じ型の値を複数まとめて管理できる変数**です。  
番号（インデックス）で各要素にアクセスします。インデックスは **0から始まります**。

```
int[] scores = {80, 65, 90, 70, 55};

インデックス:    0    1    2    3    4
値:             80   65   90   70   55
```

---

### 配列の宣言方法

```java
// 方法1：宣言と同時に値を初期化
int[] scores = {80, 65, 90, 70, 55};

// 方法2：サイズを指定して宣言（初期値は型のデフォルト値）
int[] nums = new int[5];    // 要素は全て 0
String[] names = new String[3]; // 要素は全て null

// 方法3：宣言後に new で初期化
int[] data;
data = new int[]{10, 20, 30};
```

---

### 型ごとのデフォルト値

| 型 | デフォルト値 |
|---|---|
| `int` / `long` / `short` / `byte` | `0` |
| `double` / `float` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'`（null文字） |
| 参照型（`String` など） | `null` |

---

### 要素へのアクセス

```java
int[] scores = {80, 65, 90, 70, 55};

// 読み取り
System.out.println(scores[0]);  // 80
System.out.println(scores[2]);  // 90

// 書き込み
scores[1] = 75;
System.out.println(scores[1]);  // 75（変更後）
```

---

### 配列の長さ：length

`配列名.length` で配列の要素数を取得できます。

```java
int[] scores = {80, 65, 90, 70, 55};
System.out.println(scores.length);  // 5
```

> ⚠️ `length` はメソッドではなくフィールドです。`()` は不要です。  
> （String の `length()` とは異なります）

---

### 配列の操作例

```java
int[] nums = {3, 1, 4, 1, 5, 9, 2, 6};

// 最大値を求める
int max = nums[0];
for (int n : nums) {
    if (n > max) max = n;
}
System.out.println("最大値: " + max);  // 最大値: 9

// 合計を求める
int sum = 0;
for (int n : nums) {
    sum += n;
}
System.out.println("合計: " + sum);    // 合計: 31
```

---

### 範囲外アクセス（ArrayIndexOutOfBoundsException）

配列の範囲外のインデックスにアクセスすると例外が発生します。

```java
int[] arr = {10, 20, 30};

System.out.println(arr[0]);   // 10  ← OK
System.out.println(arr[2]);   // 30  ← OK
System.out.println(arr[3]);   // ❌ ArrayIndexOutOfBoundsException！
```

> ⚠️ 有効なインデックスは `0` 〜 `length - 1` です。

---

### コード例

```java
public class ArrayExample {
    public static void main(String[] args) {
        int[] scores = {85, 62, 90, 71, 55};

        System.out.println("要素数: " + scores.length);  // 5

        // 全要素を出力
        for (int i = 0; i < scores.length; i++) {
            System.out.println("scores[" + i + "] = " + scores[i]);
        }

        // 最小値を求める
        int min = scores[0];
        for (int score : scores) {
            if (score < min) min = score;
        }
        System.out.println("最小値: " + min);  // 55
    }
}
```

**出力結果：**
```
要素数: 5
scores[0] = 85
scores[1] = 62
scores[2] = 90
scores[3] = 71
scores[4] = 55
最小値: 55
```

---

## ✏️ 問題

### 問題 5-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        String[] fruits = {"apple", "banana", "cherry", "date"};

        System.out.println(fruits.length);
        System.out.println(fruits[0]);
        System.out.println(fruits[2]);

        fruits[1] = "blueberry";
        System.out.println(fruits[1]);
    }
}
```

<details>
<summary>答えを見る</summary>

```
4
apple
cherry
blueberry
```

**解説：**
- `fruits.length` は要素数 `4`。
- `fruits[0]` は先頭要素 `"apple"`。
- `fruits[2]` は3番目の要素 `"cherry"`（インデックスは0始まり）。
- `fruits[1] = "blueberry"` で上書き後、`fruits[1]` は `"blueberry"`。

</details>

---

### 問題 5-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int[] nums = new int[5];
        System.out.println(nums[0]);
        System.out.println(nums[4]);

        for (int i = 0; i < nums.length; i++) {
            nums[i] = (i + 1) * 10;
        }

        int sum = 0;
        for (int n : nums) {
            sum += n;
        }

        System.out.println(nums[2]);
        System.out.println(sum);
    }
}
```

<details>
<summary>答えを見る</summary>

```
0
0
30
150
```

**解説：**
- `new int[5]` で宣言した配列の初期値は全て `0`。
- ループで `nums[i] = (i+1)*10` → `{10, 20, 30, 40, 50}`。
- `nums[2]` は `30`。
- 合計は `10+20+30+40+50 = 150`。

</details>

---

### 問題 5-1-3（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q3 {
    public static void main(String[] args) {
        int[] data = {5, 2, 8, 1, 9, 3, 7, 4, 6};

        int max = data[0];
        int min = data[0];
        int sum = 0;

        for (int d : data) {
            if (d > max) max = d;
            if (d < min) min = d;
            sum += d;
        }

        System.out.println("最大: " + max);
        System.out.println("最小: " + min);
        System.out.println("合計: " + sum);
        System.out.println("平均: " + (double) sum / data.length);
    }
}
```

<details>
<summary>答えを見る</summary>

```
最大: 9
最小: 1
合計: 45
平均: 5.0
```

**解説：**
- `data` は `{5,2,8,1,9,3,7,4,6}` の9要素。
- 最大は `9`、最小は `1`。
- 合計は `5+2+8+1+9+3+7+4+6 = 45`。
- 平均は `45 / 9 = 5.0`。

</details>

---

### 問題 5-1-4（コード記述）　⭐⭐⭐

次の条件を満たす `ScoreAnalyzer` クラスを作成してください。

**条件：**
- `int` 型配列 `scores` に `{72, 88, 55, 91, 63, 77, 84, 49, 95, 68}` を代入する
- 以下をすべて出力する

| 出力内容 | 説明 |
|---|---|
| 受験者数 | `scores.length` |
| 合計点 | 全要素の合計 |
| 平均点 | 小数で出力 |
| 最高点 | 最大値 |
| 最低点 | 最小値 |
| 合格者数 | 70点以上の人数 |

**期待される出力：**
```
受験者数: 10
合計点: 742
平均点: 74.2
最高点: 95
最低点: 49
合格者数: 7
```

<details>
<summary>答えを見る</summary>

```java
public class ScoreAnalyzer {
    public static void main(String[] args) {
        int[] scores = {72, 88, 55, 91, 63, 77, 84, 49, 95, 68};

        int sum    = 0;
        int max    = scores[0];
        int min    = scores[0];
        int passed = 0;

        for (int score : scores) {
            sum += score;
            if (score > max) max = score;
            if (score < min) min = score;
            if (score >= 70) passed++;
        }

        System.out.println("受験者数: " + scores.length);
        System.out.println("合計点: "   + sum);
        System.out.println("平均点: "   + (double) sum / scores.length);
        System.out.println("最高点: "   + max);
        System.out.println("最低点: "   + min);
        System.out.println("合格者数: " + passed);
    }
}
```

**解説：**
- 拡張for文で1回のループで合計・最大・最小・合格者数をすべて求めています。
- 合計 `742 / 10 = 74.2`（`(double)` キャストが必要）。
- 70点以上：72・88・91・77・84・95・68… → 68は除外。72・88・91・77・84・95・77 の7名。

</details>
