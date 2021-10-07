## 資料型別-布林值

Rust 為了表示真假值，使用關鍵字true和false

這樣的關鍵字具有非數字類型的表達式稱為布林

例

```rust
let true_var = true;
let false_var = false;
print!("{} {}", true_var, false_var);

輸出
true false
```

除了關鍵字true和false賦予值也可以透過比較運算表達式來賦予值

例

```rust
let true_var = 456 > 123 ;
let false_var = -456 >= 123;
print!("{} {} {}", true_var, false_var, -123 < 123);

輸出
true false true
```

比較運算子如下：

- ==：等於
- !=：不等於
- <：小於
- >：大於
- <=：小於或是等於
- >=：大於或是等於

跟常見語言都一樣

 

除了比較數字之外也能比較字串

例

```rust
print!("{} {} {}", "efg" < "efgh", "efg" < "efh", "D" < "d");

輸出 true true true
```

字串的比較規則就是比較兩個字串的第一個字母，然後繼續比較兩個字串相同位置的字母直到發生下列情況之一：

- 如果兩個字母都沒有其他字母了，則相等
- 如果一個字串沒有其他字母，而另一個還有其他字母，則較少字母的字串較小
- 如果兩個字串相同位置的字母比較，字母表在前的比較小，字母大寫比字母小寫小

邏輯運算子如下：

- !：not
- &&：且
- ||：或

```rust
let true_var = true;
let false_var = false;

print!("{} {}", !true_var, !false_var);
輸出
false true

print!("{} {} {} {}", true_var && true_var, false_var && false_var, true_var && false_var, false_var && true_var);
輸出
true false false false

print!("{} {} {} {}", true_var || true_var, false_var || false_var, true_var || false_var, false_var || true_var);
輸出
true false true true
```