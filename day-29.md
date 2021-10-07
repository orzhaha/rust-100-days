## 錯誤處理

Rust將錯誤分成兩大類

- 不可復原的(unrecoverable)
- 可復原的(recoverable)

至於什麼時候該用什麼樣的錯誤就要看使用情境了

例如程式啟動時讀不到設定檔這個就可以使用不可復原的錯誤

### 不可復原的錯誤使用panic!

```rust
fn main() {
    panic!("恐慌性錯誤");
}

輸出
thread 'main' panicked at '恐慌性錯誤', main.rs:2:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

panic會印出錯誤訊息以及第幾行的訊息，還有提示可以設定RUST_BACKTRACE環境變數顯示錯誤回朔資訊

下面執行時增加環境變數RUST_BACKTRACE=1

```rust
RUST_BACKTRACE=1 ./main

輸出
thread 'main' panicked at '恐慌性錯誤', main.rs:2:5
stack backtrace:
   0: std::panicking::begin_panic
   1: main::main
   2: core::ops::function::FnOnce::call_once
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

這時候可以看到簡略的錯誤，也可以設定RUST_BACKTRACE=full會列出整個過程

Rust的panic不像Golang可以recover回來

### 可復原的的錯誤使用Result

Resut是個枚舉型別

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Reust是枚舉型別就可以透過match來判斷是回傳OK還是回傳Err，再依裡面的泛型參數取值

```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");

    let _ = match f {
        Ok(file) => {
            println!("{:?}", file)
        }
        Err(error) => {
            println!("{:?}", error)
        }
    };
}

輸出
Os { code: 2, kind: NotFound, message: "No such file or directory" }
```

如果覺得match判斷錯誤太麻煩，也可以透過unwrap或expect來直接產生panic錯誤

```rust
let f = File::open("hello.txt").unwrap(); // 無法指定錯誤訊息
let f = File::open("hello.txt").expect("開啟hello.txt錯誤"); // 可以指定錯誤訊息
```

也可以在寫函式時回傳Result型別，這邊簡單定義Result泛型型別

```rust
fn main() {
    let ok = return_result_ok();
    println!("{:?}", ok);
    let error = return_result_error();
    println!("{:?}", error);
}

fn return_result_ok() -> Result<String, String> {
    let s = String::from("成功");
    return Ok(s);
}

fn return_result_error() -> Result<String, String> {
    let s = String::from("失敗");
    return Ok(s);
}
```