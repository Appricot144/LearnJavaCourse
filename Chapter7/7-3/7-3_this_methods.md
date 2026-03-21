# 第7章：クラスとオブジェクト

## 7-3. thisキーワード・メソッド

---

## 📖 解説

### this の3つの使い方

`this` はクラスの**インスタンス自身**を指すキーワードです。  
Javaでは主に3つの場面で使われます。

| 使い方 | 説明 |
|---|---|
| `this.フィールド名` | フィールドと引数の名前が衝突するとき区別する |
| `this.メソッド名()` | 自分自身のメソッドを明示的に呼び出す |
| `this(引数)` | 同じクラスの別コンストラクタを呼び出す |

---

### 使い方1：フィールドと引数の区別

```java
public class Person {
    String name;
    int    age;

    public Person(String name, int age) {
        this.name = name;  // this.name → フィールド
        this.age  = age;   //      name → 引数
    }

    public void setName(String name) {
        this.name = name;  // セッターでも同様
    }
}
```

---

### 使い方2：メソッドチェーン（thisを返す）

メソッドが `this` を返すことで、メソッドを連続して呼び出せます（メソッドチェーン）。

```java
public class Builder {
    String text = "";

    public Builder add(String s) {
        text += s;
        return this;  // 自分自身を返す
    }

    public Builder addLine(String s) {
        text += s + "\n";
        return this;
    }

    public String build() {
        return text;
    }
}

Builder b = new Builder();
String result = b.add("Hello").add(", ").addLine("World!").build();
System.out.println(result);  // Hello, World!
```

---

### 使い方3：コンストラクタの委譲（this()）

`this()` を使うと、同じクラスの別コンストラクタを呼び出せます。  
**必ずコンストラクタの先頭行に書く必要があります。**

```java
public class Rectangle {
    double width;
    double height;

    // メインコンストラクタ
    public Rectangle(double width, double height) {
        this.width  = width;
        this.height = height;
    }

    // 正方形用コンストラクタ（委譲）
    public Rectangle(double side) {
        this(side, side);  // 上のコンストラクタに委譲
    }

    // デフォルト（1×1）
    public Rectangle() {
        this(1.0);  // 上の委譲コンストラクタに委譲
    }
}

Rectangle r1 = new Rectangle(4.0, 6.0);  // 4×6の長方形
Rectangle r2 = new Rectangle(5.0);       // 5×5の正方形
Rectangle r3 = new Rectangle();          // 1×1の正方形
```

---

### アクセサメソッド（getter / setter）

フィールドを `private` にして、外部からのアクセスを `getter` / `setter` で制御する設計です。  
これがカプセル化の基本です（第8章で詳しく学習）。

```java
public class Circle {
    private double radius;  // private で外から直接アクセス不可

    public Circle(double radius) {
        setRadius(radius);  // setter 経由で初期化
    }

    // getter：値を返す
    public double getRadius() {
        return radius;
    }

    // setter：検証してから値をセット
    public void setRadius(double radius) {
        if (radius < 0) {
            throw new IllegalArgumentException("半径は0以上にしてください");
        }
        this.radius = radius;
    }

    public double getArea() {
        return Math.PI * radius * radius;
    }
}
```

---

### インスタンスメソッドとクラスメソッド

| 種類 | 宣言 | 呼び出し | アクセス |
|---|---|---|---|
| インスタンスメソッド | `public void method()` | `obj.method()` | フィールドにアクセス可 |
| クラスメソッド（static） | `public static void method()` | `ClassName.method()` | static フィールドのみ |

```java
public class MathUtils {
    // クラスメソッド（インスタンス不要）
    public static int square(int n) {
        return n * n;
    }

    // インスタンスメソッド（インスタンスが必要）
    public int value;

    public int doubled() {
        return value * 2;
    }
}

// クラスメソッドはクラス名で呼ぶ
System.out.println(MathUtils.square(5));  // 25

// インスタンスメソッドはインスタンスで呼ぶ
MathUtils m = new MathUtils();
m.value = 10;
System.out.println(m.doubled());          // 20
```

---

### コード例：チェーンメソッドによる設定

```java
public class Pizza {
    String  size;
    String  crust;
    boolean cheese;
    boolean pepperoni;

    public Pizza(String size) {
        this.size      = size;
        this.crust     = "thin";
        this.cheese    = true;
        this.pepperoni = false;
    }

    public Pizza crust(String crust) {
        this.crust = crust;
        return this;
    }

    public Pizza pepperoni() {
        this.pepperoni = true;
        return this;
    }

    public void order() {
        System.out.println(size + "サイズ / "
            + crust + "クラスト / "
            + (cheese ? "チーズあり" : "チーズなし") + " / "
            + (pepperoni ? "ペパロニあり" : "ペパロニなし"));
    }
}

// Main
Pizza pizza = new Pizza("M").crust("thick").pepperoni();
pizza.order();
// → Mサイズ / thickクラスト / チーズあり / ペパロニあり
```

---

## ✏️ 問題

### 問題 7-3-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Vector2D {
    double x;
    double y;

    public Vector2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public Vector2D add(Vector2D other) {
        return new Vector2D(this.x + other.x, this.y + other.y);
    }

    public Vector2D scale(double factor) {
        return new Vector2D(this.x * factor, this.y * factor);
    }

    public void show() {
        System.out.println("(" + x + ", " + y + ")");
    }
}

public class Q1 {
    public static void main(String[] args) {
        Vector2D v1 = new Vector2D(1.0, 2.0);
        Vector2D v2 = new Vector2D(3.0, 4.0);

        v1.show();
        v2.show();

        Vector2D v3 = v1.add(v2);
        v3.show();

        Vector2D v4 = v1.scale(3.0);
        v4.show();

        v1.show();  // v1 は変化しない
    }
}
```

<details>
<summary>答えを見る</summary>

```
(1.0, 2.0)
(3.0, 4.0)
(4.0, 6.0)
(3.0, 6.0)
(1.0, 2.0)
```

**解説：**
- `v1.add(v2)` は `v1` の `x+v2.x`, `y+v2.y` の**新しいインスタンス**を返します。`v1` 自体は変化しません。
- `v1.scale(3.0)` も同様に新しいインスタンスを返します。`v1.x=1.0×3.0=3.0`、`v1.y=2.0×3.0=6.0`。
- 最後の `v1.show()` は元のまま `(1.0, 2.0)` です。

</details>

---

### 問題 7-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Config {
    String host;
    int    port;
    int    timeout;

    public Config() {
        this.host    = "localhost";
        this.port    = 8080;
        this.timeout = 30;
    }

    public Config host(String host) {
        this.host = host;
        return this;
    }

    public Config port(int port) {
        this.port = port;
        return this;
    }

    public Config timeout(int timeout) {
        this.timeout = timeout;
        return this;
    }

    public void show() {
        System.out.println(host + ":" + port
            + " (timeout=" + timeout + "s)");
    }
}

public class Q2 {
    public static void main(String[] args) {
        Config c1 = new Config();
        c1.show();

        Config c2 = new Config()
            .host("example.com")
            .port(443)
            .timeout(60);
        c2.show();

        Config c3 = new Config().port(9000);
        c3.show();
    }
}
```

<details>
<summary>答えを見る</summary>

```
localhost:8080 (timeout=30s)
example.com:443 (timeout=60s)
localhost:9000 (timeout=30s)
```

**解説：**
- `c1` はデフォルト値のまま（localhost:8080 timeout=30）。
- `c2` は `host`・`port`・`timeout` をすべてメソッドチェーンで設定。各メソッドは `this` を返すため連続呼び出しが可能。
- `c3` は `port` だけ変更（9000）、`host` と `timeout` はデフォルト値のまま。

</details>

---

### 問題 7-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `ShoppingCart`（買い物かご）クラスを作成してください。

**条件：**

| 要素 | 内容 |
|---|---|
| フィールド | `String[] items`（商品名、最大10個）、`int[] prices`（価格）、`int count`（登録数） |
| コンストラクタ | 引数なし。`items = new String[10]`、`prices = new int[10]`、`count = 0` に初期化 |
| `addItem(String name, int price)` | かごに商品を追加。`count` が10以上なら `"かごがいっぱいです"` を出力。追加後 `"[name] を追加（price円）"` を出力。`this` を返す（メソッドチェーン用） |
| `total()` | 合計金額を返す（`int`） |
| `showCart()` | かご内の全商品と合計を出力 |

**main で以下を実行して出力する：**

```
[りんご] を追加（150円）
[パン] を追加（200円）
[牛乳] を追加（180円）
---
りんご: 150円
パン: 200円
牛乳: 180円
合計: 530円
```

<details>
<summary>答えを見る</summary>

```java
class ShoppingCart {
    String[] items;
    int[]    prices;
    int      count;

    public ShoppingCart() {
        items  = new String[10];
        prices = new int[10];
        count  = 0;
    }

    public ShoppingCart addItem(String name, int price) {
        if (count >= 10) {
            System.out.println("かごがいっぱいです");
            return this;
        }
        items[count]  = name;
        prices[count] = price;
        count++;
        System.out.println("[" + name + "] を追加（" + price + "円）");
        return this;
    }

    public int total() {
        int sum = 0;
        for (int i = 0; i < count; i++) {
            sum += prices[i];
        }
        return sum;
    }

    public void showCart() {
        for (int i = 0; i < count; i++) {
            System.out.println(items[i] + ": " + prices[i] + "円");
        }
        System.out.println("合計: " + total() + "円");
    }
}

public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        cart.addItem("りんご", 150)
            .addItem("パン",   200)
            .addItem("牛乳",   180);

        System.out.println("---");
        cart.showCart();
    }
}
```

**解説：**
- `addItem` は `this` を返すため `cart.addItem(...).addItem(...).addItem(...)` のようにメソッドチェーンで書けます。
- `total()` は `prices` 配列を `count` の個数だけ合計します。
- `showCart()` は登録済みの商品を `items[i]` と `prices[i]` で出力し、最後に `total()` を呼び出して合計を表示します。

</details>
