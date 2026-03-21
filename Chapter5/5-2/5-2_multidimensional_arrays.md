# 第5章：配列

## 5-2. 多次元配列

---

## 📖 解説

### 多次元配列とは？

多次元配列とは、**配列の配列**です。  
表（行・列）のようなデータを扱うときに使います。  
最もよく使われるのは **2次元配列** です。

```
int[][] matrix = {
    {1, 2, 3},   // 0行目
    {4, 5, 6},   // 1行目
    {7, 8, 9}    // 2行目
};

          列0  列1  列2
行0  →  [  1,   2,   3 ]
行1  →  [  4,   5,   6 ]
行2  →  [  7,   8,   9 ]
```

---

### 2次元配列の宣言方法

```java
// 方法1：宣言と同時に初期化
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// 方法2：行数・列数を指定（初期値は 0）
int[][] grid = new int[3][4];  // 3行4列、全要素 0

// 方法3：行数だけ指定して後から列を設定（ジャグ配列）
int[][] jagged = new int[3][];
jagged[0] = new int[]{1, 2};
jagged[1] = new int[]{3, 4, 5};
jagged[2] = new int[]{6};
```

---

### 要素へのアクセス

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// [行インデックス][列インデックス]
System.out.println(matrix[0][0]);  // 1（0行0列）
System.out.println(matrix[1][2]);  // 6（1行2列）
System.out.println(matrix[2][1]);  // 8（2行1列）

// 書き換え
matrix[0][1] = 99;
System.out.println(matrix[0][1]);  // 99
```

---

### length の使い方

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

System.out.println(matrix.length);     // 3（行数）
System.out.println(matrix[0].length);  // 3（0行目の列数）
System.out.println(matrix[1].length);  // 3（1行目の列数）
```

---

### 2次元配列のループ処理

ネストした `for` 文を使って全要素にアクセスします。

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// インデックスを使う場合
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}

// 拡張for文を使う場合
for (int[] row : matrix) {
    for (int val : row) {
        System.out.print(val + " ");
    }
    System.out.println();
}
```

**出力結果（どちらも同じ）：**
```
1 2 3
4 5 6
7 8 9
```

---

### 行ごとに列数が異なるジャグ配列

```java
int[][] jagged = {
    {1, 2},
    {3, 4, 5},
    {6}
};

for (int[] row : jagged) {
    for (int val : row) {
        System.out.print(val + " ");
    }
    System.out.println();
}
```

**出力結果：**
```
1 2
3 4 5
6
```

---

### コード例：成績表の集計

```java
public class GradeTable {
    public static void main(String[] args) {
        // 3人 × 3教科の成績
        int[][] grades = {
            {80, 75, 90},  // 生徒0
            {65, 88, 72},  // 生徒1
            {91, 60, 85}   // 生徒2
        };

        for (int i = 0; i < grades.length; i++) {
            int sum = 0;
            for (int score : grades[i]) {
                sum += score;
            }
            System.out.println("生徒" + i + " 合計: " + sum
                + " 平均: " + (double) sum / grades[i].length);
        }
    }
}
```

**出力結果：**
```
生徒0 合計: 245 平均: 81.66666666666667
生徒1 合計: 225 平均: 75.0
生徒2 合計: 236 平均: 78.66666666666667
```

---

## ✏️ 問題

### 問題 5-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int[][] arr = {
            {10, 20, 30},
            {40, 50, 60}
        };

        System.out.println(arr.length);
        System.out.println(arr[0].length);
        System.out.println(arr[0][1]);
        System.out.println(arr[1][2]);

        arr[0][0] = 99;
        System.out.println(arr[0][0]);
    }
}
```

<details>
<summary>答えを見る</summary>

```
2
3
20
60
99
```

**解説：**
- `arr.length` は行数 `2`。
- `arr[0].length` は0行目の列数 `3`。
- `arr[0][1]` は0行1列 → `20`。
- `arr[1][2]` は1行2列 → `60`。
- `arr[0][0] = 99` で上書き後 → `99`。

</details>

---

### 問題 5-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int[][] matrix = new int[3][3];

        // 対角線に値を設定
        for (int i = 0; i < matrix.length; i++) {
            matrix[i][i] = i + 1;
        }

        for (int[] row : matrix) {
            for (int val : row) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
1 0 0
0 2 0
0 0 3
```

**解説：**
- `new int[3][3]` の初期値は全て `0`。
- `matrix[i][i] = i + 1` で対角線（左上から右下）の要素に値を設定します。
  - `i=0`: `matrix[0][0] = 1`
  - `i=1`: `matrix[1][1] = 2`
  - `i=2`: `matrix[2][2] = 3`
- 対角線以外は `0` のまま。

</details>

---

### 問題 5-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `ClassScores` クラスを作成してください。

**条件：**
- 以下の2次元配列 `scores` を宣言する（4人 × 3教科）

```
生徒0: {70, 85, 60}
生徒1: {90, 78, 88}
生徒2: {55, 62, 71}
生徒3: {80, 95, 73}
```

- 各生徒の合計点と平均点を出力する
- 全生徒の中の最高点・最低点を出力する
- 教科ごとの平均点を出力する（教科0・教科1・教科2）

**期待される出力：**
```
生徒0: 合計=215, 平均=71.67
生徒1: 合計=256, 平均=85.33
生徒2: 合計=188, 平均=62.67
生徒3: 合計=248, 平均=82.67
全体最高点: 95
全体最低点: 55
教科0の平均: 73.75
教科1の平均: 80.0
教科2の平均: 73.0
```

> 💡 ヒント：平均を小数第2位で丸めるには `Math.round(avg * 100) / 100.0` を使います。

<details>
<summary>答えを見る</summary>

```java
public class ClassScores {
    public static void main(String[] args) {
        int[][] scores = {
            {70, 85, 60},
            {90, 78, 88},
            {55, 62, 71},
            {80, 95, 73}
        };

        int totalMax = scores[0][0];
        int totalMin = scores[0][0];

        // 各生徒の集計
        for (int i = 0; i < scores.length; i++) {
            int sum = 0;
            for (int score : scores[i]) {
                sum += score;
                if (score > totalMax) totalMax = score;
                if (score < totalMin) totalMin = score;
            }
            double avg = Math.round((double) sum / scores[i].length * 100) / 100.0;
            System.out.println("生徒" + i + ": 合計=" + sum + ", 平均=" + avg);
        }

        System.out.println("全体最高点: " + totalMax);
        System.out.println("全体最低点: " + totalMin);

        // 教科ごとの平均
        int subjects = scores[0].length;
        for (int j = 0; j < subjects; j++) {
            int sum = 0;
            for (int i = 0; i < scores.length; i++) {
                sum += scores[i][j];
            }
            System.out.println("教科" + j + "の平均: " + (double) sum / scores.length);
        }
    }
}
```

**解説：**
- 外側のループで各生徒（行）を処理し、内側のループで各教科（列）を処理します。
- 最大・最小は全要素を走査しながら同時に更新します。
- 教科ごとの平均は列方向（縦）に走査します（`scores[i][j]` の `j` を固定して `i` を変化）。

</details>
