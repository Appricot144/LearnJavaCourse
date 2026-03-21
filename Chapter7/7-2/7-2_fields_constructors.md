# 第7章：クラスとオブジェクト

## 7-2. フィールドとコンストラクタ

---

## 📖 解説

### コンストラクタとは？

コンストラクタとは、**インスタンス生成時に自動的に呼ばれる特別なメソッド**です。  
主にフィールドの初期値を設定するために使います。

```
特徴：
・クラス名と同じ名前
・戻り値の型を書かない（void も書かない）
・new クラス名() のタイミングで自動的に呼ばれる
```

---

### コンストラクタのない場合の問題点

コンストラクタがないと、インスタンス生成後に毎回フィールドを設定しなければなりません。

```java
// コンストラクタなし（不便）
Dog pochi = new Dog();
pochi.name  = "ポチ";   // 毎回設定が必要
pochi.age   = 3;
pochi.breed = "柴犬";

// コンストラクタあり（便利）
Dog pochi = new Dog("ポチ", 3, "柴犬");  // 生成と同時に設定
```

---

### コンストラクタの定義

```java
public class Dog {
    String name;
    int    age;
    String breed;

    // コンストラクタ
    public Dog(String name, int age, String breed) {
        this.name  = name;   // this.フィールド名 = 引数
        this.age   = age;
        this.breed = breed;
    }

    public void bark() {
        System.out.println(name + ": ワン！");
    }
}
```

---

### this キーワード（コンストラクタ内）

`this` はそのクラスの**インスタンス自身**を指します。  
引数名とフィールド名が同じ場合に区別するために使います。

```java
public Dog(String name, int age) {
    this.name = name;  // this.name → フィールド
                       //      name → 引数
    this.age  = age;
}
```

> ⚠️ `this` をつけないと、引数が自分自身に代入されるだけで意味がありません。

---

### デフォルトコンストラクタ

コンストラクタを1つも定義しない場合、Javaは引数なしの**デフォルトコンストラクタ**を自動生成します。  
ただし、引数ありのコンストラクタを定義すると自動生成されなくなります。

```java
// コンストラクタを定義しない場合 → Javaが自動生成
Dog dog = new Dog();   // OK（デフォルトコンストラクタが使われる）

// 引数ありコンストラクタだけを定義した場合
public Dog(String name) { ... }

Dog dog = new Dog();          // ❌ コンパイルエラー！
Dog dog = new Dog("ポチ");    // ✅ OK
```

---

### コンストラクタのオーバーロード

複数のコンストラクタを定義できます（引数の型・個数が異なること）。

```java
public class Person {
    String name;
    int    age;
    String email;

    // コンストラクタ1：名前のみ
    public Person(String name) {
        this.name  = name;
        this.age   = 0;
        this.email = "";
    }

    // コンストラクタ2：名前と年齢
    public Person(String name, int age) {
        this.name  = name;
        this.age   = age;
        this.email = "";
    }

    // コンストラクタ3：全フィールド
    public Person(String name, int age, String email) {
        this.name  = name;
        this.age   = age;
        this.email = email;
    }
}
```

---

### static フィールド（クラス変数）

`static` を付けたフィールドは**クラス全体で共有**されます。  
インスタンスではなくクラスに属するため「クラス変数」とも呼ばれます。

```java
public class Counter {
    static int total = 0;  // クラス変数（全インスタンスで共有）
    int id;                // インスタンス変数（各インスタンスが持つ）

    public Counter() {
        total++;
        id = total;
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.total);  // 3（クラス名でアクセス）
System.out.println(c1.id);          // 1
System.out.println(c2.id);          // 2
```

---

### コード例

```java
public class Student {
    String name;
    int    grade;
    double gpa;

    public Student(String name, int grade, double gpa) {
        this.name  = name;
        this.grade = grade;
        this.gpa   = gpa;
    }

    public Student(String name) {
        this(name, 1, 0.0);  // 別のコンストラクタを呼ぶ
    }

    public void showProfile() {
        System.out.println(name + "（" + grade + "年・GPA:" + gpa + "）");
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("田中", 3, 3.8);
        Student s2 = new Student("山田");  // 1年・GPA:0.0

        s1.showProfile();  // 田中（3年・GPA:3.8）
        s2.showProfile();  // 山田（1年・GPA:0.0）
    }
}
```

**出力結果：**
```
田中（3年・GPA:3.8）
山田（1年・GPA:0.0）
```

---

## ✏️ 問題

### 問題 7-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Book {
    String title;
    String author;
    int    price;

    public Book(String title, String author, int price) {
        this.title  = title;
        this.author = author;
        this.price  = price;
    }

    public void show() {
        System.out.println("「" + title + "」" + author + " (" + price + "円)");
    }
}

public class Q1 {
    public static void main(String[] args) {
        Book b1 = new Book("Javaの基本", "山田太郎", 2800);
        Book b2 = new Book("デザインパターン", "鈴木花子", 3500);

        b1.show();
        b2.show();
        System.out.println(b1.title.equals(b2.title));
    }
}
```

<details>
<summary>答えを見る</summary>

```
「Javaの基本」山田太郎 (2800円)
「デザインパターン」鈴木花子 (3500円)
false
```

**解説：**
- コンストラクタで各フィールドが初期化されます。
- `b1.title.equals(b2.title)` は `"Javaの基本".equals("デザインパターン")` → `false`。

</details>

---

### 問題 7-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Temperature {
    double celsius;

    public Temperature(double celsius) {
        this.celsius = celsius;
    }

    public Temperature() {
        this(0.0);  // デフォルトは0度
    }

    public double toFahrenheit() {
        return celsius * 9 / 5 + 32;
    }

    public void show() {
        System.out.printf("%.1f℃ = %.1f℉%n", celsius, toFahrenheit());
    }
}

public class Q2 {
    public static void main(String[] args) {
        Temperature t1 = new Temperature(100.0);
        Temperature t2 = new Temperature(0.0);
        Temperature t3 = new Temperature();

        t1.show();
        t2.show();
        t3.show();

        System.out.println(t1.celsius > t2.celsius);
    }
}
```

<details>
<summary>答えを見る</summary>

```
100.0℃ = 212.0℉
0.0℃ = 32.0℉
0.0℃ = 32.0℉
true
```

**解説：**
- `t1`：100℃ → 100 × 9/5 + 32 = 212℉。
- `t2`：0℃ → 0 × 9/5 + 32 = 32℉。
- `t3`：引数なしコンストラクタが `this(0.0)` で `Temperature(0.0)` を呼ぶため `t2` と同じ。
- `t1.celsius > t2.celsius` は `100.0 > 0.0` → `true`。

</details>

---

### 問題 7-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `Product`（商品）クラスと `Main` クラスを作成してください。

**Product クラスの条件：**

| 要素 | 内容 |
|---|---|
| フィールド | `String name`、`int price`、`int stock` |
| `static int totalProducts` | 生成したインスタンスの総数（クラス変数） |
| コンストラクタ1 | `(String name, int price, int stock)` で全フィールドを初期化。`totalProducts` をインクリメント |
| コンストラクタ2 | `(String name, int price)` で `stock = 0` としてコンストラクタ1を呼ぶ |
| `sell(int amount)` | 在庫が足りれば `stock` を減らして `"X個販売"` を出力、足りなければ `"在庫不足"` を出力 |
| `show()` | `"[name] 価格:X円 在庫:Y個"` の形式で出力 |

**期待される出力：**
```
商品数: 3
[りんご] 価格:150円 在庫:100個
[バナナ] 価格:200円 在庫:50個
[メロン] 価格:800円 在庫:0個
5個販売
在庫不足
[りんご] 価格:150円 在庫:95個
```

<details>
<summary>答えを見る</summary>

```java
class Product {
    String name;
    int    price;
    int    stock;
    static int totalProducts = 0;

    public Product(String name, int price, int stock) {
        this.name  = name;
        this.price = price;
        this.stock = stock;
        totalProducts++;
    }

    public Product(String name, int price) {
        this(name, price, 0);
    }

    public void sell(int amount) {
        if (amount > stock) {
            System.out.println("在庫不足");
        } else {
            stock -= amount;
            System.out.println(amount + "個販売");
        }
    }

    public void show() {
        System.out.println("[" + name + "] 価格:" + price
            + "円 在庫:" + stock + "個");
    }
}

public class Main {
    public static void main(String[] args) {
        Product p1 = new Product("りんご", 150, 100);
        Product p2 = new Product("バナナ", 200, 50);
        Product p3 = new Product("メロン", 800);  // stock = 0

        System.out.println("商品数: " + Product.totalProducts);
        p1.show();
        p2.show();
        p3.show();

        p1.sell(5);
        p1.sell(200);  // 在庫不足
        p1.show();
    }
}
```

**解説：**
- `totalProducts` は `static` フィールドなので全インスタンスで共有され、コンストラクタが呼ばれるたびに増加します。
- コンストラクタ2は `this(name, price, 0)` でコンストラクタ1を呼び出し、処理を再利用しています。
- `sell()` は在庫チェック後に `stock` を更新します。

</details>
