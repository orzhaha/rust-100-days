## 流程控制-if

利用布林值來決定如何繼續執行程式進行決策

例

```rust
let n = 3;

if n > 2 {
    println!("執行")
}
```

if 跟其他語言差不多，if關鍵字後面布林求值稱為條件只有當true才會執行大括號裡面的語法

大括號裡面可以包含零個多個語法，用大括號包起來的稱為塊(block)

條件必須是布林類型，因此不允許下面的語法

```rust
// 不允許
if 1 {
   print!("執行");
}
```

條件不需要用小括號包起來，會出現警告

```rust
// 出現警告
if (3 > 2) {
   println!("執行")
}
```

條件之後需要一個塊(block)包起來

```rust
// 不允許
i 3 > 2 println!("執行")
```

如果要在條件為false情況執行流程可以使用else關鍵字

```rust
let n = 3;

if n > 5 {
    println!("執行")
} else 
    println!("false執行")
}

輸出
false執行
```

Rust也有類似PHP的三元運算

```rust
let n = 3;

let str = if n > 2 { "true_str" } else { "false_str" };

println!("{}", str);

輸出
true_str
```

以下範例是不允許的

```rust
// 不允許，因為無法定義str的型別
let str = if true { "true_str" }

// 不允許，因為型別不一樣，一個是字串一個是數字
let str = if true { "true_str" } else { 9527 }
```