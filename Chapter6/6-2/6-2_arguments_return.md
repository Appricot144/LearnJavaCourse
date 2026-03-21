# 第6章：メソッド

## 6-2. 引数と戻り値

---

## 📖 解説

### 引数とは？

引数とは、**メソッドに渡す値**のことです。  
メソッドを呼び出すときに値を渡すことで、同じメソッドでも異なる動作をさせられます。

```java
// 引数なし → 常に同じ動作
public static void greet() {
    System.out.println("こんにちは、ゲスト！");
}

// 引数あり → 渡した値によって動作が変わる
public static void greet(String name) {
    System.out.println("こんにちは、" + name + "！");
}
```

---

### 引数の定義と呼び出し

```
public static void メソッド名(型 引数名, 型 引数名, ...) {
    // 処理
}
```

```java
// 定義
public static void showInfo(String name, int age) {
    System.out.println(name + "さん（" + age + "歳）");
}

// 呼び出し（順番・型を合わせる）
showInfo("田中", 25);   // → 田中さん（25歳）
showInfo("鈴木", 30);   // → 鈴木さん（30歳）
```

> ⚠️ 引数の**順番・個数・型**はメソッド定義と完全に一致させる必要があります。

---

### 戻り値とは？

戻り値とは、**メソッドが呼び出し元に返す値**のことです。  
戻り値の型をメソッドの戻り値型として宣言し、`return` で返します。

```
public static 戻り値の型 メソッド名(引数) {
    // 処理
    return 返す値;
}
```

```java
public static int add(int a, int b) {
    return a + b;  // 計算結果を返す
}

// 呼び出し側で受け取る
int result = add(3, 5);
System.out.println(result);  // 8
```

---

### 引数・戻り値の組み合わせパターン

| パターン | 例 |
|---|---|
| 引数なし・戻り値なし | `void greet()` |
| 引数あり・戻り値なし | `void print(String msg)` |
| 引数なし・戻り値あり | `int getYear()` |
| 引数あり・戻り値あり | `int add(int a, int b)` |

---

### 値渡し（プリミティブ型）

Javaはプリミティブ型の引数を**値渡し**します。  
メソッド内で引数を変更しても、呼び出し元の変数は変わりません。

```java
public static void doubleValue(int x) {
    x = x * 2;
    System.out.println("メソッド内: " + x);  // 20
}

public static void main(String[] args) {
    int n = 10;
    doubleValue(n);
    System.out.println("呼び出し後: " + n);  // 10 ← 変わらない！
}
```

---

### 複数の return 文

条件によって異なる値を返すことができます。

```java
public static String getGrade(int score) {
    if (score >= 90) return "S";
    if (score >= 80) return "A";
    if (score >= 70) return "B";
    if (score >= 60) return "C";
    return "F";
}
```

> 💡 どのパスを通っても必ず `return` されるようにしましょう。

---

### コード例

```java
public class MethodCalc {

    // 引数あり・戻り値あり
    public static double calcBMI(double weight, double height) {
        return weight / (height * height);
    }

    // 引数あり・戻り値あり（String）
    public static String judgeBMI(double bmi) {
        if (bmi < 18.5) return "低体重";
        if (bmi < 25.0) return "普通体重";
        if (bmi < 30.0) return "肥満（1度）";
        return "肥満（2度以上）";
    }

    public static void main(String[] args) {
        double bmi = calcBMI(65.0, 1.70);
        System.out.printf("BMI: %.2f%n", bmi);
        System.out.println("判定: " + judgeBMI(bmi));
    }
}
```

**出力結果：**
```
BMI: 22.49
判定: 普通体重
```

---

## ✏️ 問題

### 問題 6-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {

    public static int multiply(int a, int b) {
        return a * b;
    }

    public static String repeat(String s, int n) {
        String result = "";
        for (int i = 0; i < n; i++) {
            result += s;
        }
        return result;
    }

    public static void main(String[] args) {
        int x = multiply(4, 7);
        System.out.println(x);

        System.out.println(multiply(3, multiply(2, 5)));

        String s = repeat("ab", 3);
        System.out.println(s);
    }
}
```

<details>
<summary>答えを見る</summary>

```
28
30
ababab
```

**解説：**
- `multiply(4, 7)` → `28`。
- `multiply(2, 5)` → `10`、`multiply(3, 10)` → `30`（メソッドのネスト呼び出し）。
- `repeat("ab", 3)` → `"ab"` を3回繰り返して `"ababab"`。

</details>

---

### 問題 6-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {

    public static int clamp(int value, int min, int max) {
        if (value < min) return min;
        if (value > max) return max;
        return value;
    }

    public static boolean isEven(int n) {
        return n % 2 == 0;
    }

    public static void tryDouble(int x) {
        x = x * 2;
        System.out.println("内部: " + x);
    }

    public static void main(String[] args) {
        System.out.println(clamp(5,  1, 10));
        System.out.println(clamp(-3, 1, 10));
        System.out.println(clamp(15, 1, 10));

        System.out.println(isEven(4));
        System.out.println(isEven(7));

        int n = 20;
        tryDouble(n);
        System.out.println("外部: " + n);
    }
}
```

<details>
<summary>答えを見る</summary>

```
5
1
10
true
false
内部: 40
外部: 20
```

**解説：**
- `clamp(5, 1, 10)` → `5`（範囲内なのでそのまま）。
- `clamp(-3, 1, 10)` → `1`（min より小さいので min を返す）。
- `clamp(15, 1, 10)` → `10`（max より大きいので max を返す）。
- `isEven(4)` → `true`、`isEven(7)` → `false`。
- `tryDouble(n)` はプリミティブの値渡しのため、メソッド内で変更しても呼び出し元の `n` は `20` のまま。

</details>

---

### 問題 6-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `StringUtils` クラスを作成してください。

**条件：以下の4つのメソッドを定義する**

| メソッド | 引数 | 戻り値 | 処理内容 |
|---|---|---|---|
| `countChar` | `String s, char c` | `int` | 文字列 `s` 中に文字 `c` が何個含まれるか数えて返す |
| `isPalindrome` | `String s` | `boolean` | 文字列 `s` が回文かどうかを返す |
| `capitalize` | `String s` | `String` | 先頭だけ大文字・残りを小文字にして返す |
| `repeat` | `String s, int n` | `String` | 文字列 `s` を `n` 回繰り返した文字列を返す |

**main メソッドで以下を出力する：**

```
'l' の数: 3
"racecar" は回文: true
"hello" は回文: false
capitalize("jAVA"): Java
repeat("ha", 4): hahahaha
```

<details>
<summary>答えを見る</summary>

```java
public class StringUtils {

    public static int countChar(String s, char c) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == c) count++;
        }
        return count;
    }

    public static boolean isPalindrome(String s) {
        String lower = s.toLowerCase();
        for (int i = 0; i < lower.length() / 2; i++) {
            if (lower.charAt(i) != lower.charAt(lower.length() - 1 - i)) {
                return false;
            }
        }
        return true;
    }

    public static String capitalize(String s) {
        if (s == null || s.isEmpty()) return s;
        return s.substring(0, 1).toUpperCase() + s.substring(1).toLowerCase();
    }

    public static String repeat(String s, int n) {
        String result = "";
        for (int i = 0; i < n; i++) {
            result += s;
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println("'l' の数: "          + countChar("hello world", 'l'));
        System.out.println("\"racecar\" は回文: " + isPalindrome("racecar"));
        System.out.println("\"hello\" は回文: "   + isPalindrome("hello"));
        System.out.println("capitalize(\"jAVA\"): " + capitalize("jAVA"));
        System.out.println("repeat(\"ha\", 4): "    + repeat("ha", 4));
    }
}
```

**解説：**
- `countChar`：`charAt(i)` で1文字ずつ比較してカウントします。
- `isPalindrome`：前半を走査し、対称位置の文字と比較します（`s.length() / 2` まででOK）。
- `capitalize`：`substring(0,1).toUpperCase()` と `substring(1).toLowerCase()` を連結します。
- `repeat`：ループで文字列を連結して返します。

</details>
