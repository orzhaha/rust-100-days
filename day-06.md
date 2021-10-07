## 資料型別-整數浮點數

Rust是靜態型別語言，所以在編譯時需要知道變數的型別是什麼

前面的程式範例很多是沒有宣吿型別但是卻可以編譯，這邊用到的是透過

通常編譯器能通過數值來推導型別是什麼

Rust 有四種主要純量型別：整數、浮點數、布林以及字元

### 整數型別

整數是沒有小數點的數字，分有帶號(signed)跟非帶號(unsigned)，差別就是一個有負值一個沒有負值

帶號範圍-(2^n - 1) 到 (2^n - 1) - 1

非帶號範圍0到2^n - 1

isize跟usize則是依據運行環境的電腦是32位元還是還64位元決定大小

Rust預設整數型別是i32

[Untitled](https://www.notion.so/9de683ba9f094d0e909ec7058ee15b3b)

### 溢位問題

Rust在執行時會檢查是否有溢位

```rust
let mut n: i32 = i32::max_value();
 
// Overflow
n = n + 1;

出現panic錯誤
thread 'main' panicked at 'attempt to add with overflow'
```

如果你想要讓溢位也視為正常的可以在編譯時增加參數

```rust

// rustc用法
rustc -O main.rs

// cargo用法
cargo build --release

```

### 浮點數型別

浮點數只有兩種型別

- f32 32位元大小
- f64 64位元大小

浮點數是依照 IEEE-754

Rust預設浮點數型別是f64