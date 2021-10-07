## 命令行的輸入輸出

### 命令行參數

一般來說編譯好的執行檔都是透過命令行來制執有些時候需要讀取一些命令行參數或是環境參數

程式輸入的最基本形式事示通命令行

例如下面

```rust
for arg in  std::env::args() {
    println!("{}", arg)
}
```

這時候編譯好的執行檔名稱如果是main在命令行執行

```rust
./main arg1 arg2 arg3

輸出
./main
arg1
arg2
arg3
```

args標準庫函式會返回命令行參數上的跌代器類別是Args並生成String值

第一個是程式名稱包含路徑其餘都是參數指令

### 環境變數

輸入輸出的另一種形式通過環境變數

```rust
for env in std::env::vars() {
    println!("{:?}", env)
}

輸出
("CARGO_HOME", "/playground/.cargo")
**("CARGO_MANIFEST_DIR", "/playground")
("CARGO_PKG_AUTHORS", "The Rust Playground")
("CARGO_PKG_DESCRIPTION", "")
("CARGO_PKG_HOMEPAGE", "")
("CARGO_PKG_LICENSE", "")
...**
```

剛剛是把每個環境變數都印出來假如要讀取特定環境變數可以透過var函示指定key

```rust
println!("{:?}", std::env::var("envkey"));
```

既然能讀取當然也能寫入，下面就透過set_var函式來寫入環境變數

```rust
std::env::set_var("setenvkey", "env_value");

println!("{:?}", std::env::var("setenvkey"));

輸出
Ok("env_value")
```

### 命令行的輸入

可以在程式啟動後獲取從鍵盤輸入的一行字直到用戶按下Enter鍵在輸出

```rust
let mut line = String::new();

std::io::stdin().read_line(&mnt line);

println!("{}", line);
```