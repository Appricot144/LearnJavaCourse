# 第5章：配列

## 5-3. 配列の繰り返し処理

---

## 📖 解説

### 配列とループの組み合わせ

配列は繰り返し処理と組み合わせることで真価を発揮します。  
主に以下の3つの操作パターンがあります。

| パターン | 内容 |
|---|---|
| **走査（トラバース）** | 全要素を順番に参照・出力する |
| **検索** | 特定の値や条件に合う要素を見つける |
| **変換** | 各要素に処理を加えて新しい配列を作る |

---

### パターン1：走査（全要素の出力・集計）

```java
int[] nums = {3, 7, 2, 9, 1, 5};

// インデックス付きで出力
for (int i = 0; i < nums.length; i++) {
    System.out.println("[" + i + "] " + nums[i]);
}

// 拡張for文で合計
int sum = 0;
for (int n : nums) {
    sum += n;
}
System.out.println("合計: " + sum);  // 合計: 27
```

---

### パターン2：検索（線形探索）

配列を先頭から順番に調べて、目的の値を探します。

```java
int[] nums = {3, 7, 2, 9, 1, 5};
int target = 9;
int foundIndex = -1;  // 見つからない場合のデフォルト値

for (int i = 0; i < nums.length; i++) {
    if (nums[i] == target) {
        foundIndex = i;
        break;  // 見つかったら終了
    }
}

if (foundIndex != -1) {
    System.out.println(target + " は index " + foundIndex + " にあります");
} else {
    System.out.println(target + " は見つかりませんでした");
}
// → 9 は index 3 にあります
```

---

### パターン3：変換（配列の要素を加工して新しい配列を作る）

```java
int[] original = {1, 2, 3, 4, 5};
int[] doubled  = new int[original.length];

for (int i = 0; i < original.length; i++) {
    doubled[i] = original[i] * 2;
}

for (int n : doubled) {
    System.out.print(n + " ");
}
// → 2 4 6 8 10
```

---

### 配列のコピー

配列を別の変数に代入しても、同じ配列を参照するだけです（参照コピー）。  
独立したコピーを作るには `Arrays.copyOf()` や `System.arraycopy()` を使います。

```java
int[] a = {1, 2, 3};

// 参照コピー（同じ配列を指す）
int[] b = a;
b[0] = 99;
System.out.println(a[0]);  // 99 ← a も変わる！

// 独立したコピー
int[] c = java.util.Arrays.copyOf(a, a.length);
c[0] = 55;
System.out.println(a[0]);  // 99 ← a は変わらない
```

> ⚠️ 配列は参照型のため `=` での代入は「コピー」ではなく「同じ配列への参照」になります。

---

### Arrays クラスのよく使うメソッド

`java.util.Arrays` には配列操作の便利なメソッドが揃っています。

```java
import java.util.Arrays;

int[] nums = {3, 1, 4, 1, 5, 9, 2, 6};

// ソート（昇順）
Arrays.sort(nums);
System.out.println(Arrays.toString(nums));
// → [1, 1, 2, 3, 4, 5, 6, 9]

// 配列を文字列に変換（出力確認に便利）
System.out.println(Arrays.toString(nums));

// コピー
int[] copy = Arrays.copyOf(nums, nums.length);

// 部分コピー（index 2〜4）
int[] part = Arrays.copyOfRange(nums, 2, 5);
System.out.println(Arrays.toString(part));
// → [2, 3, 4]
```

---

### コード例：配列の逆順出力と反転

```java
public class ArrayLoop {
    public static void main(String[] args) {
        int[] nums = {10, 20, 30, 40, 50};

        // 逆順に出力
        System.out.print("逆順: ");
        for (int i = nums.length - 1; i >= 0; i--) {
            System.out.print(nums[i] + " ");
        }
        System.out.println();

        // 配列を反転（in-place）
        for (int i = 0; i < nums.length / 2; i++) {
            int tmp = nums[i];
            nums[i] = nums[nums.length - 1 - i];
            nums[nums.length - 1 - i] = tmp;
        }

        System.out.print("反転後: ");
        for (int n : nums) {
            System.out.print(n + " ");
        }
        System.out.println();
    }
}
```

**出力結果：**
```
逆順: 50 40 30 20 10
反転後: 50 40 30 20 10
```

---

## ✏️ 問題

### 問題 5-3-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
import java.util.Arrays;

public class Q1 {
    public static void main(String[] args) {
        int[] nums = {5, 3, 8, 1, 9, 2, 7, 4, 6};

        int max = nums[0];
        int secondMax = Integer.MIN_VALUE;

        for (int n : nums) {
            if (n > max) {
                secondMax = max;
                max = n;
            } else if (n > secondMax && n != max) {
                secondMax = n;
            }
        }

        System.out.println("最大値: "    + max);
        System.out.println("2番目の最大値: " + secondMax);

        Arrays.sort(nums);
        System.out.println("ソート後: " + Arrays.toString(nums));
    }
}
```

<details>
<summary>答えを見る</summary>

```
最大値: 9
2番目の最大値: 8
ソート後: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**解説：**
- 最大値は `9`、2番目は `8`。
- `Arrays.sort()` で昇順ソート、`Arrays.toString()` で `[1, 2, 3, 4, 5, 6, 7, 8, 9]` と表示されます。

</details>

---

### 問題 5-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int[] a = {1, 2, 3};
        int[] b = a;          // 参照コピー
        int[] c = java.util.Arrays.copyOf(a, a.length);  // 独立コピー

        b[0] = 10;
        c[1] = 20;

        System.out.println(a[0]);
        System.out.println(a[1]);
        System.out.println(b[0]);
        System.out.println(c[0]);
        System.out.println(c[1]);
    }
}
```

<details>
<summary>答えを見る</summary>

```
10
2
10
1
20
```

**解説：**
- `b = a` は参照コピーなので、`b[0] = 10` にすると `a[0]` も `10` に変わります。
- `c = Arrays.copyOf(a, ...)` は独立したコピーなので、`c[1] = 20` にしても `a[1]` は `2` のままです。
- `c[0]` は `a` のコピー時点の値 `1`（`b[0]` の変更の影響を受けないため `1`）。

</details>

---

### 問題 5-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `ArrayProcessor` クラスを作成してください。

**条件：**
- `int` 型配列 `nums` に `{4, 7, 2, 9, 1, 5, 8, 3, 6, 10}` を代入する
- 以下の処理をすべて行い出力する

| 処理 | 内容 |
|---|---|
| 偶数のみ抽出 | 偶数を順番に出力（スペース区切り） |
| 線形探索 | 値 `7` が何番目（0始まり）にあるかを出力 |
| 2倍配列 | 全要素を2倍した新しい配列を作り出力 |
| 逆順出力 | 元の配列を逆順に出力 |

**期待される出力：**
```
偶数: 4 2 8 6 10 
7 のインデックス: 1
2倍: [8, 14, 4, 18, 2, 10, 16, 6, 12, 20]
逆順: 10 6 3 8 5 1 9 2 7 4 
```

<details>
<summary>答えを見る</summary>

```java
import java.util.Arrays;

public class ArrayProcessor {
    public static void main(String[] args) {
        int[] nums = {4, 7, 2, 9, 1, 5, 8, 3, 6, 10};

        // 偶数のみ抽出
        System.out.print("偶数: ");
        for (int n : nums) {
            if (n % 2 == 0) System.out.print(n + " ");
        }
        System.out.println();

        // 線形探索
        int target = 7;
        int idx = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) { idx = i; break; }
        }
        System.out.println(target + " のインデックス: " + idx);

        // 2倍配列
        int[] doubled = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            doubled[i] = nums[i] * 2;
        }
        System.out.println("2倍: " + Arrays.toString(doubled));

        // 逆順出力
        System.out.print("逆順: ");
        for (int i = nums.length - 1; i >= 0; i--) {
            System.out.print(nums[i] + " ");
        }
        System.out.println();
    }
}
```

**解説：**
- 偶数判定は `n % 2 == 0` で確認します。
- 線形探索は先頭から順に `nums[i] == target` をチェックし、見つかったら `break`。
- 2倍配列は新しい配列 `doubled` を作り、`nums[i] * 2` を代入します。
- 逆順は `i = nums.length - 1` から `i >= 0` の間、`i--` で走査します。

</details>
