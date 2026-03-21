# 第3章：条件分岐

## 3-2. switch文

---

## 📖 解説

### switch文とは？

`switch` 文は、**1つの変数や式の値によって処理を分岐**させる構文です。  
`if / else if` の連続と同じことができますが、値が多い場合に読みやすくなります。

---

### 基本構造

```java
switch (式) {
    case 値1:
        // 式 == 値1 のときの処理
        break;
    case 値2:
        // 式 == 値2 のときの処理
        break;
    default:
        // どの case にも一致しないときの処理
}
```

> ⚠️ **`break` を忘れると次の `case` に処理が流れてしまいます（フォールスルー）。**

---

### コード例：曜日の判定

```java
int day = 3;

switch (day) {
    case 1:
        System.out.println("月曜日");
        break;
    case 2:
        System.out.println("火曜日");
        break;
    case 3:
        System.out.println("水曜日");  // ← これが実行される
        break;
    case 4:
        System.out.println("木曜日");
        break;
    case 5:
        System.out.println("金曜日");
        break;
    default:
        System.out.println("週末");
}
```

**出力結果：**
```
水曜日
```

---

### break を省略するとどうなる？（フォールスルー）

`break` を省略すると、一致した `case` 以降の処理がすべて実行されます。

```java
int x = 1;

switch (x) {
    case 1:
        System.out.println("one");
        // break なし！
    case 2:
        System.out.println("two");
        // break なし！
    case 3:
        System.out.println("three");
        break;
    default:
        System.out.println("other");
}
```

**出力結果：**
```
one
two
three
```

> 💡 フォールスルーを意図的に使うこともできます（後述の複数ラベルまとめ）。

---

### 複数の case をまとめる

同じ処理をする `case` は `break` を省略してまとめられます。

```java
int month = 4;

switch (month) {
    case 3:
    case 4:
    case 5:
        System.out.println("春");  // 3・4・5月はここへ
        break;
    case 6:
    case 7:
    case 8:
        System.out.println("夏");
        break;
    case 9:
    case 10:
    case 11:
        System.out.println("秋");
        break;
    default:
        System.out.println("冬");
}
```

**出力結果：**
```
春
```

---

### switch文で使える型

| 使える型 | 例 |
|---|---|
| `int` / `byte` / `short` / `char` | `switch (score)` |
| `String` | `switch (name)` |
| `enum`（列挙型） | `switch (color)` |

> ⚠️ `long` / `float` / `double` / `boolean` は使えません。

---

### switch文 vs if文

| 比較項目 | switch文 | if文 |
|---|---|---|
| 向いているケース | 値が多い・1変数の等値比較 | 範囲比較・複雑な条件 |
| 使える比較 | `==`（等値のみ） | `>=`、`&&` など自由 |
| 読みやすさ | 値が多い場合に○ | 条件が複雑な場合に○ |

---

### コード例（String の switch）

```java
public class SeasonChecker {
    public static void main(String[] args) {
        String season = "summer";

        switch (season) {
            case "spring":
                System.out.println("春：お花見の季節");
                break;
            case "summer":
                System.out.println("夏：海水浴の季節");  // ← これが実行される
                break;
            case "autumn":
                System.out.println("秋：紅葉の季節");
                break;
            case "winter":
                System.out.println("冬：スキーの季節");
                break;
            default:
                System.out.println("不明な季節");
        }
    }
}
```

**出力結果：**
```
夏：海水浴の季節
```

---

## ✏️ 問題

### 問題 3-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int num = 2;

        switch (num) {
            case 1:
                System.out.println("いち");
                break;
            case 2:
                System.out.println("に");
                break;
            case 3:
                System.out.println("さん");
                break;
            default:
                System.out.println("その他");
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
に
```

**解説：**
- `num = 2` なので `case 2:` に一致し、`"に"` が出力されます。
- `break` があるため `case 3:` 以降は実行されません。

</details>

---

### 問題 3-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int x = 2;

        switch (x) {
            case 1:
                System.out.println("A");
                break;
            case 2:
                System.out.println("B");
            case 3:
                System.out.println("C");
                break;
            case 4:
                System.out.println("D");
                break;
            default:
                System.out.println("Z");
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
B
C
```

**解説：**
- `x = 2` なので `case 2:` に一致し `"B"` が出力されます。
- `case 2:` に `break` がないため、フォールスルーが発生し `case 3:` の `"C"` も実行されます。
- `case 3:` に `break` があるため、そこで処理が終わります。

</details>

---

### 問題 3-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `TrafficLight` クラスを作成してください。

**条件：**
- `String` 型の変数 `color` に `"yellow"` を代入する
- `switch` 文を使って以下のメッセージを出力する

| color の値 | 出力メッセージ |
|---|---|
| `"red"` | 止まれ |
| `"yellow"` | 注意して進め |
| `"green"` | 進め |
| それ以外 | 不明な信号です |

- さらに `color` が `"red"` または `"yellow"` のとき、追加で `"安全を確認してください"` を出力する  
  （ヒント：`case` の複数ラベルとフォールスルーを活用）

**期待される出力：**
```
注意して進め
安全を確認してください
```

<details>
<summary>答えを見る</summary>

```java
public class TrafficLight {
    public static void main(String[] args) {
        String color = "yellow";

        switch (color) {
            case "red":
                System.out.println("止まれ");
                System.out.println("安全を確認してください");
                break;
            case "yellow":
                System.out.println("注意して進め");
                System.out.println("安全を確認してください");
                break;
            case "green":
                System.out.println("進め");
                break;
            default:
                System.out.println("不明な信号です");
        }
    }
}
```

**別解（フォールスルーを活用した場合）：**

```java
public class TrafficLight {
    public static void main(String[] args) {
        String color = "yellow";
        String message = "";

        switch (color) {
            case "red":
                message = "止まれ";
                break;
            case "yellow":
                message = "注意して進め";
                break;
            case "green":
                System.out.println("進め");
                return;
            default:
                System.out.println("不明な信号です");
                return;
        }

        System.out.println(message);
        System.out.println("安全を確認してください");
    }
}
```

**解説：**
- `color = "yellow"` なので `case "yellow":` に一致します。
- `"注意して進め"` と `"安全を確認してください"` が出力されます。
- 別解では `red` と `yellow` の共通処理（安全確認）を switch の外にまとめています。

</details>
