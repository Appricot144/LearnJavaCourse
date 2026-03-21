# 第1章：変数とデータ型

## 1-3. 文字列（String）の操作

---

## 📖 解説

### String とは？

`String` はJavaで文字列を扱うためのクラスです。  
`int` や `double` などのプリミティブ型とは異なり、**参照型（オブジェクト型）** です。

```java
String name = "Java";       // ダブルクォートで囲む
String empty = "";          // 空文字列
String nullStr = null;      // null（何も参照していない状態）
```

> ⚠️ `char` はシングルクォート `'A'`、`String` はダブルクォート `"A"` を使います。

---

### 文字列の連結

`+` 演算子で文字列を連結できます。数値との連結も可能です。

```java
String firstName = "山田";
String lastName  = "太郎";

String fullName = lastName + " " + firstName;
System.out.println(fullName);   // 山田 太郎

int age = 25;
System.out.println("年齢: " + age);  // 年齢: 25
```

---

### よく使う String のメソッド

| メソッド | 説明 | 例 |
|---|---|---|
| `length()` | 文字数を返す | `"Java".length()` → `4` |
| `charAt(i)` | i番目の文字を返す | `"Java".charAt(0)` → `'J'` |
| `substring(i)` | i文字目以降を返す | `"Java".substring(1)` → `"ava"` |
| `substring(i, j)` | i〜j-1文字目を返す | `"Java".substring(0, 2)` → `"Ja"` |
| `toUpperCase()` | 大文字に変換 | `"java".toUpperCase()` → `"JAVA"` |
| `toLowerCase()` | 小文字に変換 | `"JAVA".toLowerCase()` → `"java"` |
| `trim()` | 前後の空白を除去 | `"  hi  ".trim()` → `"hi"` |
| `contains(s)` | 文字列を含むか | `"Java".contains("av")` → `true` |
| `replace(a, b)` | 文字列を置換 | `"Java".replace("a", "o")` → `"Jovo"` |
| `equals(s)` | 文字列が等しいか | `"abc".equals("abc")` → `true` |
| `indexOf(s)` | 最初に現れる位置 | `"Java".indexOf("a")` → `1` |

---

### 文字列の比較

文字列の比較は `==` ではなく **`equals()`** を使います。

```java
String a = "hello";
String b = "hello";
String c = new String("hello");

System.out.println(a == b);          // true（同じ文字列リテラルを参照）
System.out.println(a == c);          // false（異なるオブジェクト）
System.out.println(a.equals(c));     // true  ← 内容の比較には equals() を使う
```

> ⚠️ `==` はオブジェクトの参照先を比較します。内容の比較には必ず `equals()` を使いましょう。

---

### コード例

```java
public class StringExample {
    public static void main(String[] args) {
        String s = "Hello, Java!";

        System.out.println(s.length());           // 12
        System.out.println(s.toUpperCase());      // HELLO, JAVA!
        System.out.println(s.substring(7));       // Java!
        System.out.println(s.substring(7, 11));   // Java
        System.out.println(s.contains("Java"));   // true
        System.out.println(s.replace("Java", "World")); // Hello, World!
        System.out.println(s.charAt(0));          // H
        System.out.println(s.indexOf("Java"));    // 7
    }
}
```

**出力結果：**
```
12
HELLO, JAVA!
Java!
Java
true
Hello, World!
H
7
```

---

## ✏️ 問題

### 問題 1-3-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        String s = "Programming";

        System.out.println(s.length());
        System.out.println(s.charAt(0));
        System.out.println(s.toUpperCase());
        System.out.println(s.toLowerCase());
        System.out.println(s.substring(3));
    }
}
```

<details>
<summary>答えを見る</summary>

```
11
P
PROGRAMMING
programming
gramming
```

**解説：**
- `"Programming"` は11文字なので `length()` は `11`。
- `charAt(0)` は0番目（先頭）の文字 `'P'`。
- `substring(3)` は3文字目（`'g'`）以降なので `"gramming"`。

</details>

---

### 問題 1-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        String s = "  Hello, World!  ";

        System.out.println(s.trim());
        System.out.println(s.trim().length());
        System.out.println(s.trim().replace("World", "Java"));
        System.out.println(s.trim().contains("Java"));
        System.out.println(s.trim().indexOf("o"));
    }
}
```

<details>
<summary>答えを見る</summary>

```
Hello, World!
13
Hello, Java!
false
4
```

**解説：**
- `trim()` で前後の空白が除去され `"Hello, World!"` になります。
- `trim().length()` は `"Hello, World!"` の文字数 `13`。
- `trim().replace("World", "Java")` で `"Hello, Java!"` になります。
- `trim().contains("Java")` は `"Hello, World!"` に `"Java"` が含まれないので `false`。
- `indexOf("o")` は最初に現れる `'o'` の位置（0始まり）で `4`（`Hello` の `o`）。

</details>

---

### 問題 1-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `NameFormatter` クラスを作成してください。

**条件：**
- `String` 型の変数 `firstName` に `"  taro  "` を代入する
- `String` 型の変数 `lastName` に `"  yamada  "` を代入する
- 前後の空白を除去してから、それぞれ先頭を大文字・残りを小文字に整形する
  - ヒント：`substring(0, 1).toUpperCase()` で先頭1文字を大文字に、`substring(1).toLowerCase()` で2文字目以降を小文字にできます
- `"姓: 名:"` の形式で出力する

**期待される出力：**
```
姓: Yamada
名: Taro
フルネーム: Yamada Taro
文字数: 11
```

<details>
<summary>答えを見る</summary>

```java
public class NameFormatter {
    public static void main(String[] args) {
        String firstName = "  taro  ";
        String lastName  = "  yamada  ";

        // 空白除去
        String f = firstName.trim();
        String l = lastName.trim();

        // 先頭大文字・残り小文字に整形
        String formattedFirst = f.substring(0, 1).toUpperCase() + f.substring(1).toLowerCase();
        String formattedLast  = l.substring(0, 1).toUpperCase() + l.substring(1).toLowerCase();

        String fullName = formattedLast + " " + formattedFirst;

        System.out.println("姓: " + formattedLast);
        System.out.println("名: " + formattedFirst);
        System.out.println("フルネーム: " + fullName);
        System.out.println("文字数: " + fullName.length());
    }
}
```

**解説：**
- `trim()` で前後の空白を除去します。
- `substring(0, 1).toUpperCase()` で先頭1文字を大文字に変換します。
- `substring(1).toLowerCase()` で2文字目以降を小文字に変換します。
- 2つを `+` で連結することで先頭だけ大文字の文字列が完成します。
- `"Yamada Taro"` は11文字（スペース含む）なので `length()` は `11`。

</details>
