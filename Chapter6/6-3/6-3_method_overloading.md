# 第6章：メソッド

## 6-3. メソッドのオーバーロード

---

## 📖 解説

### オーバーロードとは？

**同じ名前で、引数の型・個数・順番が異なる複数のメソッドを定義すること**をオーバーロード（多重定義）といいます。  
呼び出し時の引数に応じて、Javaが自動的に適切なメソッドを選択します。

```java
// 引数の型が違う → オーバーロード
public static void print(int n)    { System.out.println("int: "    + n); }
public static void print(double d) { System.out.println("double: " + d); }
public static void print(String s) { System.out.println("String: " + s); }

// 呼び出し
print(42);       // → int: 42
print(3.14);     // → double: 3.14
print("Hello");  // → String: Hello
```

---

### オーバーロードの条件

オーバーロードが成立するには、**引数リストが異なる**必要があります。

| 条件 | 説明 |
|---|---|
| ✅ 引数の**型**が異なる | `add(int, int)` と `add(double, double)` |
| ✅ 引数の**個数**が異なる | `add(int)` と `add(int, int)` |
| ✅ 引数の**順番**が異なる | `add(int, double)` と `add(double, int)` |
| ❌ 戻り値の型だけが異なる | コンパイルエラー（オーバーロードにならない） |

---

### よくあるオーバーロードの例

#### 個数によるオーバーロード

```java
public static int add(int a, int b) {
    return a + b;
}

public static int add(int a, int b, int c) {
    return a + b + c;
}

System.out.println(add(1, 2));     // 3
System.out.println(add(1, 2, 3));  // 6
```

#### 型によるオーバーロード

```java
public static double area(double radius) {
    return Math.PI * radius * radius;
}

public static double area(double width, double height) {
    return width * height;
}

System.out.println(area(5.0));        // 78.53...（円の面積）
System.out.println(area(4.0, 6.0));   // 24.0（長方形の面積）
```

---

### オーバーロードと型の自動変換

引数の型が完全一致するメソッドがない場合、Javaは**自動的に型を昇格**させてマッチするメソッドを探します。

```java
public static void show(double d) {
    System.out.println("double: " + d);
}

show(10);      // int → double に昇格 → "double: 10.0"
show(3.14f);   // float → double に昇格 → "double: 3.14..."
```

> ⚠️ 複数のメソッドが「あいまい」に一致する場合はコンパイルエラーになります。

---

### オーバーロードのメリット

メソッド名を統一することで、利用者がAPIを直感的に使えます。

```java
// オーバーロードなし（名前が煩雑）
printInt(42);
printDouble(3.14);
printString("hello");

// オーバーロードあり（名前が統一されてシンプル）
print(42);
print(3.14);
print("hello");
```

---

### コード例：format メソッドのオーバーロード

```java
public class Formatter {

    // 整数を指定桁数で右寄せ
    public static String format(int n, int width) {
        String s = String.valueOf(n);
        while (s.length() < width) {
            s = " " + s;
        }
        return s;
    }

    // 文字列を指定桁数で右寄せ
    public static String format(String s, int width) {
        while (s.length() < width) {
            s = " " + s;
        }
        return s;
    }

    // 小数を指定の小数点以下桁数で整形
    public static String format(double d, int decimals) {
        return String.format("%." + decimals + "f", d);
    }

    public static void main(String[] args) {
        System.out.println("|" + format(42,    6) + "|");  // |    42|
        System.out.println("|" + format("Hi",  6) + "|");  // |    Hi|
        System.out.println("|" + format(3.14159, 3) + "|"); // |3.142|
    }
}
```

**出力結果：**
```
|    42|
|    Hi|
|3.142|
```

---

## ✏️ 問題

### 問題 6-3-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {

    public static String describe(int n) {
        return "整数: " + n;
    }

    public static String describe(double d) {
        return "小数: " + d;
    }

    public static String describe(String s) {
        return "文字列: " + s;
    }

    public static String describe(int a, int b) {
        return "2つの整数: " + a + " と " + b;
    }

    public static void main(String[] args) {
        System.out.println(describe(10));
        System.out.println(describe(3.14));
        System.out.println(describe("Java"));
        System.out.println(describe(3, 7));
    }
}
```

<details>
<summary>答えを見る</summary>

```
整数: 10
小数: 3.14
文字列: Java
2つの整数: 3 と 7
```

**解説：**
- `describe(10)` → `int` 型に一致 → `"整数: 10"`。
- `describe(3.14)` → `double` 型に一致 → `"小数: 3.14"`。
- `describe("Java")` → `String` 型に一致 → `"文字列: Java"`。
- `describe(3, 7)` → `(int, int)` の2引数に一致 → `"2つの整数: 3 と 7"`。

</details>

---

### 問題 6-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {

    public static int max(int a, int b) {
        return a > b ? a : b;
    }

    public static int max(int a, int b, int c) {
        return max(max(a, b), c);
    }

    public static double max(double a, double b) {
        return a > b ? a : b;
    }

    public static void main(String[] args) {
        System.out.println(max(3, 7));
        System.out.println(max(3, 7, 5));
        System.out.println(max(1, 9, 4));
        System.out.println(max(2.5, 1.8));
        System.out.println(max(10, 3));
    }
}
```

<details>
<summary>答えを見る</summary>

```
7
7
9
2.5
10
```

**解説：**
- `max(3, 7)` → `int` 2引数 → `7`。
- `max(3, 7, 5)` → `int` 3引数 → `max(max(3,7), 5)` = `max(7, 5)` = `7`。
- `max(1, 9, 4)` → `max(max(1,9), 4)` = `max(9, 4)` = `9`。
- `max(2.5, 1.8)` → `double` 2引数 → `2.5`。
- `max(10, 3)` → `int` 2引数 → `10`。

</details>

---

### 問題 6-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `Calculator` クラスを作成してください。

**条件：`sum` メソッドを以下の4パターンでオーバーロードする**

| シグネチャ | 処理内容 |
|---|---|
| `sum(int a, int b)` | `a + b` を返す |
| `sum(int a, int b, int c)` | `a + b + c` を返す |
| `sum(double a, double b)` | `a + b` を返す |
| `sum(int[] arr)` | 配列の全要素の合計を返す |

**main メソッドで以下を出力する：**

```
sum(3, 4): 7
sum(3, 4, 5): 12
sum(1.5, 2.3): 3.8
sum({1,2,3,4,5}): 15
sum(3, 4) + sum(1.5, 2.3): 10.8
```

> 💡 ヒント：最後の行は `sum(int, int)` の戻り値（int）と `sum(double, double)` の戻り値（double）を加算します。

<details>
<summary>答えを見る</summary>

```java
public class Calculator {

    public static int sum(int a, int b) {
        return a + b;
    }

    public static int sum(int a, int b, int c) {
        return a + b + c;
    }

    public static double sum(double a, double b) {
        return a + b;
    }

    public static int sum(int[] arr) {
        int total = 0;
        for (int n : arr) {
            total += n;
        }
        return total;
    }

    public static void main(String[] args) {
        System.out.println("sum(3, 4): "       + sum(3, 4));
        System.out.println("sum(3, 4, 5): "    + sum(3, 4, 5));
        System.out.println("sum(1.5, 2.3): "   + sum(1.5, 2.3));
        System.out.println("sum({1,2,3,4,5}): " + sum(new int[]{1, 2, 3, 4, 5}));
        System.out.println("sum(3, 4) + sum(1.5, 2.3): "
            + (sum(3, 4) + sum(1.5, 2.3)));
    }
}
```

**解説：**
- `sum(int, int)` と `sum(double, double)` は引数の型が違うためオーバーロード成立。
- `sum(int, int, int)` は引数の個数が違うためオーバーロード成立。
- `sum(int[])` は引数の型が配列のためオーバーロード成立。
- 最後の行は `sum(3, 4)` が `int` の `7` を返し、`sum(1.5, 2.3)` が `double` の `3.8` を返します。`int + double` の演算結果は `double` なので `10.8` になります。

</details>
