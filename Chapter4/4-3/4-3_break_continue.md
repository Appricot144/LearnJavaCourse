# 第4章：繰り返し処理

## 4-3. break / continue

---

## 📖 解説

### break文とは？

`break` 文はループを**即座に終了**させる命令です。  
`switch` 文でも使いましたが、`for` や `while` でも同様に機能します。

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // i が 5 になったらループを終了
    }
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

### continue文とは？

`continue` 文は**その回の処理だけをスキップ**して、次の繰り返しに進む命令です。  
ループ自体は終了しません。

```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;  // 偶数のときはスキップ
    }
    System.out.println(i);
}
```

**出力結果：**
```
1
3
5
7
9
```

---

### break と continue の違い

| 命令 | 動作 | ループへの影響 |
|---|---|---|
| `break` | その時点でループを完全に終了 | ループを抜ける |
| `continue` | その回の処理だけスキップ | 次の繰り返しに進む |

```
break のイメージ：
i=0 → i=1 → i=2 → i=3 → [break!] → ループ終了

continue のイメージ：
i=0 → [skip] → i=1 → i=2 → [skip] → i=3 → ...
```

---

### while文と break

`while (true)` と組み合わせて、条件を満たしたときに `break` で抜ける書き方がよく使われます。

```java
int i = 0;

while (true) {  // 無限ループ
    if (i >= 5) {
        break;  // 条件を満たしたら脱出
    }
    System.out.println(i);
    i++;
}
System.out.println("終了");
```

**出力結果：**
```
0
1
2
3
4
終了
```

---

### ネストしたループでの break / continue

`break` や `continue` は**自分が属する最も内側のループ**にのみ作用します。

```java
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            break;  // 内側のループだけ終了
        }
        System.out.println(i + "-" + j);
    }
}
```

**出力結果：**
```
0-0
1-0
2-0
```

> 💡 外側のループを `break` したい場合はラベル付き break（`break ラベル名;`）を使いますが、  
> 読みにくくなるためフラグ変数や別メソッドへの切り出しで代替するのが一般的です。

---

### コード例：素数判定

```java
public class PrimeChecker {
    public static void main(String[] args) {
        int num = 17;
        boolean isPrime = true;

        if (num < 2) {
            isPrime = false;
        } else {
            for (int i = 2; i < num; i++) {
                if (num % i == 0) {
                    isPrime = false;
                    break;  // 割り切れた時点で終了
                }
            }
        }

        System.out.println(num + " は素数: " + isPrime);  // 17 は素数: true
    }
}
```

---

## ✏️ 問題

### 問題 4-3-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            if (i % 3 == 0) {
                continue;
            }
            if (i == 8) {
                break;
            }
            System.out.println(i);
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
5
7
```

**解説：**
- `i` が 1〜10 まで増えますが、`i % 3 == 0`（3の倍数）のときは `continue` でスキップされます。
- `i == 8` のとき `break` でループ終了。
- 3の倍数（3・6）はスキップ、8以降は出力されないため `1・2・4・5・7` が出力されます。

</details>

---

### 問題 4-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int count = 0;

        for (int i = 1; i <= 20; i++) {
            if (i % 2 == 0) continue;
            if (i % 5 == 0) break;
            count++;
            System.out.print(i + " ");
        }

        System.out.println();
        System.out.println("count: " + count);
    }
}
```

<details>
<summary>答えを見る</summary>

```
1 3 
count: 2
```

**解説：**
- `i = 1`：奇数かつ5の倍数でない → 出力。count=1
- `i = 2`：偶数 → `continue`（スキップ）
- `i = 3`：奇数かつ5の倍数でない → 出力。count=2
- `i = 4`：偶数 → `continue`（スキップ）
- `i = 5`：奇数 → 偶数チェックはパス、`i % 5 == 0` → `break`（ループ終了）
- よって出力は `1 3`、`count` は `2`。

</details>

---

### 問題 4-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `NumberFilter` クラスを作成してください。

**条件：**
- `for` 文で `1` から `30` まで繰り返す
- 以下のルールを適用する
  - **4の倍数**のときは `continue`（スキップ）
  - **出力した数の合計が 100 を超えたら** `break`（ループ終了）
- スキップ・終了した数以外を出力し、最後に合計を出力する

**期待される出力：**
```
1 2 3 5 6 7 9 10 11 13 14 15 17 18 19 21 
合計: 171
```

> 💡 ヒント：合計を保持する変数を用意し、`continue` の前に合計を加算しないように注意します。

<details>
<summary>答えを見る</summary>

```java
public class NumberFilter {
    public static void main(String[] args) {
        int sum = 0;

        for (int i = 1; i <= 30; i++) {
            if (i % 4 == 0) {
                continue;  // 4の倍数はスキップ
            }
            sum += i;
            System.out.print(i + " ");
            if (sum > 100) {
                break;  // 合計が100を超えたら終了
            }
        }

        System.out.println();
        System.out.println("合計: " + sum);
    }
}
```

**解説：**
- 4の倍数（4・8・12・16・20・24・28）はスキップされます。
- それ以外の数を順に加算し、`sum > 100` になった時点で `break`。
- `1+2+3+5+6+7+9+10+11+13+14+15+17+18+19+21 = 171` で `21` を加算した時点で100を超えます。
- `break` はその回の加算・出力の後に評価されるため、`21` は出力されます。

</details>
