## 所有權(二)

### 所有權與函式

將一個變數當作函式的參數傳給其他函式，怎樣安全的處理所有權

傳遞數值給函式這樣的語義和賦值給變數是類似的。傳遞變數給函式會是移動或拷貝就像賦值一樣

```rust
fn main() {
    // s被宣告
    let s = String::from("hello"); // s進入作用域

    takes_ownership(s); // s的值被當作參數傳入函式 所以可以當作s已經被移動，從這開始已經無效

    // x被宣告
    let x = 5; // x進入作用域

    makes_copy(x); // x的值被當作參數傳入函式，但x是純量型別 i32被copy，依然有效
} // 函式結束，x無效，接著是s的值已經被移動了它不會有任何動作

fn takes_ownership(some_string: String) {
    // 一個String參數some_string傳入，有效
    println!("{}", some_string);
} // 函式結束，參數some_string佔用的記憶體被釋放

fn makes_copy(some_integer: i32) {
    // 一個i32參數some_integer傳入，有效
    println!("{}", some_integer);
} // 函式結束，參數some_integer是純量型別，沒有任何動作發生
```

如果在呼叫takes_ownership之後在使用s變數在編譯時會出錯

### 回傳值與作用域

回傳值轉移所有權

```rust
fn main() {
    let s1 = gives_ownership(); // gives_ownership移動它的回傳值給s1
    let s2 = String::from("哈囉"); // s2進入作用域
    let s3 = takes_and_gives_back(s2); // s2移入takes_and_gives_back，該函式又將其回傳值移到s3
} // s3 在此離開作用域並釋放
  // s2 已被移走，所以沒有任何動作發生
  // s1 離開作用域並釋放

// 此函式回傳一個String
fn gives_ownership() -> String {
    let some_string = String::from("hello"); // some_string進入作用域

    return some_string; // 回傳some_string並移動給呼叫它的函式
}

// 此函式會取得一個String然後回傳它
fn takes_and_gives_back(a_string: String) -> String {
    // a_string進入作用域
    return a_string; // 回傳a_string並移動給呼叫的函式
}
```

引用與借用在前面介紹[定義函式](https://ithelp.ithome.com.tw/articles/10269251)時有介紹過了，這邊就不多講了

講一下迷途指標(dangling pointer)，這個在很多指標語言常發生的錯誤

簡單講就是用到空指標，Rust會在編譯時檢查這類型的錯誤

例如

```rust
fn dangle() -> &String { // 回傳String的迷途引用
    let s = String::from("hello"); // 宣告一個新的String
    return &s // 回傳String的引用
} // s在此會離開作用域並釋放
```

編譯時會產生錯誤 missing lifetime specifier

這個有關於生命週期的會在下一篇講