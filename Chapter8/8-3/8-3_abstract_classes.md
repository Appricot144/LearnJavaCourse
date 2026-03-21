# 第8章：オブジェクト指向の概念

## 8-3. 抽象クラス

---

## 📖 解説

### 抽象クラスとは？

抽象クラスとは、**インスタンスを直接生成できないクラス**です。  
「共通の骨格だけ定義し、具体的な実装はサブクラスに委ねる」設計に使います。

```
通常のクラス → new でインスタンスを生成できる
抽象クラス  → new できない（サブクラス経由でのみ使う）
```

---

### 抽象クラスと抽象メソッドの宣言

`abstract` キーワードを使います。

```java
// 抽象クラス
public abstract class Shape {

    String color;

    public Shape(String color) {
        this.color = color;
    }

    // 抽象メソッド：実装なし、サブクラスで必ず実装する
    public abstract double area();
    public abstract double perimeter();

    // 通常メソッド：共通処理はここに書ける
    public void show() {
        System.out.printf("%s（%s）面積:%.2f 周囲:%.2f%n",
            getClass().getSimpleName(), color, area(), perimeter());
    }
}
```

> ⚠️ 抽象メソッドを1つでも持つクラスは `abstract` を付けなければなりません。

---

### 抽象クラスを継承するサブクラス

サブクラスは抽象メソッドを**すべて実装**しなければなりません。

```java
public class Circle extends Shape {
    double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }

    @Override
    public double perimeter() {
        return 2 * Math.PI * radius;
    }
}

public class Rectangle extends Shape {
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

    @Override
    public double perimeter() {
        return (width + height) * 2;
    }
}
```

---

### 通常クラス vs 抽象クラス vs インターフェース

| 比較項目 | 通常クラス | 抽象クラス | インターフェース |
|---|---|---|---|
| インスタンス生成 | ✅ できる | ❌ できない | ❌ できない |
| 抽象メソッド | ❌ 持てない | ✅ 持てる | ✅ 全てが抽象 |
| 通常メソッド | ✅ 持てる | ✅ 持てる | ✅ default のみ |
| フィールド | ✅ 持てる | ✅ 持てる | ❌ 定数のみ |
| 継承 | 1つだけ | 1つだけ | 複数可能 |

> 💡 インターフェースは第9章で詳しく学習します。

---

### 抽象クラスの使いどころ

```
✅ 向いているケース
・複数のクラスに共通の「骨格」がある
・共通処理を親クラスにまとめたい
・特定のメソッドはサブクラスに実装を強制したい

❌ 向いていないケース
・共通フィールドや共通処理が何もない（→ インターフェース向き）
・複数の親から継承したい（→ インターフェース向き）
```

---

### テンプレートメソッドパターン

抽象クラスの典型的な使い方の1つです。  
「処理の骨格」を親クラスで定義し、個々のステップをサブクラスに実装させます。

```java
public abstract class DataProcessor {

    // テンプレートメソッド（処理の流れを定義）
    public final void process() {
        readData();
        processData();
        writeResult();
    }

    // サブクラスで実装する抽象メソッド
    protected abstract void readData();
    protected abstract void processData();
    protected abstract void writeResult();
}

public class CsvProcessor extends DataProcessor {
    @Override
    protected void readData()     { System.out.println("CSVファイルを読み込む"); }
    @Override
    protected void processData()  { System.out.println("CSVデータを処理する"); }
    @Override
    protected void writeResult()  { System.out.println("結果をCSVに書き出す"); }
}

public class JsonProcessor extends DataProcessor {
    @Override
    protected void readData()     { System.out.println("JSONファイルを読み込む"); }
    @Override
    protected void processData()  { System.out.println("JSONデータを処理する"); }
    @Override
    protected void writeResult()  { System.out.println("結果をJSONに書き出す"); }
}
```

---

### コード例

```java
public class Main {
    public static void main(String[] args) {
        // Shape は抽象クラスなので new Shape() はできない
        Shape[] shapes = {
            new Circle("赤", 5.0),
            new Rectangle("青", 4.0, 6.0),
            new Circle("緑", 3.0)
        };

        for (Shape s : shapes) {
            s.show();
        }
    }
}
```

**出力結果：**
```
Circle（赤）面積:78.54 周囲:31.42
Rectangle（青）面積:24.00 周囲:20.00
Circle（緑）面積:28.27 周囲:18.85
```

---

## ✏️ 問題

### 問題 8-3-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
abstract class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }

    public abstract String sound();

    public void introduce() {
        System.out.println("私は " + name + " です。" + sound() + " と鳴きます。");
    }
}

class Dog extends Animal {
    public Dog(String name) { super(name); }

    @Override
    public String sound() { return "ワン"; }

    public void fetch() { System.out.println(name + " がボールを取ってきた！"); }
}

class Cat extends Animal {
    public Cat(String name) { super(name); }

    @Override
    public String sound() { return "ニャー"; }
}

public class Q1 {
    public static void main(String[] args) {
        Animal[] animals = {
            new Dog("ポチ"),
            new Cat("タマ"),
            new Dog("ハチ")
        };

        for (Animal a : animals) {
            a.introduce();
        }

        System.out.println("---");

        for (Animal a : animals) {
            if (a instanceof Dog) {
                ((Dog) a).fetch();
            }
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
私は ポチ です。ワン と鳴きます。
私は タマ です。ニャー と鳴きます。
私は ハチ です。ワン と鳴きます。
---
ポチ がボールを取ってきた！
ハチ がボールを取ってきた！
```

**解説：**
- `Animal` は抽象クラスなので直接インスタンス化はできませんが、`Dog` / `Cat` は具体クラスなので生成できます。
- `introduce()` 内の `sound()` は動的ディスパッチで各サブクラスの実装が呼ばれます。
- `instanceof Dog` でチェックし、ダウンキャストしてから `fetch()` を呼び出しています。
- `Cat` は `fetch()` を持たないため、`Dog` のみ `fetch()` が実行されます。

</details>

---

### 問題 8-3-2（出力結果を答える）　⭐⭐⭐

次のコードを実行したとき、出力される結果を答えてください。

```java
abstract class Game {
    private String title;

    public Game(String title) {
        this.title = title;
    }

    // テンプレートメソッド
    public final void play() {
        System.out.println("=== " + title + " ===");
        initialize();
        startPlay();
        endPlay();
    }

    protected abstract void initialize();
    protected abstract void startPlay();
    protected abstract void endPlay();
}

class Chess extends Game {
    public Chess() { super("チェス"); }

    @Override
    protected void initialize() { System.out.println("チェス盤を準備する"); }
    @Override
    protected void startPlay()  { System.out.println("チェスを開始する"); }
    @Override
    protected void endPlay()    { System.out.println("チェスを終了する"); }
}

class Soccer extends Game {
    public Soccer() { super("サッカー"); }

    @Override
    protected void initialize() { System.out.println("グラウンドを準備する"); }
    @Override
    protected void startPlay()  { System.out.println("キックオフ！"); }
    @Override
    protected void endPlay()    { System.out.println("試合終了のホイッスル"); }
}

public class Q2 {
    public static void main(String[] args) {
        Game[] games = { new Chess(), new Soccer() };

        for (Game g : games) {
            g.play();
            System.out.println();
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
=== チェス ===
チェス盤を準備する
チェスを開始する
チェスを終了する

=== サッカー ===
グラウンドを準備する
キックオフ！
試合終了のホイッスル

```

**解説：**
- `play()` は `final` メソッドなのでオーバーライドできません（処理の流れを固定）。
- `play()` の中で `initialize()` → `startPlay()` → `endPlay()` の順に呼ばれます。
- それぞれのメソッドはサブクラスの実装が動的ディスパッチで呼ばれます。
- これがテンプレートメソッドパターンです。

</details>

---

### 問題 8-3-3（コード記述）　⭐⭐⭐

次の条件を満たすクラス群を作成してください。

**条件：**

- 抽象クラス `Tax`（税金）を定義する
  - `double price`（税抜き価格）フィールド
  - コンストラクタ `(double price)`
  - 抽象メソッド `double getTaxRate()`（税率を返す）
  - 通常メソッド `double getTaxAmount()`（税額 = price × getTaxRate()）を返す
  - 通常メソッド `double getTotalPrice()`（税込価格 = price + 税額）を返す
  - 通常メソッド `void show()`（`"税抜:X円 税率:Y% 税込:Z円"` の形式で出力）

- 具体クラス `ReducedTax`（軽減税率 8%）：`getTaxRate()` が `0.08` を返す
- 具体クラス `StandardTax`（標準税率 10%）：`getTaxRate()` が `0.10` を返す

**main で以下を出力する：**

```
税抜:1000円 税率:8.0% 税込:1080.0円
税抜:1000円 税率:10.0% 税込:1100.0円
税抜:500円 税率:8.0% 税込:540.0円
合計（税込）: 2720.0円
```

<details>
<summary>答えを見る</summary>

```java
abstract class Tax {
    double price;

    public Tax(double price) {
        this.price = price;
    }

    public abstract double getTaxRate();

    public double getTaxAmount() {
        return price * getTaxRate();
    }

    public double getTotalPrice() {
        return price + getTaxAmount();
    }

    public void show() {
        System.out.printf("税抜:%.0f円 税率:%.1f%% 税込:%.1f円%n",
            price, getTaxRate() * 100, getTotalPrice());
    }
}

class ReducedTax extends Tax {
    public ReducedTax(double price) { super(price); }

    @Override
    public double getTaxRate() { return 0.08; }
}

class StandardTax extends Tax {
    public StandardTax(double price) { super(price); }

    @Override
    public double getTaxRate() { return 0.10; }
}

public class Main {
    public static void main(String[] args) {
        Tax[] items = {
            new ReducedTax(1000),
            new StandardTax(1000),
            new ReducedTax(500)
        };

        double total = 0;
        for (Tax t : items) {
            t.show();
            total += t.getTotalPrice();
        }

        System.out.println("合計（税込）: " + total + "円");
    }
}
```

**解説：**
- `Tax` は抽象クラスなので `new Tax(1000)` はできません。
- `getTaxAmount()` と `getTotalPrice()` は `getTaxRate()` を内部で呼びます。動的ディスパッチで各サブクラスの税率が使われます。
- `show()` の `%%` は `printf` でパーセント記号を出力するためのエスケープです。
- 合計は `1080.0 + 1100.0 + 540.0 = 2720.0`。

</details>
