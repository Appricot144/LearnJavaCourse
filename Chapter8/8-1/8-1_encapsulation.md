# 第8章：オブジェクト指向の概念

## 8-1. カプセル化（フィールドとアクセサ）

---

## 📖 解説

### オブジェクト指向の3大原則

Javaのオブジェクト指向は3つの柱で成り立っています。

| 原則 | 意味 | 本節の位置づけ |
|---|---|---|
| **カプセル化** | データと処理をまとめ、内部を隠す | ← 今ここ |
| **継承** | 既存クラスを引き継いで拡張する | 第9章 |
| **ポリモーフィズム** | 同じ操作で異なる振る舞いをする | 第8-2節 |

---

### カプセル化とは？

カプセル化とは、**データ（フィールド）と処理（メソッド）をクラスにまとめ、外部からの直接アクセスを隠す**設計原則です。

```
カプセル化のイメージ：

  外部から見える部分（public）
  ┌─────────────────────────────┐
  │  + deposit(amount)          │← public メソッド
  │  + withdraw(amount)         │
  │  + getBalance()             │
  ├─────────────────────────────┤
  │  - balance  ← 見えない！    │← private フィールド
  │  - owner    ← 見えない！    │
  └─────────────────────────────┘
```

---

### カプセル化の実装パターン

1. フィールドを `private` にする
2. 読み取り用の `getter` を定義する
3. 書き込み用の `setter` を定義する（検証ロジックを入れる）

```java
public class Person {
    // 1. private フィールド
    private String name;
    private int    age;

    public Person(String name, int age) {
        this.name = name;
        setAge(age);  // 検証を通す
    }

    // 2. getter（読み取り専用）
    public String getName() { return name; }
    public int    getAge()  { return age;  }

    // 3. setter（検証つき書き込み）
    public void setName(String name) {
        if (name == null || name.isBlank())
            throw new IllegalArgumentException("名前は空にできません");
        this.name = name;
    }

    public void setAge(int age) {
        if (age < 0 || age > 150)
            throw new IllegalArgumentException("年齢が不正: " + age);
        this.age = age;
    }
}
```

---

### getter だけで setter を作らない（読み取り専用）

変更させたくないフィールドは getter だけを定義します。

```java
public class Circle {
    private final double radius;  // final で変更不可

    public Circle(double radius) {
        if (radius <= 0)
            throw new IllegalArgumentException("半径は正の値にしてください");
        this.radius = radius;
    }

    public double getRadius() { return radius; }        // getter のみ
    public double getArea()   { return Math.PI * radius * radius; }
    public double getCircumference() { return 2 * Math.PI * radius; }
}
```

---

### final フィールド

`final` を付けたフィールドは**一度設定したら変更できません**。  
コンストラクタで初期化する必要があります。

```java
public class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() { return x; }
    public int getY() { return y; }

    // 移動は新しい Point を返す（イミュータブル設計）
    public Point translate(int dx, int dy) {
        return new Point(x + dx, y + dy);
    }

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}
```

---

### カプセル化のメリットまとめ

| メリット | 説明 |
|---|---|
| **安全性** | 不正な値の設定を防げる |
| **保守性** | 内部実装を変えても外部への影響がない |
| **可読性** | メソッド名で意図が明確になる |
| **再利用性** | 検証ロジックが1カ所にまとまる |

---

### コード例：銀行口座

```java
public class BankAccount {
    private final String id;     // 変更不可
    private final String owner;  // 変更不可
    private int balance;         // 変更可（入出金で変わる）

    public BankAccount(String id, String owner, int initialBalance) {
        this.id    = id;
        this.owner = owner;
        setBalance(initialBalance);
    }

    public String getId()      { return id; }
    public String getOwner()   { return owner; }
    public int    getBalance() { return balance; }

    private void setBalance(int balance) {
        if (balance < 0)
            throw new IllegalArgumentException("残高は0以上");
        this.balance = balance;
    }

    public void deposit(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("入金額は1以上");
        balance += amount;
    }

    public boolean withdraw(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("出金額は1以上");
        if (amount > balance) return false;
        balance -= amount;
        return true;
    }

    @Override
    public String toString() {
        return "[" + id + "] " + owner + " 残高:" + balance + "円";
    }
}
```

---

## ✏️ 問題

### 問題 8-1-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Thermometer {
    private double temperature;
    private double maxRecorded;
    private double minRecorded;

    public Thermometer(double initial) {
        this.temperature  = initial;
        this.maxRecorded  = initial;
        this.minRecorded  = initial;
    }

    public void setTemperature(double temp) {
        this.temperature = temp;
        if (temp > maxRecorded) maxRecorded = temp;
        if (temp < minRecorded) minRecorded = temp;
    }

    public double getTemperature() { return temperature; }
    public double getMax()         { return maxRecorded; }
    public double getMin()         { return minRecorded; }

    public void show() {
        System.out.println("現在:" + temperature
            + " 最高:" + maxRecorded
            + " 最低:" + minRecorded);
    }
}

public class Q1 {
    public static void main(String[] args) {
        Thermometer t = new Thermometer(20.0);
        t.show();

        t.setTemperature(35.5);
        t.setTemperature(12.3);
        t.setTemperature(28.0);
        t.show();

        System.out.println(t.getMax() - t.getMin());
    }
}
```

<details>
<summary>答えを見る</summary>

```
現在:20.0 最高:20.0 最低:20.0
現在:28.0 最高:35.5 最低:12.3
23.2
```

**解説：**
- 初期値は `20.0`。最高・最低ともに `20.0`。
- `35.5` → 最高更新。`12.3` → 最低更新。`28.0` → 現在値のみ更新。
- 最後は `35.5 - 12.3 = 23.2`。
- `temperature` は `private` なので外から直接変更はできません。

</details>

---

### 問題 8-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class ImmutablePoint {
    private final int x;
    private final int y;

    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() { return x; }
    public int getY() { return y; }

    public ImmutablePoint translate(int dx, int dy) {
        return new ImmutablePoint(x + dx, y + dy);
    }

    public double distanceTo(ImmutablePoint other) {
        int dx = this.x - other.x;
        int dy = this.y - other.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

public class Q2 {
    public static void main(String[] args) {
        ImmutablePoint p1 = new ImmutablePoint(0, 0);
        ImmutablePoint p2 = p1.translate(3, 4);
        ImmutablePoint p3 = p2.translate(1, 0);

        System.out.println(p1);
        System.out.println(p2);
        System.out.println(p3);
        System.out.println(p1.distanceTo(p2));
        System.out.println(p2.distanceTo(p3));
    }
}
```

<details>
<summary>答えを見る</summary>

```
(0, 0)
(3, 4)
(4, 4)
5.0
1.0
```

**解説：**
- `p1` は `(0, 0)` のまま変化しません（`final` フィールド・イミュータブル設計）。
- `p1.translate(3, 4)` → 新しいインスタンス `(3, 4)` を返します。
- `p2.translate(1, 0)` → 新しいインスタンス `(4, 4)` を返します。
- `p1` から `p2` の距離：`√(3²+4²) = √25 = 5.0`（三平方の定理）。
- `p2` から `p3` の距離：`√(1²+0²) = 1.0`。

</details>

---

### 問題 8-1-3（コード記述）　⭐⭐⭐

次の条件を満たす `Student`（学生）クラスを作成してください。

**条件：**

| 要素 | 内容 |
|---|---|
| `private final String id` | 学籍番号（変更不可） |
| `private String name` | 氏名 |
| `private int[] scores` | テストの点数（最大5教科・`private final`） |
| `private int scoreCount` | 登録した点数の数 |
| コンストラクタ | `(String id, String name)` で初期化。`scores = new int[5]`、`scoreCount = 0` |
| `getId()` / `getName()` | getter |
| `setName(String)` | 空文字は拒否 |
| `addScore(int score)` | 0〜100 の範囲チェック後に追加。満杯なら `"これ以上追加できません"` を出力 |
| `getAverage()` | 平均を `double` で返す（点数が0件なら0.0） |
| `show()` | `"[id] name 平均:X.X点"` の形式で出力 |

**期待される出力：**
```
[S001] 田中太郎 平均:0.0点
[S001] 田中太郎 平均:78.0点
[S001] 田中太郎 平均:76.0点
```

<details>
<summary>答えを見る</summary>

```java
public class Student {
    private final String id;
    private String name;
    private final int[] scores;
    private int scoreCount;

    public Student(String id, String name) {
        this.id         = id;
        this.name       = name;
        this.scores     = new int[5];
        this.scoreCount = 0;
    }

    public String getId()   { return id; }
    public String getName() { return name; }

    public void setName(String name) {
        if (name == null || name.isBlank())
            throw new IllegalArgumentException("名前は空にできません");
        this.name = name;
    }

    public void addScore(int score) {
        if (score < 0 || score > 100)
            throw new IllegalArgumentException("点数は0〜100の範囲で入力してください");
        if (scoreCount >= scores.length) {
            System.out.println("これ以上追加できません");
            return;
        }
        scores[scoreCount++] = score;
    }

    public double getAverage() {
        if (scoreCount == 0) return 0.0;
        int sum = 0;
        for (int i = 0; i < scoreCount; i++) sum += scores[i];
        return (double) sum / scoreCount;
    }

    public void show() {
        System.out.printf("[%s] %s 平均:%.1f点%n", id, name, getAverage());
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student("S001", "田中太郎");
        s.show();

        s.addScore(80);
        s.addScore(75);
        s.addScore(79);
        s.show();

        s.addScore(68);
        s.addScore(78);
        s.show();
    }
}
```

**解説：**
- `id` と `scores` は `final` なので再代入不可です。
- `addScore` は0〜100の範囲チェックと上限チェックを行います。
- `getAverage` は `scoreCount` が0のとき `0.0` を返して0除算を防ぎます。
- `show` では `printf` で小数第1位まで出力します。

</details>
