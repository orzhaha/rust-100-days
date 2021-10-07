## 資料型別-字元.字串

Rust的char型別是最基本的字母型別，用單引號包起來

例

```rust
let a = 'b';
```

Rust的字串分兩種

- str
- String

嚴格來講Rust在核心語言中只有一個字串型別，哪就是切片字串(&str)是不可變的(stack)

String則是Rust標準函式庫提供的型別，String是可變(Heap)

例

```rust
// &str型別
let str_var = "字串";

// String型別
let string_var = str_var.to_string();
// String型別
let string_var = String::from("字串");
```

索引字串切片

因為字串是使用 UTF-8 編碼，每個字符可能有多個位元組所以用字串索引會容易出現錯誤

```rust
let str_var = "安安";

println!("{}", &str_var[0..1]);

會出現錯誤
thread 'main' panicked at 'byte index 1 is not a char boundary; it is inside '安' (bytes 0..3) of `安安`'
```

比較好得做法透過as_bytes或是chars方法來取直

```rust
let str_var = "安安你好啊";

for b in str_var.as_bytes() {
    print!("{}, ", b);
}

for c in str_var.chars() {
    print!("{}, ", c);
}

輸出
229, 174, 137, 229, 174, 137, 228, 189, 160, 229, 165, 189, 229, 149, 138,
安, 安, 你, 好, 啊,
```

字串相加，這邊會使用到&借用，後面會講解

```rust
let string1_var = String::from("安安");
let string2_var = String::from("你好啊");

println!("{}", string1_var + &string2_var);

輸出
安安你好啊
```

Rust沒有提供型別判斷的方法，如果想要知道變數是什麼型別可以利用編譯時的錯來查看

例

```rust
// char型別
let char_var = 'c';
char_var.not_found;

出現錯誤
`char` is a primitive type and therefore doesn't have fields

// &str型別
let str_var = "str";
str_var.not_found;

出現錯誤
no field `not_found` on type `&str`
```
