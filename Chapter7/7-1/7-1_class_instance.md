# 第7章：クラスとオブジェクト

## 7-1. クラスの定義・インスタンス生成

---

## 📖 解説

### クラスとオブジェクトとは？

**クラス**とは、オブジェクトの**設計図（テンプレート）**です。  
**オブジェクト（インスタンス）**とは、クラスをもとに実際に作られた**実体**です。

```
クラス（設計図）         オブジェクト（実体）
  Dog クラス    →  new →  pochi（Dog）
                →  new →  hana（Dog）
                →  new →  kuro（Dog）
```

1つのクラスから複数のオブジェクトを生成できます。

---

### クラスの基本構造

```java
public class Dog {
    // フィールド（属性）
    String name;
    int    age;
    String breed;

    // メソッド（振る舞い）
    public void bark() {
        System.out.println(name + ": ワン！");
    }

    public void info() {
        System.out.println(name + "（" + age + "歳・" + breed + "）");
    }
}
```

| 要素 | 説明 |
|---|---|
| **フィールド** | オブジェクトが持つデータ（属性） |
| **メソッド** | オブジェクトの動作（振る舞い） |

---

### インスタンスの生成

`new` キーワードを使ってインスタンスを生成します。

```java
クラス名 変数名 = new クラス名();
```

```java
Dog pochi = new Dog();  // Dog のインスタンスを生成
Dog hana  = new Dog();  // もう1つ別のインスタンスを生成
```

---

### フィールドへのアクセス

`オブジェクト名.フィールド名` でフィールドにアクセスします。

```java
Dog pochi = new Dog();

// フィールドに値を設定
pochi.name  = "ポチ";
pochi.age   = 3;
pochi.breed = "柴犬";

// フィールドの値を参照
System.out.println(pochi.name);   // ポチ
System.out.println(pochi.age);    // 3
```

---

### メソッドの呼び出し

`オブジェクト名.メソッド名()` でメソッドを呼び出します。

```java
pochi.bark();  // → ポチ: ワン！
pochi.info();  // → ポチ（3歳・柴犬）
```

---

### 複数のインスタンスは独立している

各インスタンスはそれぞれ独自のフィールドを持ちます。

```java
Dog pochi = new Dog();
pochi.name = "ポチ";
pochi.age  = 3;

Dog hana = new Dog();
hana.name = "ハナ";
hana.age  = 5;

pochi.bark();  // ポチ: ワン！
hana.bark();   // ハナ: ワン！

System.out.println(pochi.age);  // 3
System.out.println(hana.age);   // 5  ← 独立している
```

---

### null と NullPointerException

変数を宣言しただけでインスタンスを生成していない場合は `null` になります。  
`null` に対してメソッドやフィールドにアクセスすると `NullPointerException` が発生します。

```java
Dog dog = null;
dog.bark();  // ❌ NullPointerException！

// 必ず new でインスタンスを生成してから使う
Dog dog2 = new Dog();
dog2.bark();  // ✅ OK
```

---

### コード例

```java
public class Car {
    String maker;
    String model;
    int    year;
    int    speed;

    public void accelerate(int amount) {
        speed += amount;
        System.out.println(model + " が加速: " + speed + "km/h");
    }

    public void brake(int amount) {
        speed = Math.max(0, speed - amount);
        System.out.println(model + " がブレーキ: " + speed + "km/h");
    }

    public void showInfo() {
        System.out.println(year + "年式 " + maker + " " + model);
    }
}

public class Main {
    public static void main(String[] args) {
        Car car1 = new Car();
        car1.maker = "Toyota";
        car1.model = "Prius";
        car1.year  = 2022;
        car1.speed = 0;

        car1.showInfo();
        car1.accelerate(60);
        car1.accelerate(40);
        car1.brake(30);
    }
}
```

**出力結果：**
```
2022年式 Toyota Prius
Prius が加速: 60km/h
Prius が加速: 100km/h
Prius がブレーキ: 70km/h
```

---

## ✏️ 問題

### 問題 7-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Point {
    int x;
    int y;

    public void show() {
        System.out.println("(" + x + ", " + y + ")");
    }

    public void move(int dx, int dy) {
        x += dx;
        y += dy;
    }
}

public class Q1 {
    public static void main(String[] args) {
        Point p = new Point();
        p.x = 3;
        p.y = 5;
        p.show();

        p.move(2, -1);
        p.show();

        Point q = new Point();
        q.show();  // デフォルト値
    }
}
```

<details>
<summary>答えを見る</summary>

```
(3, 5)
(5, 4)
(0, 0)
```

**解説：**
- `p` は `(3, 5)` に設定後、`move(2, -1)` で `x += 2` → `5`、`y -= 1` → `4`。
- `q` は `new Point()` で生成したままフィールドに値を設定していないため、`int` のデフォルト値 `0` になります。

</details>

---

### 問題 7-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Counter {
    int count;

    public void increment() {
        count++;
    }

    public void decrement() {
        count--;
    }

    public void reset() {
        count = 0;
    }

    public void show() {
        System.out.println("count: " + count);
    }
}

public class Q2 {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();

        c1.increment();
        c1.increment();
        c1.increment();
        c2.increment();

        c1.show();
        c2.show();

        c1.decrement();
        c1.show();

        c1.reset();
        c1.show();
    }
}
```

<details>
<summary>答えを見る</summary>

```
count: 3
count: 1
count: 2
count: 0
```

**解説：**
- `c1` は `increment()` を3回呼んで `count = 3`。
- `c2` は `increment()` を1回呼んで `count = 1`（`c1` とは独立）。
- `c1.decrement()` で `c1.count = 2`。
- `c1.reset()` で `c1.count = 0`。

</details>

---

### 問題 7-1-3（コード記述）　⭐⭐⭐

次の条件を満たす `Rectangle`（長方形）クラスと `Main` クラスを作成してください。

**Rectangle クラスの条件：**

| 要素 | 内容 |
|---|---|
| フィールド | `double width`（幅）、`double height`（高さ） |
| `area()` | 面積（width × height）を返す |
| `perimeter()` | 周囲の長さ（(width + height) × 2）を返す |
| `isSquare()` | 正方形かどうか（width == height）を返す |
| `show()` | `"幅: X, 高さ: Y"` の形式で出力 |

**Main クラスで以下を出力：**

```
幅: 4.0, 高さ: 6.0
面積: 24.0
周囲: 20.0
正方形: false
---
幅: 5.0, 高さ: 5.0
面積: 25.0
周囲: 20.0
正方形: true
```

<details>
<summary>答えを見る</summary>

```java
public class Rectangle {
    double width;
    double height;

    public double area() {
        return width * height;
    }

    public double perimeter() {
        return (width + height) * 2;
    }

    public boolean isSquare() {
        return width == height;
    }

    public void show() {
        System.out.println("幅: " + width + ", 高さ: " + height);
    }
}

public class Main {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle();
        r1.width  = 4.0;
        r1.height = 6.0;
        r1.show();
        System.out.println("面積: "   + r1.area());
        System.out.println("周囲: "   + r1.perimeter());
        System.out.println("正方形: " + r1.isSquare());

        System.out.println("---");

        Rectangle r2 = new Rectangle();
        r2.width  = 5.0;
        r2.height = 5.0;
        r2.show();
        System.out.println("面積: "   + r2.area());
        System.out.println("周囲: "   + r2.perimeter());
        System.out.println("正方形: " + r2.isSquare());
    }
}
```

**解説：**
- `Rectangle` クラスにフィールド `width`・`height` と4つのメソッドを定義します。
- `r1` と `r2` はそれぞれ独立したインスタンスで、フィールドの値が異なります。
- `isSquare()` は `width == height` で正方形かどうかを判定します。

</details>

### 問題 7-1-4（コード記述）　⭐⭐⭐

次の条件を満たす `BankAccount`（銀行口座）クラスと `Main` クラスを作成してください。

**BankAccount クラスの条件：**

| 要素 | 内容 |
|---|---|
| フィールド | `String owner`（名義人）、`int balance`（残高） |
| `deposit(int amount)` | 残高に `amount` を加算し `"入金: X円 残高: Y円"` を出力 |
| `withdraw(int amount)` | 残高が足りれば減算し `"出金: X円 残高: Y円"` を出力、足りなければ `"残高不足"` を出力 |
| `showBalance()` | `"[名義人] 残高: X円"` の形式で出力 |

**期待される出力：**
```
[田中太郎] 残高: 0円
入金: 50000円 残高: 50000円
入金: 30000円 残高: 80000円
出金: 20000円 残高: 60000円
残高不足
[田中太郎] 残高: 60000円
```

<details>
<summary>答えを見る</summary>

```java
public class BankAccount {
    String owner;
    int    balance;

    public void deposit(int amount) {
        balance += amount;
        System.out.println("入金: " + amount + "円 残高: " + balance + "円");
    }

    public void withdraw(int amount) {
        if (amount > balance) {
            System.out.println("残高不足");
        } else {
            balance -= amount;
            System.out.println("出金: " + amount + "円 残高: " + balance + "円");
        }
    }

    public void showBalance() {
        System.out.println("[" + owner + "] 残高: " + balance + "円");
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.owner   = "田中太郎";
        account.balance = 0;

        account.showBalance();
        account.deposit(50000);
        account.deposit(30000);
        account.withdraw(20000);
        account.withdraw(100000);  // 残高不足
        account.showBalance();
    }
}
```

**解説：**
- `deposit` は無条件で残高を増やします。
- `withdraw` は `amount > balance` のとき `"残高不足"` を出力して処理をスキップします。
- フィールドの `balance` はオブジェクト内で状態を保持し続けます。

</details>
