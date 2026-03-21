# 第6章：メソッド

## 6-1. メソッドの定義と呼び出し

---

## 📖 解説

### メソッドとは？

メソッドとは、**一連の処理をまとめて名前を付けたもの**です。  
同じ処理を何度も書く代わりに、メソッドとして定義して呼び出すことができます。

**メソッドを使うメリット：**
- 同じ処理を繰り返し書かなくてよい（再利用性）
- 処理に名前が付くので読みやすくなる（可読性）
- 変更箇所が1カ所にまとまる（保守性）

---

### メソッドの基本構造

```
アクセス修飾子　static　戻り値の型　メソッド名(引数リスト) {
    // 処理
    return 戻り値;  // 戻り値がある場合
}
```

```java
public static void greet() {
    System.out.println("こんにちは！");
}
```

| 部分 | 説明 |
|---|---|
| `public` | どこからでも呼び出せる（アクセス修飾子） |
| `static` | インスタンスなしで呼び出せる |
| `void` | 戻り値なし |
| `greet` | メソッド名 |
| `()` | 引数なし |

---

### メソッドの呼び出し

定義したメソッドは `メソッド名()` で呼び出します。

```java
public class MethodExample {

    public static void greet() {
        System.out.println("こんにちは！");
    }

    public static void main(String[] args) {
        greet();  // メソッドの呼び出し
        greet();  // 何度でも呼び出せる
        greet();
    }
}
```

**出力結果：**
```
こんにちは！
こんにちは！
こんにちは！
```

---

### void メソッドと return

`void` メソッドは戻り値がありません。  
処理の途中でメソッドを終了させたいときは `return;` を使います。

```java
public static void checkAge(int age) {
    if (age < 0) {
        System.out.println("無効な年齢です");
        return;  // ここで終了
    }
    System.out.println("年齢: " + age);
}
```

---

### メソッドの実行フロー

```
main() {
    greet();  ───→  greet() {
    greet();  ←───      println("こんにちは！")
}                   }
```

`main` メソッドから `greet` メソッドが呼ばれ、処理が終わると `main` に戻ります。

---

### メソッドの命名規則

| ルール | 例 |
|---|---|
| 小文字で始めるキャメルケース | `calculateTotal`, `getUserName` |
| 動詞または動詞＋名詞 | `print`, `getScore`, `showResult` |
| 略語は避ける | `calculate` ○、`calc` △ |

---

### コード例：複数のメソッドを定義する

```java
public class ShapeDrawer {

    public static void drawLine() {
        System.out.println("──────────");
    }

    public static void drawBox() {
        System.out.println("┌────────┐");
        System.out.println("│        │");
        System.out.println("└────────┘");
    }

    public static void showMenu() {
        drawLine();
        System.out.println("  メニュー");
        drawLine();
        System.out.println("  1. スタート");
        System.out.println("  2. 終了");
        drawLine();
    }

    public static void main(String[] args) {
        showMenu();
        System.out.println();
        drawBox();
    }
}
```

**出力結果：**
```
──────────
  メニュー
──────────
  1. スタート
  2. 終了
──────────

┌────────┐
│        │
└────────┘
```

---

## ✏️ 問題

### 問題 6-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {

    public static void hello() {
        System.out.println("Hello!");
    }

    public static void bye() {
        System.out.println("Goodbye!");
    }

    public static void greetSet() {
        hello();
        bye();
    }

    public static void main(String[] args) {
        hello();
        greetSet();
        bye();
    }
}
```

<details>
<summary>答えを見る</summary>

```
Hello!
Hello!
Goodbye!
Goodbye!
```

**解説：**
- `main` で `hello()` が呼ばれ `"Hello!"` が出力されます。
- 次に `greetSet()` が呼ばれ、その中で `hello()` → `"Hello!"`、`bye()` → `"Goodbye!"` の順に実行されます。
- 最後に `main` の `bye()` が呼ばれ `"Goodbye!"` が出力されます。

</details>

---

### 問題 6-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {

    public static void printStars(int count) {
        for (int i = 0; i < count; i++) {
            System.out.print("*");
        }
        System.out.println();
    }

    public static void printTriangle() {
        printStars(1);
        printStars(2);
        printStars(3);
        printStars(4);
        printStars(5);
    }

    public static void main(String[] args) {
        printTriangle();
        System.out.println("---");
        printStars(3);
    }
}
```

<details>
<summary>答えを見る</summary>

```
*
**
***
****
*****
---
***
```

**解説：**
- `printTriangle()` は `printStars(1)` 〜 `printStars(5)` を順に呼び出し、1〜5個の `*` を出力します。
- `System.out.println("---")` でセパレータを出力。
- `printStars(3)` で `***` を出力します。

</details>

---

### 問題 6-1-3（コード記述）　⭐⭐⭐

次の条件を満たす `ReportPrinter` クラスを作成してください。

**条件：**
- 以下の3つのメソッドを定義する

| メソッド名 | 処理内容 |
|---|---|
| `printHeader()` | `"=== 成績レポート ==="` を出力 |
| `printSeparator()` | `"-------------------"` を出力 |
| `printFooter()` | `"=== 以上 ==="` を出力 |

- `main` メソッドで以下の形式でレポートを出力する

**期待される出力：**
```
=== 成績レポート ===
-------------------
名前: 山田太郎
点数: 85点
評価: B
-------------------
名前: 佐藤花子
点数: 92点
評価: A
-------------------
=== 以上 ===
```

<details>
<summary>答えを見る</summary>

```java
public class ReportPrinter {

    public static void printHeader() {
        System.out.println("=== 成績レポート ===");
    }

    public static void printSeparator() {
        System.out.println("-------------------");
    }

    public static void printFooter() {
        System.out.println("=== 以上 ===");
    }

    public static void main(String[] args) {
        printHeader();
        printSeparator();
        System.out.println("名前: 山田太郎");
        System.out.println("点数: 85点");
        System.out.println("評価: B");
        printSeparator();
        System.out.println("名前: 佐藤花子");
        System.out.println("点数: 92点");
        System.out.println("評価: A");
        printSeparator();
        printFooter();
    }
}
```

**解説：**
- 繰り返し使うセパレータ行をメソッドにまとめることで、変更が必要なときに1カ所だけ修正すれば済みます。
- `printSeparator()` は3回呼ばれていますが、定義は1カ所だけです。これがメソッドの再利用性の利点です。

</details>
