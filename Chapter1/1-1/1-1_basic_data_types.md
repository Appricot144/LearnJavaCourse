# 第1章：変数とデータ型

## 1-1. 基本データ型の宣言と代入

---

## 📖 解説

### 変数とは？

変数とは、**データを一時的に保存するための「箱」**です。  
Javaでは変数を使う前に、必ず「どんな種類のデータを入れるか」を宣言する必要があります。

```
データ型　変数名　=　値;
  ↓        ↓       ↓
 int      score  =  100;
```

---

### Javaの基本データ型（プリミティブ型）一覧

| データ型 | サイズ | 内容 | 例 |
|---|---|---|---|
| `byte` | 8bit | 整数（-128 〜 127） | `byte b = 10;` |
| `short` | 16bit | 整数（-32,768 〜 32,767） | `short s = 1000;` |
| `int` | 32bit | 整数（約±21億） | `int n = 42;` |
| `long` | 64bit | 大きな整数 | `long l = 100L;` |
| `float` | 32bit | 小数（精度低） | `float f = 3.14f;` |
| `double` | 64bit | 小数（精度高） | `double d = 3.14;` |
| `boolean` | 1bit | 真偽値 | `boolean flag = true;` |
| `char` | 16bit | 1文字 | `char c = 'A';` |

> ⚠️ **注意点**
> - `long` の値には末尾に `L` をつける（例：`100L`）
> - `float` の値には末尾に `f` をつける（例：`3.14f`）
> - `char` はシングルクォート `' '` で囲む

---

### コード例

```java
public class DataTypes {
    public static void main(String[] args) {

        // 整数型
        int age = 25;
        long population = 8000000000L;

        // 小数型
        double height = 175.5;
        float weight = 68.3f;

        // 真偽値
        boolean isStudent = true;

        // 文字型
        char grade = 'A';

        System.out.println("年齢: " + age);
        System.out.println("身長: " + height);
        System.out.println("学生か: " + isStudent);
        System.out.println("成績: " + grade);
    }
}
```

**出力結果：**
```
年齢: 25
身長: 175.5
学生か: true
成績: A
```

---

### 変数の宣言と代入のパターン

```java
// パターン1：宣言と代入を同時に行う
int score = 100;

// パターン2：宣言だけ先に行い、後で代入する
int score;
score = 100;

// パターン3：複数の変数を同時に宣言する
int x = 10, y = 20, z = 30;
```

---

## ✏️ 問題

### 問題 1-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;
        double c = 3.14;
        boolean flag = false;
        char ch = 'Z';

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
        System.out.println(flag);
        System.out.println(ch);
    }
}
```

<details>
<summary>答えを見る</summary>

```
10
3
3.14
false
Z
```

**解説：**  
各変数の型と値がそのまま表示されます。  
`boolean` は `true` / `false`、`char` はシングルクォートなしで1文字だけ表示されます。

</details>

---

### 問題 1-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        long bigNum = 123456789L;
        float pi = 3.14f;
        double precise = 3.141592653589793;
        byte small = 127;

        System.out.println(bigNum);
        System.out.println(pi);
        System.out.println(precise);
        System.out.println(small);
    }
}
```

<details>
<summary>答えを見る</summary>

```
123456789
3.14
3.141592653589793
127
```

**解説：**  
- `long` は `L` サフィックスをつけて宣言しますが、出力では `L` は表示されません。  
- `float` は精度が低いため、`3.14f` は `3.14` と表示されます。  
- `double` はより精度が高く、小数点以下の桁数が多く表示されます。  
- `byte` の最大値は `127` です。

</details>

---

### 問題 1-1-3（コード記述）　⭐⭐☆

以下の条件を満たす `MyProfile` クラスを作成してください。

**条件：**
- `int` 型の変数 `age` に自分の年齢（例：20）を代入する
- `double` 型の変数 `height` に身長（例：168.5）を代入する
- `boolean` 型の変数 `hasLicense` に運転免許の有無（例：false）を代入する
- `char` 型の変数 `bloodType` に血液型の頭文字（例：`'A'`）を代入する
- 4つの変数をすべて `System.out.println()` で出力する

**期待される出力例：**
```
20
168.5
false
A
```

<details>
<summary>答えを見る</summary>

```java
public class MyProfile {
    public static void main(String[] args) {
        int age = 20;
        double height = 168.5;
        boolean hasLicense = false;
        char bloodType = 'A';

        System.out.println(age);
        System.out.println(height);
        System.out.println(hasLicense);
        System.out.println(bloodType);
    }
}
```

**解説：**  
- 各データ型に合った値を代入しています。
- `char` はシングルクォート `'A'` で1文字を表します。
- 値は入力した内容に応じて変わりますが、型と形式が合っていれば正解です。

</details>
