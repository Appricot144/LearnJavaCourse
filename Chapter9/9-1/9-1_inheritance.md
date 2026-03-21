# 第9章：継承・インターフェース・Enum

## 9-1. 継承（extends / override）

---

## 📖 解説

### 継承とは？

継承とは、**既存のクラス（親クラス）の機能を引き継いで、新しいクラス（子クラス）を作る**仕組みです。  
共通の機能を親クラスにまとめ、差分だけ子クラスに書くことでコードの重複を減らせます。

```
親クラス（スーパークラス）
    ↓ extends
子クラス（サブクラス）← 親の機能をすべて引き継ぐ + 独自の機能を追加
```

---

### extends による継承

```java
// 親クラス
public class Animal {
    String name;
    int    age;

    public Animal(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    public void eat() {
        System.out.println(name + " が食事する");
    }

    public void sleep() {
        System.out.println(name + " が眠る");
    }
}

// 子クラス
public class Dog extends Animal {
    String breed;

    public Dog(String name, int age, String breed) {
        super(name, age);  // 親のコンストラクタを呼ぶ
        this.breed = breed;
    }

    public void bark() {
        System.out.println(name + " が吠える：ワン！");
    }
}
```

---

### super キーワード

`super` は親クラスを指すキーワードです。2つの使い方があります。

```java
// 使い方1：親のコンストラクタを呼ぶ（必ず子コンストラクタの先頭行）
public Dog(String name, int age, String breed) {
    super(name, age);  // Animal(String, int) を呼ぶ
    this.breed = breed;
}

// 使い方2：親のメソッドを呼ぶ
@Override
public void eat() {
    super.eat();  // 親の eat() を呼ぶ
    System.out.println("（骨つきで）");
}
```

---

### メソッドのオーバーライド

子クラスで親クラスのメソッドを再定義します。  
`@Override` を付けることでミスをコンパイラが検出してくれます。

```java
public class Cat extends Animal {
    public Cat(String name, int age) {
        super(name, age);
    }

    @Override
    public void eat() {
        System.out.println(name + " が魚を食べる");  // 独自の実装
    }

    public void purr() {
        System.out.println(name + " がゴロゴロ言う");
    }
}
```

---

### 継承の連鎖

継承は複数段階で行えます。

```
Animal
  └─ Dog
       └─ GuideDog（盲導犬）
```

```java
public class GuideDog extends Dog {
    String owner;

    public GuideDog(String name, int age, String breed, String owner) {
        super(name, age, breed);  // Dog のコンストラクタ
        this.owner = owner;
    }

    public void guide() {
        System.out.println(name + " が " + owner + " を案内する");
    }
}
```

---

### 継承とアクセス修飾子

| 修飾子 | 同じクラス | サブクラス | 他のクラス |
|---|---|---|---|
| `private` | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ |

> 💡 親クラスの `private` フィールドは子クラスから直接アクセスできません。  
> `protected` または getter/setter を使います。

---

### final クラス・final メソッド

- `final` クラス：継承不可（`String` クラスなど）
- `final` メソッド：オーバーライド不可

```java
public final class Singleton { ... }     // 継承不可
public final void process() { ... }      // オーバーライド不可
```

---

### コード例

```java
public class Vehicle {
    protected String maker;
    protected int    speed;

    public Vehicle(String maker) {
        this.maker = maker;
        this.speed = 0;
    }

    public void accelerate(int v) {
        speed += v;
        System.out.println(maker + " 速度: " + speed + "km/h");
    }

    public String getInfo() {
        return maker + "（" + speed + "km/h）";
    }
}

public class ElectricCar extends Vehicle {
    private int battery;

    public ElectricCar(String maker, int battery) {
        super(maker);
        this.battery = battery;
    }

    @Override
    public void accelerate(int v) {
        battery -= v / 10;
        super.accelerate(v);  // 親の処理も実行
        System.out.println("バッテリー残量: " + battery + "%");
    }

    @Override
    public String getInfo() {
        return super.getInfo() + " バッテリー:" + battery + "%";
    }
}

public class Main {
    public static void main(String[] args) {
        ElectricCar ec = new ElectricCar("Tesla", 100);
        ec.accelerate(60);
        ec.accelerate(40);
        System.out.println(ec.getInfo());
    }
}
```

**出力結果：**
```
Tesla 速度: 60km/h
バッテリー残量: 94%
Tesla 速度: 100km/h
バッテリー残量: 90%
Tesla（100km/h） バッテリー:90%
```

---

## ✏️ 問題

### 問題 9-1-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Person {
    String name;
    int    age;

    public Person(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    public String greet() {
        return "こんにちは、" + name + "（" + age + "歳）です";
    }

    public String toString() {
        return "Person[" + name + ", " + age + "]";
    }
}

class Student extends Person {
    String school;

    public Student(String name, int age, String school) {
        super(name, age);
        this.school = school;
    }

    @Override
    public String greet() {
        return super.greet() + "。" + school + " に通っています";
    }

    @Override
    public String toString() {
        return "Student[" + name + ", " + age + ", " + school + "]";
    }
}

class GradStudent extends Student {
    String lab;

    public GradStudent(String name, int age, String school, String lab) {
        super(name, age, school);
        this.lab = lab;
    }

    @Override
    public String greet() {
        return super.greet() + "（" + lab + " 所属）";
    }
}

public class Q1 {
    public static void main(String[] args) {
        Person   p = new Person("田中", 45);
        Student  s = new Student("山田", 20, "東大");
        GradStudent g = new GradStudent("鈴木", 24, "京大", "AI研究室");

        System.out.println(p.greet());
        System.out.println(s.greet());
        System.out.println(g.greet());

        System.out.println(p);
        System.out.println(s);
    }
}
```

<details>
<summary>答えを見る</summary>

```
こんにちは、田中（45歳）です
こんにちは、山田（20歳）です。東大 に通っています
こんにちは、鈴木（24歳）です。京大 に通っています（AI研究室 所属）
Person[田中, 45]
Student[山田, 20, 東大]
```

**解説：**
- `Student.greet()` は `super.greet()`（Person の実装）を呼び出してから追記します。
- `GradStudent.greet()` は `super.greet()`（Student の実装）を呼び出してから追記します。連鎖的に `Person → Student → GradStudent` の順で文字列が組み立てられます。
- `toString()` は `GradStudent` に定義されていないため、`Student` の実装が使われます。

</details>

---

### 問題 9-1-2（出力結果を答える）　⭐⭐⭐

次のコードを実行したとき、出力される結果を答えてください。

```java
class Shape {
    protected String color;

    public Shape(String color) { this.color = color; }

    public double area() { return 0.0; }

    public void describe() {
        System.out.println(getClass().getSimpleName()
            + "（" + color + "）面積: " + area());
    }
}

class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double area() { return Math.PI * radius * radius; }
}

class Triangle extends Shape {
    private double base, height;

    public Triangle(String color, double base, double height) {
        super(color);
        this.base   = base;
        this.height = height;
    }

    @Override
    public double area() { return base * height / 2; }
}

public class Q2 {
    public static void main(String[] args) {
        Shape[] shapes = {
            new Circle("赤", 3.0),
            new Triangle("青", 6.0, 4.0),
            new Circle("緑", 1.0)
        };

        double total = 0;
        for (Shape s : shapes) {
            s.describe();
            total += s.area();
        }
        System.out.printf("合計面積: %.2f%n", total);
        System.out.println(shapes[0] instanceof Shape);
        System.out.println(shapes[0] instanceof Circle);
    }
}
```

<details>
<summary>答えを見る</summary>

```
Circle（赤）面積: 28.274333882308138
Triangle（青）面積: 12.0
Circle（緑）面積: 3.141592653589793
合計面積: 43.42
true
true
```

**解説：**
- `describe()` は `Shape` に定義され、内部で `area()` を呼びます。動的ディスパッチにより各サブクラスの `area()` が実行されます。
- `getClass().getSimpleName()` で実際のクラス名（`Circle`、`Triangle`）を取得します。
- 合計面積：`28.274...` + `12.0` + `3.141...` = `43.42`（小数第2位で丸め）。
- `shapes[0]` は `Circle` インスタンスなので `instanceof Shape` も `instanceof Circle` も `true`。

</details>

---

### 問題 9-1-3（コード記述）　⭐⭐⭐

次の条件を満たすクラス群を作成してください。

**条件：**

- `Item`（商品）クラス：`String name`・`int price` フィールド。コンストラクタ `(name, price)`。`getPrice()` は `price` を返す。`toString()` は `"name（price円）"` を返す。

- `DiscountItem`（割引商品）クラス：`Item` を継承。`int discountRate`（割引率 %）フィールド追加。`getPrice()` を `price * (100 - discountRate) / 100` にオーバーライド。`toString()` を `"name（price円 → 割引後getPrice()円）"` にオーバーライド。

- `BundleItem`（セット商品）クラス：`Item` を継承。`int quantity`（数量）フィールド追加。`getPrice()` を `price * quantity` にオーバーライド。`toString()` を `"name × quantity（price円）合計: getPrice()円"` にオーバーライド。

**main で以下を出力する：**

```
りんご（150円）
バナナ（200円 → 割引後170円）
みかんセット × 5（100円）合計: 500円
---
合計金額: 820円
```

<details>
<summary>答えを見る</summary>

```java
class Item {
    String name;
    int    price;

    public Item(String name, int price) {
        this.name  = name;
        this.price = price;
    }

    public int getPrice() { return price; }

    @Override
    public String toString() {
        return name + "（" + price + "円）";
    }
}

class DiscountItem extends Item {
    int discountRate;

    public DiscountItem(String name, int price, int discountRate) {
        super(name, price);
        this.discountRate = discountRate;
    }

    @Override
    public int getPrice() {
        return price * (100 - discountRate) / 100;
    }

    @Override
    public String toString() {
        return name + "（" + price + "円 → 割引後" + getPrice() + "円）";
    }
}

class BundleItem extends Item {
    int quantity;

    public BundleItem(String name, int price, int quantity) {
        super(name, price);
        this.quantity = quantity;
    }

    @Override
    public int getPrice() {
        return price * quantity;
    }

    @Override
    public String toString() {
        return name + " × " + quantity
            + "（" + price + "円）合計: " + getPrice() + "円";
    }
}

public class Main {
    public static void main(String[] args) {
        Item[] cart = {
            new Item("りんご", 150),
            new DiscountItem("バナナ", 200, 15),
            new BundleItem("みかんセット", 100, 5)
        };

        int total = 0;
        for (Item item : cart) {
            System.out.println(item);
            total += item.getPrice();
        }
        System.out.println("---");
        System.out.println("合計金額: " + total + "円");
    }
}
```

**解説：**
- `DiscountItem.getPrice()`：`200 × (100 - 15) / 100 = 200 × 85 / 100 = 170`。
- `BundleItem.getPrice()`：`100 × 5 = 500`。
- 合計：`150 + 170 + 500 = 820`。
- `toString()` の中で `getPrice()` を呼ぶと動的ディスパッチにより各サブクラスの実装が使われます。

</details>

---

### 問題 9-1-4（コード記述）　⭐⭐⭐

次の条件を満たすクラス群を作成してください。

**条件：**

- `Account`（口座）クラス：`String id`・`String owner`・`protected int balance` フィールド。コンストラクタ `(id, owner, initialBalance)`。`deposit(int)`（入金）・`withdraw(int)` → 残高不足なら `false` を返す。`getBalance()`・`toString()` → `"[id] owner: balance円"` を返す。

- `SavingsAccount`（定期貯蓄口座）クラス：`Account` を継承。`double interestRate`（年利）フィールド追加。`applyInterest()` メソッドで `balance += balance * interestRate` を行い結果を出力。

**main で以下を出力する：**

```
[A001] 田中: 100000円
入金後: [A001] 田中: 150000円
利息適用: 利息7500円追加 → 残高157500円
[A001] 田中: 157500円
```

<details>
<summary>答えを見る</summary>

```java
class Account {
    String id;
    String owner;
    protected int balance;

    public Account(String id, String owner, int initialBalance) {
        this.id      = id;
        this.owner   = owner;
        this.balance = initialBalance;
    }

    public void deposit(int amount) {
        balance += amount;
    }

    public boolean withdraw(int amount) {
        if (amount > balance) return false;
        balance -= amount;
        return true;
    }

    public int getBalance() { return balance; }

    @Override
    public String toString() {
        return "[" + id + "] " + owner + ": " + balance + "円";
    }
}

class SavingsAccount extends Account {
    double interestRate;

    public SavingsAccount(String id, String owner,
                          int initialBalance, double interestRate) {
        super(id, owner, initialBalance);
        this.interestRate = interestRate;
    }

    public void applyInterest() {
        int interest = (int)(balance * interestRate);
        balance += interest;
        System.out.println("利息適用: 利息" + interest
            + "円追加 → 残高" + balance + "円");
    }
}

public class Main {
    public static void main(String[] args) {
        SavingsAccount sa = new SavingsAccount("A001", "田中", 100000, 0.05);
        System.out.println(sa);

        sa.deposit(50000);
        System.out.println("入金後: " + sa);

        sa.applyInterest();
        System.out.println(sa);
    }
}
```

**解説：**
- `SavingsAccount` は `Account` を継承し、`super(id, owner, initialBalance)` で親のコンストラクタを呼びます。
- `balance` は `protected` なので `SavingsAccount` から直接アクセスできます。
- 利息：`150000 × 0.05 = 7500`、残高：`150000 + 7500 = 157500`。

</details>
