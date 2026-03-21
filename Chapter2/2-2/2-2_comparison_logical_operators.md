# 第2章：演算子

## 2-2. 比較演算子・論理演算子

---

## 📖 解説

### 比較演算子とは？

2つの値を比較し、結果を **`true` または `false`（boolean型）** で返す演算子です。  
主に `if` 文や繰り返し処理の条件式で使います。

| 演算子 | 意味 | 例 | 結果 |
|---|---|---|---|
| `==` | 等しい | `5 == 5` | `true` |
| `!=` | 等しくない | `5 != 3` | `true` |
| `>` | より大きい | `5 > 3` | `true` |
| `<` | より小さい | `5 < 3` | `false` |
| `>=` | 以上 | `5 >= 5` | `true` |
| `<=` | 以下 | `5 <= 4` | `false` |

```java
int a = 10;
int b = 20;

System.out.println(a == b);  // false
System.out.println(a != b);  // true
System.out.println(a < b);   // true
System.out.println(a >= 10); // true
```

---

### 論理演算子とは？

複数の条件を組み合わせるときに使う演算子です。結果は `boolean` 型になります。

| 演算子 | 意味 | 説明 |
|---|---|---|
| `&&` | AND（かつ） | 両方が `true` のときだけ `true` |
| `\|\|` | OR（または） | どちらか一方が `true` なら `true` |
| `!` | NOT（否定） | `true` → `false`、`false` → `true` |

---

### AND（&&）の真理値表

| A | B | A && B |
|---|---|---|
| `true` | `true` | `true` |
| `true` | `false` | `false` |
| `false` | `true` | `false` |
| `false` | `false` | `false` |

```java
int age = 20;
boolean hasTicket = true;

System.out.println(age >= 18 && hasTicket);  // true  （両方満たす）
System.out.println(age >= 18 && !hasTicket); // false （チケットなし）
```

---

### OR（||）の真理値表

| A | B | A \|\| B |
|---|---|---|
| `true` | `true` | `true` |
| `true` | `false` | `true` |
| `false` | `true` | `true` |
| `false` | `false` | `false` |

```java
int score = 55;

System.out.println(score >= 60 || score < 0); // false （どちらも満たさない）
System.out.println(score >= 50 || score < 0); // true  （前半が true）
```

---

### NOT（!）

条件の真偽を反転させます。

```java
boolean isRaining = false;

System.out.println(isRaining);   // false
System.out.println(!isRaining);  // true  ← 反転
```

---

### 短絡評価（ショートサーキット）

`&&` と `||` は**短絡評価**を行います。

- `&&`：左辺が `false` なら右辺は評価しない
- `||`：左辺が `true` なら右辺は評価しない

```java
int x = 0;

// x != 0 が false なので右辺（10/x）は評価されない → ゼロ除算が起きない
System.out.println(x != 0 && 10 / x > 1);  // false（安全）
```

---

### コード例

```java
public class LogicExample {
    public static void main(String[] args) {
        int score = 75;
        boolean isPassed = score >= 60;
        boolean isHighScore = score >= 90;

        System.out.println("合格: "     + isPassed);                    // true
        System.out.println("高得点: "   + isHighScore);                 // false
        System.out.println("合格かつ高得点: " + (isPassed && isHighScore)); // false
        System.out.println("合格または高得点: " + (isPassed || isHighScore)); // true
        System.out.println("不合格: "   + !isPassed);                   // false
    }
}
```

**出力結果：**
```
合格: true
高得点: false
合格かつ高得点: false
合格または高得点: true
不合格: false
```

---

## ✏️ 問題

### 問題 2-2-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int x = 15;
        int y = 15;
        int z = 20;

        System.out.println(x == y);
        System.out.println(x == z);
        System.out.println(x != z);
        System.out.println(z > x);
        System.out.println(x >= y);
    }
}
```

<details>
<summary>答えを見る</summary>

```
true
false
true
true
true
```

**解説：**
- `x == y` は `15 == 15` なので `true`。
- `x == z` は `15 == 20` なので `false`。
- `x != z` は `15 != 20` なので `true`。
- `z > x` は `20 > 15` なので `true`。
- `x >= y` は `15 >= 15`（以上なので同値もOK）なので `true`。

</details>

---

### 問題 2-2-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int age = 22;
        boolean hasMembership = false;
        int score = 85;

        System.out.println(age >= 20 && hasMembership);
        System.out.println(age >= 20 || hasMembership);
        System.out.println(!hasMembership);
        System.out.println(score >= 80 && score < 90);
        System.out.println(score < 60 || score >= 80);
    }
}
```

<details>
<summary>答えを見る</summary>

```
false
true
true
true
true
```

**解説：**
- `age >= 20 && hasMembership` → `true && false` → `false`。
- `age >= 20 || hasMembership` → `true || false` → `true`。
- `!hasMembership` → `!false` → `true`。
- `score >= 80 && score < 90` → `true && true` → `true`（80点台）。
- `score < 60 || score >= 80` → `false || true` → `true`。

</details>

---

### 問題 2-2-3（コード記述）　⭐⭐⭐

次の条件を満たす `EntryChecker` クラスを作成してください。

**条件：**
- `int` 型の変数 `age` に `17` を代入する
- `int` 型の変数 `height` に `160` を代入する
- `boolean` 型の変数 `hasGuardian` に `true` を代入する
- 以下の判定をすべて出力する

| 判定内容 | 条件 |
|---|---|
| 成人かどうか | age が 18 以上 |
| 身長制限クリア | height が 150 以上 |
| 単独入場可 | age が 18 以上 かつ height が 150 以上 |
| 保護者同伴で入場可 | (age が 13 以上 かつ height が 150 以上) または hasGuardian が true |
| 入場不可 | 単独入場可 でも 保護者同伴で入場可 でもない |

**期待される出力：**
```
成人: false
身長制限クリア: true
単独入場可: false
保護者同伴で入場可: true
入場不可: false
```

<details>
<summary>答えを見る</summary>

```java
public class EntryChecker {
    public static void main(String[] args) {
        int age = 17;
        int height = 160;
        boolean hasGuardian = true;

        boolean isAdult        = age >= 18;
        boolean heightOk       = height >= 150;
        boolean canEnterAlone  = isAdult && heightOk;
        boolean canEnterGuard  = (age >= 13 && heightOk) || hasGuardian;
        boolean cannotEnter    = !canEnterAlone && !canEnterGuard;

        System.out.println("成人: "           + isAdult);
        System.out.println("身長制限クリア: "  + heightOk);
        System.out.println("単独入場可: "      + canEnterAlone);
        System.out.println("保護者同伴で入場可: " + canEnterGuard);
        System.out.println("入場不可: "        + cannotEnter);
    }
}
```

**解説：**
- `age = 17` なので `isAdult` は `false`。
- `height = 160` なので `heightOk` は `true`。
- `canEnterAlone` は `false && true` → `false`。
- `canEnterGuard` は `(true && true) || true` → `true`。
- `cannotEnter` は `!false && !true` → `true && false` → `false`。

</details>
