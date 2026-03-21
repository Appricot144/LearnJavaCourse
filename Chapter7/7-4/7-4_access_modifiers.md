# 第7章：クラスとオブジェクト

## 7-4. アクセス修飾子

---

## 📖 解説

### アクセス修飾子とは？

アクセス修飾子とは、**クラス・フィールド・メソッドへのアクセス範囲を制御するキーワード**です。  
「どこから使えるか」を明示することで、意図しない操作を防ぎます。

---

### 4種類のアクセス修飾子

| 修飾子 | 同じクラス | 同じパッケージ | サブクラス | どこでも |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| （なし）package-private | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

> 💡 まずは `private`（隠す）と `public`（公開する）の2つをしっかり押さえましょう。

---

### private：クラスの外から隠す

フィールドを `private` にすると、クラスの外から直接アクセスできなくなります。

```java
public class Person {
    private String name;  // 外から直接アクセス不可
    private int    age;

    public Person(String name, int age) {
        this.name = name;
        this.age  = age;
    }
}

Person p = new Person("田中", 25);
System.out.println(p.name);  // ❌ コンパイルエラー！
p.age = -5;                  // ❌ コンパイルエラー！
```

---

### public getter / setter でアクセスを制御する

`private` フィールドには `public` なメソッドを通してアクセスします。  
setter に検証ロジックを入れることで、不正な値の設定を防げます。

```java
public class Person {
    private String name;
    private int    age;

    public Person(String name, int age) {
        this.name = name;
        setAge(age);  // setter 経由で検証
    }

    // getter
    public String getName() { return name; }
    public int    getAge()  { return age;  }

    // setter（検証あり）
    public void setName(String name) {
        if (name == null || name.isEmpty()) {
            throw new IllegalArgumentException("名前は空にできません");
        }
        this.name = name;
    }

    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("無効な年齢: " + age);
        }
        this.age = age;
    }

    public void show() {
        System.out.println(name + "（" + age + "歳）");
    }
}
```

---

### カプセル化のメリット

```
フィールドを private にする
       ↓
外から直接変更できなくなる
       ↓
setter で値の検証ができる
       ↓
不正な状態を防げる（例：age = -1 を弾く）
```

```java
Person p = new Person("田中", 25);
p.show();                 // 田中（25歳）

p.setName("鈴木");
p.show();                 // 鈴木（25歳）

// p.setAge(-5);          // ❌ 例外が投げられる
// p.age = -5;            // ❌ コンパイルエラー
```

---

### クラスへの public / デフォルト

クラス自体にも修飾子を付けられます。

```java
public class Animal { ... }   // どこからでも使える
class Helper { ... }          // 同じパッケージ内だけ
```

> 💡 1つの `.java` ファイルに `public` クラスは1つだけ定義でき、  
> ファイル名と `public` クラス名は一致させる必要があります。

---

### コード例

```java
public class BankAccount {
    private String owner;
    private int    balance;

    public BankAccount(String owner, int initialBalance) {
        this.owner   = owner;
        setBalance(initialBalance);
    }

    public String getOwner()   { return owner; }
    public int    getBalance() { return balance; }

    private void setBalance(int balance) {  // private：外から呼ばせない
        if (balance < 0) {
            throw new IllegalArgumentException("残高は0以上にしてください");
        }
        this.balance = balance;
    }

    public void deposit(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("入金額は1以上");
        balance += amount;
        System.out.println("入金: " + amount + "円 → 残高: " + balance + "円");
    }

    public void withdraw(int amount) {
        if (amount <= 0)   throw new IllegalArgumentException("出金額は1以上");
        if (amount > balance) {
            System.out.println("残高不足");
            return;
        }
        balance -= amount;
        System.out.println("出金: " + amount + "円 → 残高: " + balance + "円");
    }
}
```

---

## ✏️ 問題

### 問題 7-4-1（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
class Score {
    private int value;

    public Score(int value) {
        setValue(value);
    }

    public int getValue() { return value; }

    public void setValue(int value) {
        if (value < 0)   value = 0;
        if (value > 100) value = 100;
        this.value = value;
    }

    public String getRank() {
        if (value >= 90) return "S";
        if (value >= 70) return "A";
        if (value >= 50) return "B";
        return "C";
    }
}

public class Q1 {
    public static void main(String[] args) {
        Score s1 = new Score(85);
        Score s2 = new Score(120);  // 上限を超える
        Score s3 = new Score(-10);  // 下限を下回る

        System.out.println(s1.getValue() + " → " + s1.getRank());
        System.out.println(s2.getValue() + " → " + s2.getRank());
        System.out.println(s3.getValue() + " → " + s3.getRank());

        s1.setValue(45);
        System.out.println(s1.getValue() + " → " + s1.getRank());
    }
}
```

<details>
<summary>答えを見る</summary>

```
85 → A
100 → S
0 → C
45 → B
```

**解説：**
- `Score(85)` → `setValue(85)` → 範囲内なので `value = 85` → `"A"`。
- `Score(120)` → `setValue(120)` → 100を超えるので `value = 100` → `"S"`。
- `Score(-10)` → `setValue(-10)` → 0を下回るので `value = 0` → `"C"`。
- `s1.setValue(45)` → `value = 45` → `"B"`。
- `value` フィールドは `private` なので `s1.value = ...` は直接できません。

</details>

---

### 問題 7-4-2（コード記述）　⭐⭐⭐

次の条件を満たす `Temperature`（温度）クラスを作成してください。

**条件：**

| 要素 | 内容 |
|---|---|
| `private double celsius` | 摂氏温度（フィールド） |
| `public Temperature(double celsius)` | `setCelsius` 経由で初期化 |
| `public double getCelsius()` | 摂氏を返す |
| `public double getFahrenheit()` | 華氏（= celsius × 9/5 + 32）を返す |
| `public void setCelsius(double celsius)` | 絶対零度（-273.15）未満なら `IllegalArgumentException` を投げる。それ以外は設定する |
| `public void show()` | `"X.X℃ / Y.Y℉"` の形式で出力（小数第1位） |

**main で以下を出力する：**

```
100.0℃ / 212.0℉
-40.0℃ / -40.0℉
36.5℃ / 97.7℉
例外: 絶対零度より低い温度は設定できません
```

> 💡 例外は `try-catch` で捕捉して最後の行を出力してください。

<details>
<summary>答えを見る</summary>

```java
public class Temperature {
    private double celsius;

    public Temperature(double celsius) {
        setCelsius(celsius);
    }

    public double getCelsius() { return celsius; }

    public double getFahrenheit() {
        return celsius * 9.0 / 5.0 + 32.0;
    }

    public void setCelsius(double celsius) {
        if (celsius < -273.15) {
            throw new IllegalArgumentException(
                "絶対零度より低い温度は設定できません");
        }
        this.celsius = celsius;
    }

    public void show() {
        System.out.printf("%.1f℃ / %.1f℉%n", celsius, getFahrenheit());
    }
}

public class Main {
    public static void main(String[] args) {
        Temperature t1 = new Temperature(100.0);
        Temperature t2 = new Temperature(-40.0);
        Temperature t3 = new Temperature(36.5);

        t1.show();
        t2.show();
        t3.show();

        try {
            Temperature t4 = new Temperature(-300.0);
        } catch (IllegalArgumentException e) {
            System.out.println("例外: " + e.getMessage());
        }
    }
}
```

**解説：**
- `celsius` は `private` なので外部から `t1.celsius = ...` と直接変更できません。
- `setCelsius` に検証ロジックを入れることで、絶対零度より低い値を弾けます。
- `-40℃` は華氏でも `-40℉` で一致する特別な温度です（`-40 × 9/5 + 32 = -40`）。
- `try-catch` で `IllegalArgumentException` を捕捉して例外メッセージを出力します。

</details>
