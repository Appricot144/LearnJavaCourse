# 第8章：オブジェクト指向の概念

## 8-2. ポリモーフィズム

---

## 📖 解説

### ポリモーフィズムとは？

ポリモーフィズム（多態性）とは、**同じ操作（メソッド呼び出し）に対して、オブジェクトの種類によって異なる振る舞いをする**性質です。  
「多くの形を持つ」という意味のギリシャ語が語源です。

Javaでのポリモーフィズムは主に2種類あります。

| 種類 | 仕組み | 解決タイミング |
|---|---|---|
| **コンパイル時** | メソッドのオーバーロード | コンパイル時 |
| **実行時** | メソッドのオーバーライド＋継承/インターフェース | 実行時 |

---

### オーバーライドとは？

サブクラス（子クラス）が親クラスのメソッドを**再定義**することをオーバーライドといいます。  
`@Override` アノテーションをつけることで、意図的なオーバーライドであることを明示します。

```java
class Animal {
    public void speak() {
        System.out.println("...");
    }
}

class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("ワン！");  // 再定義
    }
}

class Cat extends Animal {
    @Override
    public void speak() {
        System.out.println("ニャー！");  // 再定義
    }
}
```

---

### 参照型の多態性

親クラス型の変数でサブクラスのインスタンスを参照できます。  
呼び出されるメソッドは**実際のオブジェクトの型**によって決まります（動的ディスパッチ）。

```java
Animal a1 = new Dog();  // Animal 型の変数に Dog を代入
Animal a2 = new Cat();

a1.speak();  // → ワン！（Dog の speak が呼ばれる）
a2.speak();  // → ニャー！（Cat の speak が呼ばれる）
```

---

### 配列とポリモーフィズム

親クラス型の配列に異なるサブクラスを混在させて一括処理できます。

```java
Animal[] animals = {
    new Dog(),
    new Cat(),
    new Dog(),
    new Cat()
};

for (Animal a : animals) {
    a.speak();  // それぞれの speak が呼ばれる
}
```

**出力結果：**
```
ワン！
ニャー！
ワン！
ニャー！
```

---

### instanceof 演算子

オブジェクトが特定のクラスのインスタンスかどうかを確認します。

```java
Animal a = new Dog();

System.out.println(a instanceof Animal);  // true（Dog は Animal でもある）
System.out.println(a instanceof Dog);     // true
System.out.println(a instanceof Cat);     // false

// instanceof でチェックしてからキャスト
if (a instanceof Dog) {
    Dog dog = (Dog) a;  // ダウンキャスト
    dog.fetch();        // Dog 固有のメソッド
}
```

---

### toString() のオーバーライド

`Object` クラスの `toString()` をオーバーライドすると、`println` などで自動的に使われます。

```java
class Point {
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "Point(" + x + ", " + y + ")";
    }
}

Point p = new Point(3, 4);
System.out.println(p);         // Point(3, 4)
System.out.println("座標: " + p); // 座標: Point(3, 4)
```

---

### コード例：図形の面積計算

```java
class Shape {
    String color;

    public Shape(String color) {
        this.color = color;
    }

    public double area() {
        return 0.0;  // デフォルト実装
    }

    public void show() {
        System.out.printf("%s（%s）面積: %.2f%n",
            getClass().getSimpleName(), color, area());
    }
}

class Circle extends Shape {
    double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    double width, height;

    public Rectangle(String color, double width, double height) {
        super(color);
        this.width  = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape[] shapes = {
            new Circle("赤", 5.0),
            new Rectangle("青", 4.0, 6.0),
            new Circle("緑", 3.0)
        };

        double total = 0;
        for (Shape s : shapes) {
            s.show();
            total += s.area();
        }
        System.out.printf("合計面積: %.2f%n", total);
    }
}
```

**出力結果：**
```
Circle（赤）面積: 78.54
Rectangle（青）面積: 24.00
Circle（緑）面積: 28.27
合計面積: 130.81
```

---

## ✏️ 問題

### 問題 8-2-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Vehicle {
    String name;

    public Vehicle(String name) {
        this.name = name;
    }

    public String move() {
        return name + " が移動する";
    }

    @Override
    public String toString() {
        return "[" + getClass().getSimpleName() + "] " + name;
    }
}

class Car extends Vehicle {
    public Car(String name) { super(name); }

    @Override
    public String move() {
        return name + " が走る 🚗";
    }
}

class Boat extends Vehicle {
    public Boat(String name) { super(name); }

    @Override
    public String move() {
        return name + " が泳ぐ 🚢";
    }
}

class Plane extends Vehicle {
    public Plane(String name) { super(name); }

    @Override
    public String move() {
        return name + " が飛ぶ ✈";
    }
}

public class Q1 {
    public static void main(String[] args) {
        Vehicle[] fleet = {
            new Car("トヨタ"),
            new Boat("フェリー"),
            new Plane("JAL"),
            new Car("ホンダ")
        };

        for (Vehicle v : fleet) {
            System.out.println(v.move());
        }

        System.out.println("---");

        for (Vehicle v : fleet) {
            System.out.println(v);
            System.out.println(v instanceof Car);
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
トヨタ が走る 🚗
フェリー が泳ぐ 🚢
JAL が飛ぶ ✈
ホンダ が走る 🚗
---
[Car] トヨタ
true
[Boat] フェリー
false
[Plane] JAL
false
[Car] ホンダ
true
```

**解説：**
- `Vehicle[]` に `Car`・`Boat`・`Plane` を混在させ、`move()` を呼ぶと各クラスのオーバーライドが実行されます（実行時ポリモーフィズム）。
- `toString()` は `getClass().getSimpleName()` でサブクラス名を取得します。
- `v instanceof Car` は `Car` のインスタンスのみ `true` になります。

</details>

---

### 問題 8-2-2（出力結果を答える）　⭐⭐⭐

次のコードを実行したとき、出力される結果を答えてください。

```java
class Animal {
    private String name;

    public Animal(String name) { this.name = name; }
    public String getName()    { return name; }

    public String speak()  { return "..."; }
    public String action() { return speak() + " と言って動く"; }
}

class Dog extends Animal {
    public Dog(String name) { super(name); }

    @Override
    public String speak() { return "ワン"; }
}

class PoliceDog extends Dog {
    public PoliceDog(String name) { super(name); }

    @Override
    public String speak() { return "ガウ"; }

    public String guard() {
        return getName() + ": " + action();
    }
}

public class Q2 {
    public static void main(String[] args) {
        Animal a  = new Animal("動物");
        Dog    d  = new Dog("ポチ");
        PoliceDog pd = new PoliceDog("レックス");

        System.out.println(a.action());
        System.out.println(d.action());
        System.out.println(pd.action());
        System.out.println(pd.guard());

        Animal ref = pd;
        System.out.println(ref.speak());
        System.out.println(ref instanceof Dog);
        System.out.println(ref instanceof PoliceDog);
    }
}
```

<details>
<summary>答えを見る</summary>

```
... と言って動く
ワン と言って動く
ガウ と言って動く
レックス: ガウ と言って動く
ガウ
true
true
```

**解説：**
- `action()` は `speak()` を内部で呼びます。`speak()` はオーバーライドされているため、実際のオブジェクトの `speak()` が呼ばれます（動的ディスパッチ）。
- `pd.action()` → `PoliceDog` の `speak()` が呼ばれ `"ガウ"` → `"ガウ と言って動く"`。
- `Animal ref = pd` は `Animal` 型でも、`speak()` は `PoliceDog` の実装が呼ばれます。
- `pd` は `PoliceDog` → `Dog` → `Animal` の順に継承しているため、`instanceof Dog` も `instanceof PoliceDog` も `true`。

</details>

---

### 問題 8-2-3（コード記述）　⭐⭐⭐

次の条件を満たすクラス群を作成してください。

**条件：**

- `Employee`（従業員）クラス：`String name`、`int baseSalary` を持つ。`calcSalary()` は `baseSalary` を返す。`show()` は `"[name] 給与: X円"` を出力する。

- `Manager`（管理職）クラス：`Employee` を継承。追加で `int bonus` フィールドを持つ。`calcSalary()` を `baseSalary + bonus` にオーバーライドする。

- `PartTimer`（パート）クラス：`Employee` を継承。追加で `int hoursWorked`（勤務時間）を持つ。`calcSalary()` を `baseSalary × hoursWorked` にオーバーライドする（baseSalary は時給として扱う）。

**main で以下を出力する：**

```
[田中] 給与: 300000円
[佐藤] 給与: 450000円
[山田] 給与: 112000円
---
合計給与: 862000円
```

<details>
<summary>答えを見る</summary>

```java
class Employee {
    String name;
    int    baseSalary;

    public Employee(String name, int baseSalary) {
        this.name       = name;
        this.baseSalary = baseSalary;
    }

    public int calcSalary() {
        return baseSalary;
    }

    public void show() {
        System.out.println("[" + name + "] 給与: " + calcSalary() + "円");
    }
}

class Manager extends Employee {
    int bonus;

    public Manager(String name, int baseSalary, int bonus) {
        super(name, baseSalary);
        this.bonus = bonus;
    }

    @Override
    public int calcSalary() {
        return baseSalary + bonus;
    }
}

class PartTimer extends Employee {
    int hoursWorked;

    public PartTimer(String name, int hourlyWage, int hoursWorked) {
        super(name, hourlyWage);
        this.hoursWorked = hoursWorked;
    }

    @Override
    public int calcSalary() {
        return baseSalary * hoursWorked;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee[] staff = {
            new Employee("田中",  300000),
            new Manager("佐藤",  300000, 150000),
            new PartTimer("山田", 1400,   80)
        };

        int total = 0;
        for (Employee e : staff) {
            e.show();
            total += e.calcSalary();
        }

        System.out.println("---");
        System.out.println("合計給与: " + total + "円");
    }
}
```

**解説：**
- `Employee[]` に `Employee`・`Manager`・`PartTimer` を混在させ、`e.show()` と `e.calcSalary()` を統一的に呼び出せます。
- `show()` 内の `calcSalary()` は動的ディスパッチにより各サブクラスの実装が呼ばれます。
- `Manager` の給与：`300000 + 150000 = 450000`。
- `PartTimer` の給与：`1400 × 80 = 112000`。

</details>
