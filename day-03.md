## Hello World

先建立一個hello的目錄，編輯main.rs

```rust
fn main() {
    print!("Hello, World!");
}
```

儲存然後在Terminal用rustc編譯

```rust
rustc main.rs
```

會產生出main檔案，執行main就會出現Hello, world!了

```bash
./main
Hello, World!
```

### 分析程式碼

- **fu**是rust的關鍵字，function的簡寫
- main是函式名稱，執行rust程式時將執行，如果沒有main哪可能是一個library
- print! 是rust的巨集(macro) 如果沒有"!"表示是函示有的話則是巨集
- "Hello, world!" 表示字串內容
- 最後用";"表示語法結束

rust對大小寫是敏感的

 

### Print! 用法

利用"{}"站位符輸出字串，類似golang的fmt.Printf("%s, %s!", "Hello", "World")

```rust
print!("{}, {}!", "Hello", "World")

輸出
Hello, World!

```

利用"{}"站位符輸出數字，類似golang的fmt.Printf("%s: %d", "Num", 9527)

```rust
print!("{}: {}", "Num", 9527)

輸出
Num: 9527
```

用"\n"輸出多行字串

```rust
print!("Hello, World!\nHello, World!\nHello, World!")

輸出
Hello, World!
Hello, World!
Hello, World!
```