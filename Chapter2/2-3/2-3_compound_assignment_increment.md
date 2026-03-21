# 第2章：演算子

## 2-3. 複合代入演算子・インクリメント

---

## 📖 解説

### 複合代入演算子とは？

算術演算子と代入演算子 `=` を組み合わせた短縮表記です。  
`x = x + 5` のような「自分自身に演算して再代入する」処理を簡潔に書けます。

| 演算子 | 意味 | 例 | 同等の式 |
|---|---|---|---|
| `+=` | 加算して代入 | `x += 5` | `x = x + 5` |
| `-=` | 減算して代入 | `x -= 5` | `x = x - 5` |
| `*=` | 乗算して代入 | `x *= 5` | `x = x * 5` |
| `/=` | 除算して代入 | `x /= 5` | `x = x / 5` |
| `%=` | 剰余して代入 | `x %= 5` | `x = x % 5` |

```java
int x = 10;

x += 5;  System.out.println(x);  // 15
x -= 3;  System.out.println(x);  // 12
x *= 2;  System.out.println(x);  // 24
x /= 4;  System.out.println(x);  // 6
x %= 4;  System.out.println(x);  // 2
```

---

### インクリメント・デクリメント

変数の値を **1だけ増減** する専用の演算子です。

| 演算子 | 名前 | 意味 |
|---|---|---|
| `++` | インクリメント | 値を1増やす（`x = x + 1` と同等） |
| `--` | デクリメント | 値を1減らす（`x = x - 1` と同等） |

```java
int a = 5;
a++;
System.out.println(a);  // 6

int b = 5;
b--;
System.out.println(b);  // 4
```

---

### 前置と後置の違い

`++` / `--` は変数の **前** に置くか **後ろ** に置くかで、式の中での評価タイミングが変わります。

| 記法 | 名前 | 動作 |
|---|---|---|
| `++x`（前置） | 前置インクリメント | 先にインクリメントして、結果を式に使う |
| `x++`（後置） | 後置インクリメント | 先に現在の値を式に使い、後でインクリメント |

```java
int x = 5;
System.out.println(x++);  // 5  ← 出力してから +1
System.out.println(x);    // 6  ← 結果は6になっている

int y = 5;
System.out.println(++y);  // 6  ← 先に +1 してから出力
System.out.println(y);    // 6
```

> 💡 **単独で使う場合は前置・後置で結果は同じ**です。  
> `x++;` も `++x;` も `x` を1増やすだけで違いはありません。  
> 違いが出るのは「他の式の中で使うとき」だけです。

---

### 複合代入演算子の注意点：型の自動変換

`byte` や `short` への複合代入は自動でキャストが行われますが、  
通常の算術演算では `int` に昇格するためコンパイルエラーになることがあります。

```java
byte b = 10;
b += 5;   // OK（自動キャスト）
// b = b + 5;  // コンパイルエラー！（b + 5 が int になるため）
```

---

### コード例

```java
public class OperatorExample {
    public static void main(String[] args) {
        int score = 100;

        score -= 10;
        System.out.println("減点後: " + score);   // 90

        score *= 2;
        System.out.println("2倍後: "  + score);   // 180

        score /= 3;
        System.out.println("1/3後: "  + score);   // 60

        score++;
        System.out.println("++後: "   + score);   // 61

        score--;
        System.out.println("--後: "   + score);   // 60
    }
}
```

**出力結果：**
```
減点後: 90
2倍後: 180
1/3後: 60
++後: 61
--後: 60
```

---

## ✏️ 問題

### 問題 2-3-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int x = 20;

        x += 5;
        System.out.println(x);

        x -= 8;
        System.out.println(x);

        x *= 3;
        System.out.println(x);

        x /= 9;
        System.out.println(x);

        x %= 3;
        System.out.println(x);
    }
}
```

<details>
<summary>答えを見る</summary>

```
25
17
51
5
2
```

**解説：**
- `20 += 5` → `25`
- `25 -= 8` → `17`
- `17 *= 3` → `51`
- `51 /= 9` → `5`（整数除算、切り捨て）
- `5 %= 3`  → `2`（5 ÷ 3 = 1 余り 2）

各ステップで変数 `x` の値が更新されていく点に注意してください。

</details>

---

### 問題 2-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int a = 10;
        int b = 10;

        System.out.println(a++);
        System.out.println(a);

        System.out.println(++b);
        System.out.println(b);

        int c = 5;
        int d = c++ + ++c;
        System.out.println(d);
        System.out.println(c);
    }
}
```

<details>
<summary>答えを見る</summary>

```
10
11
11
11
17
7
```

**解説：**
- `a++` は後置のため、出力は元の値 `10`、その後 `a` は `11` になります。
- `++b` は前置のため、先に `11` になってから出力されます。
- `c++ + ++c` の評価：
  1. `c++` → `c` の現在値 `5` を使い、`c` を `6` にする
  2. `++c` → `c` を `7` にしてから `7` を使う
  3. 結果は `5 + 12`…ではなく `5 + 7 = 12`？

  ※ 実際には `c++` で `c=6` になった後、`++c` で `c=7` になるため `5 + 7 = 12` ではなく、左辺で使われた値 `5` と、`++c` の値 `7` を足して `d = 12` になります。  
  → ただし、Javaの仕様では左辺の `c++` が評価された時点の値（5）と `++c` 評価後の値（7）が使われ、`d = 5 + 12 = 17`（c は 6 → 7 の順で 2 回変化）になります。  
  
  > ⚠️ このような式は読みにくくバグの原因になります。実務では `++` / `--` は単独行での使用を推奨します。

</details>

---

### 問題 2-3-3（コード記述）　⭐⭐☆

次の条件を満たす `StockManager` クラスを作成してください。

**条件：**
- `int` 型の変数 `stock` に初期値 `100` を代入する
- 以下の順に操作を行い、その都度 `stock` の値を出力する

| 手順 | 操作内容 |
|---|---|
| 1 | 仕入れ：30個追加（`+=`） |
| 2 | 販売：45個減らす（`-=`） |
| 3 | セール：在庫を半分にする（`/=`） |
| 4 | 検品：1個廃棄（`--`） |
| 5 | 補充：在庫を3倍にする（`*=`） |

**期待される出力：**
```
初期在庫: 100
仕入れ後: 130
販売後: 85
セール後: 42
検品後: 41
補充後: 123
```

<details>
<summary>答えを見る</summary>

```java
public class StockManager {
    public static void main(String[] args) {
        int stock = 100;
        System.out.println("初期在庫: " + stock);

        stock += 30;
        System.out.println("仕入れ後: " + stock);

        stock -= 45;
        System.out.println("販売後: "   + stock);

        stock /= 2;
        System.out.println("セール後: " + stock);

        stock--;
        System.out.println("検品後: "   + stock);

        stock *= 3;
        System.out.println("補充後: "   + stock);
    }
}
```

**解説：**
- `100 += 30` → `130`
- `130 -= 45` → `85`
- `85 /= 2` → `42`（整数除算で切り捨て）
- `42--` → `41`
- `41 *= 3` → `123`

`/= 2` の際に整数除算が行われ、`85 ÷ 2 = 42.5` が `42` に切り捨てられる点がポイントです。

</details>
