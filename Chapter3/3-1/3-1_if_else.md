# 第3章：条件分岐

## 3-1. if / else if / else

---

## 📖 解説

### 条件分岐とは？

条件分岐とは、**条件に応じて処理を切り替える**仕組みです。  
Javaでは `if`、`else if`、`else` を使って記述します。

---

### if 文の基本構造

```
if (条件式) {
    // 条件が true のときに実行される処理
}
```

```java
int score = 80;

if (score >= 60) {
    System.out.println("合格");  // score が 60 以上なので実行される
}
```

---

### if / else 文

`else` ブロックは条件が `false` のときに実行されます。

```
if (条件式) {
    // true のとき
} else {
    // false のとき
}
```

```java
int score = 45;

if (score >= 60) {
    System.out.println("合格");
} else {
    System.out.println("不合格");  // こちらが実行される
}
```

---

### if / else if / else 文

複数の条件を順番にチェックするときは `else if` を使います。

```
if (条件1) {
    // 条件1 が true のとき
} else if (条件2) {
    // 条件1 が false かつ 条件2 が true のとき
} else if (条件3) {
    // 条件1・2 が false かつ 条件3 が true のとき
} else {
    // すべての条件が false のとき
}
```

> ⚠️ **上から順に評価され、最初に `true` になった1つだけが実行されます。**

```java
int score = 75;

if (score >= 90) {
    System.out.println("S評価");
} else if (score >= 80) {
    System.out.println("A評価");
} else if (score >= 70) {
    System.out.println("B評価");  // ← これが実行される
} else if (score >= 60) {
    System.out.println("C評価");
} else {
    System.out.println("不合格");
}
```

---

### 条件の評価フロー

```
score = 75

↓ score >= 90 ? → false
↓ score >= 80 ? → false
↓ score >= 70 ? → true  →【B評価】を出力して終了
```

---

### ネストした if 文

`if` 文の中にさらに `if` 文を書くことができます（ネスト）。

```java
int age = 20;
boolean hasTicket = true;

if (age >= 18) {
    if (hasTicket) {
        System.out.println("入場できます");     // ← こちらが実行される
    } else {
        System.out.println("チケットが必要です");
    }
} else {
    System.out.println("年齢制限があります");
}
```

> 💡 ネストが深くなりすぎると読みにくくなります。  
> `&&` を使って1つの条件式にまとめる方が読みやすい場合もあります。

---

### コード例

```java
public class GradeChecker {
    public static void main(String[] args) {
        int score = 68;

        if (score >= 90) {
            System.out.println("S評価：優秀です！");
        } else if (score >= 80) {
            System.out.println("A評価：よくできました");
        } else if (score >= 70) {
            System.out.println("B評価：もう少し頑張ろう");
        } else if (score >= 60) {
            System.out.println("C評価：ギリギリ合格");
        } else {
            System.out.println("不合格：再挑戦してください");
        }
    }
}
```

**出力結果：**
```
C評価：ギリギリ合格
```

---

## ✏️ 問題

### 問題 3-1-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int x = 15;

        if (x > 10) {
            System.out.println("10より大きい");
        }

        if (x > 20) {
            System.out.println("20より大きい");
        } else {
            System.out.println("20以下");
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
10より大きい
20以下
```

**解説：**
- `x = 15` なので `x > 10` は `true` → `"10より大きい"` が出力されます。
- `x > 20` は `false` → `else` の `"20以下"` が出力されます。

</details>

---

### 問題 3-1-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int score = 80;

        if (score >= 90) {
            System.out.println("S");
        } else if (score >= 80) {
            System.out.println("A");
        } else if (score >= 70) {
            System.out.println("B");
        } else {
            System.out.println("C");
        }

        int temperature = 28;

        if (temperature >= 35) {
            System.out.println("猛暑日");
        } else if (temperature >= 30) {
            System.out.println("真夏日");
        } else if (temperature >= 25) {
            System.out.println("夏日");
        } else {
            System.out.println("通常");
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
A
夏日
```

**解説：**
- `score = 80` は `>= 80` を満たすので `"A"` が出力されます。`>= 70` も満たしますが、すでに `else if (score >= 80)` で実行されたため評価は終了します。
- `temperature = 28` は `>= 35`・`>= 30` を満たさず、`>= 25` を満たすので `"夏日"` が出力されます。

</details>

---

### 問題 3-1-3（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q3 {
    public static void main(String[] args) {
        int age = 16;
        boolean hasParentConsent = true;

        if (age >= 18) {
            System.out.println("単独で登録できます");
        } else if (age >= 13 && hasParentConsent) {
            System.out.println("保護者同意のもと登録できます");
        } else if (age >= 13) {
            System.out.println("保護者の同意が必要です");
        } else {
            System.out.println("登録できません");
        }
    }
}
```

<details>
<summary>答えを見る</summary>

```
保護者同意のもと登録できます
```

**解説：**
- `age = 16` なので `age >= 18` は `false`。
- `age >= 13 && hasParentConsent` は `true && true` → `true` なので `"保護者同意のもと登録できます"` が出力されます。
- 3番目の `else if (age >= 13)` も条件を満たしますが、2番目ですでに実行されたため評価は終了します。

</details>

---

### 問題 3-1-4（コード記述）　⭐⭐⭐

次の条件を満たす `BMIChecker` クラスを作成してください。

**条件：**
- `double` 型の変数 `bmi` に `22.5` を代入する
- 以下の判定基準に従って結果を出力する

| BMI | 判定 |
|---|---|
| 18.5 未満 | 低体重 |
| 18.5 以上 25.0 未満 | 普通体重 |
| 25.0 以上 30.0 未満 | 肥満（1度） |
| 30.0 以上 | 肥満（2度以上） |

- さらに、BMIが `18.5 以上 25.0 未満` のときだけ `"健康的な体重です"` を追加で出力する

**期待される出力：**
```
BMI: 22.5
判定: 普通体重
健康的な体重です
```

<details>
<summary>答えを見る</summary>

```java
public class BMIChecker {
    public static void main(String[] args) {
        double bmi = 22.5;

        System.out.println("BMI: " + bmi);

        if (bmi < 18.5) {
            System.out.println("判定: 低体重");
        } else if (bmi < 25.0) {
            System.out.println("判定: 普通体重");
            System.out.println("健康的な体重です");
        } else if (bmi < 30.0) {
            System.out.println("判定: 肥満（1度）");
        } else {
            System.out.println("判定: 肥満（2度以上）");
        }
    }
}
```

**解説：**
- `bmi = 22.5` なので `bmi < 18.5` は `false`。
- `bmi < 25.0` は `true` なので `"普通体重"` と `"健康的な体重です"` が出力されます。
- `else if (bmi < 25.0)` は「18.5 以上」の条件を含んでいます。`bmi < 18.5` の `else` として実行されるため、自動的に `18.5 以上` が保証されます。

</details>
