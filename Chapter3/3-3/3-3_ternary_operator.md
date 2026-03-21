# 第3章：条件分岐

## 3-3. 三項演算子

---

## 📖 解説

### 三項演算子とは？

三項演算子は **`if / else` を1行で書ける演算子**です。  
条件式・真の値・偽の値の **3つの要素**を使うため「三項」と呼ばれます。

```
条件式 ? 真のときの値 : 偽のときの値
```

```java
// if / else で書いた場合
int score = 75;
String result;
if (score >= 60) {
    result = "合格";
} else {
    result = "不合格";
}

// 三項演算子で書いた場合（同じ意味）
String result = score >= 60 ? "合格" : "不合格";
```

---

### 基本的な使い方

```java
int a = 10;
int b = 20;

int max = a > b ? a : b;
System.out.println("大きい方: " + max);  // 大きい方: 20

String label = a % 2 == 0 ? "偶数" : "奇数";
System.out.println(a + "は" + label);    // 10は偶数
```

---

### 三項演算子のネスト

三項演算子の中にさらに三項演算子を入れることができます。  
ただし、深くなると読みにくくなるので **2段階まで**が目安です。

```java
int score = 75;

String grade = score >= 80 ? "A" :
               score >= 60 ? "B" : "C";

System.out.println(grade);  // B
```

> ⚠️ ネストが深い場合は `if / else if` を使う方が読みやすいです。

---

### 三項演算子の使いどころ

| 向いているケース | 向いていないケース |
|---|---|
| 1行で完結するシンプルな分岐 | 処理が複数行にわたる場合 |
| 変数への代入 | 副作用のある処理（メソッド呼び出しなど） |
| 出力の中に埋め込む | 条件が複雑で読みにくい場合 |

```java
int age = 20;

// ✅ 向いている：変数代入・出力埋め込み
String type   = age >= 18 ? "成人" : "未成年";
System.out.println("区分: " + (age >= 18 ? "成人" : "未成年"));

// ❌ 避けるべき：処理が複雑でネストが深い
String result = age >= 18 ? (age >= 65 ? "高齢者" : "成人") : (age >= 13 ? "中高生" : "子ども");
```

---

### コード例

```java
public class TernaryExample {
    public static void main(String[] args) {
        int temperature = 32;
        int stock = 0;
        boolean isMember = true;

        // 気温による判定
        String weather = temperature >= 30 ? "暑い" : "涼しい";
        System.out.println("今日は" + weather);              // 今日は暑い

        // 在庫確認
        String stockStatus = stock > 0 ? "在庫あり" : "在庫切れ";
        System.out.println("商品: " + stockStatus);          // 商品: 在庫切れ

        // 会員割引
        int price = 1000;
        int finalPrice = isMember ? price * 9 / 10 : price;
        System.out.println("価格: " + finalPrice + "円");    // 価格: 900円
    }
}
```

**出力結果：**
```
今日は暑い
商品: 在庫切れ
価格: 900円
```

---

## ✏️ 問題

### 問題 3-3-1（出力結果を答える）　⭐☆☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q1 {
    public static void main(String[] args) {
        int x = 8;
        int y = 3;

        String result1 = x > y ? "xの方が大きい" : "yの方が大きい";
        int    result2 = x % 2 == 0 ? x * 2 : x + 1;
        String result3 = y > 5 ? "5より大きい" : "5以下";

        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
    }
}
```

<details>
<summary>答えを見る</summary>

```
xの方が大きい
16
5以下
```

**解説：**
- `x > y` は `8 > 3` → `true` なので `"xの方が大きい"`。
- `x % 2 == 0` は `8 % 2 == 0` → `true` なので `x * 2 = 16`。
- `y > 5` は `3 > 5` → `false` なので `"5以下"`。

</details>

---

### 問題 3-3-2（出力結果を答える）　⭐⭐☆

次のコードを実行したとき、出力される結果を答えてください。

```java
public class Q2 {
    public static void main(String[] args) {
        int score = 70;

        String grade = score >= 80 ? "A" :
                       score >= 60 ? "B" : "C";

        System.out.println("評価: " + grade);

        int n = 15;
        String fizzbuzz = n % 15 == 0 ? "FizzBuzz" :
                          n % 3  == 0 ? "Fizz"     :
                          n % 5  == 0 ? "Buzz"     : String.valueOf(n);

        System.out.println(n + " → " + fizzbuzz);
    }
}
```

<details>
<summary>答えを見る</summary>

```
評価: B
15 → FizzBuzz
```

**解説：**
- `score = 70` は `>= 80` を満たさず、`>= 60` を満たすので `"B"`。
- `n = 15` は `15 % 15 == 0` → `true` なので最初の条件で `"FizzBuzz"` が返されます。

</details>

---

### 問題 3-3-3（コード記述）　⭐⭐⭐

次の条件を満たす `TicketPricing` クラスを作成してください。

**条件：**
- `int` 型の変数 `age` に `14` を代入する
- `boolean` 型の変数 `isWeekend` に `true` を代入する
- 以下のルールを**三項演算子のみ**を使って実装する

**料金ルール：**

| 条件 | 料金 |
|---|---|
| age が 12 未満 | 500円 |
| age が 12 以上 18 未満 | 1000円 |
| age が 18 以上 | 1800円 |

- 上記の料金を `basePrice` に代入する（ネストした三項演算子を使う）
- `isWeekend` が `true` のとき、`basePrice` に200円加算した値を `finalPrice` にする
- `isWeekend` が `false` のとき、`basePrice` をそのまま `finalPrice` にする

**期待される出力：**
```
年齢: 14歳
基本料金: 1000円
週末料金加算: あり
最終料金: 1200円
```

<details>
<summary>答えを見る</summary>

```java
public class TicketPricing {
    public static void main(String[] args) {
        int age = 14;
        boolean isWeekend = true;

        int basePrice  = age < 12 ? 500 :
                         age < 18 ? 1000 : 1800;

        int finalPrice = isWeekend ? basePrice + 200 : basePrice;

        System.out.println("年齢: "       + age + "歳");
        System.out.println("基本料金: "   + basePrice  + "円");
        System.out.println("週末料金加算: " + (isWeekend ? "あり" : "なし"));
        System.out.println("最終料金: "   + finalPrice + "円");
    }
}
```

**解説：**
- `age = 14` は `< 12` を満たさず、`< 18` を満たすので `basePrice = 1000`。
- `isWeekend = true` なので `finalPrice = 1000 + 200 = 1200`。
- 出力行でも三項演算子を使い `"あり" / "なし"` を切り替えています。

</details>
