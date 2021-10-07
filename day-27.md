## 並行&並發(一)

有關於並行和並發的定義每個人可能有不一樣的解釋

- 並行指的是在同一時刻，多條指令在 CPU 上同時執行
- 並發指的是在同一時間區間內，多條指令在 CPU 上同時執行

Rust以安全高效處理並行程式設計著稱，雖然沒有內建並發但並不代表沒辦法並發

網路有一些Lib可以執行並發功能

### 透過spawn建立新的執行緒

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("{}", i);
            thread::sleep(Duration::from_millis(1)); // 讓執行短暫sleep一下
        }
    });

    thread::sleep(Duration::from_millis(5)); // 讓執行短暫sleep一下
}
```

透過範例可以看到印出來並不是如預期的都會把1-10印出來，主要是因為main已經執行完退出造成新建的執行緒還沒跑完就被中斷了，雖然可以透過Sleep讓main晚點退出但是有更好的作法

### 使用join等待所有執行緒完成

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("{}", i);
            thread::sleep(Duration::from_millis(1)); // 讓執行短暫sleep一下
        }
    });

    handle.join().unwrap(); // (Blocking)讓全部執行完才繼續往下
}
```

透過儲存thread::spawn回傳的數值為變數，可以修正產生的執行緒完全沒有執行或沒有執行完成的問題。handle呼叫join方法來確保產生的執行緒會在main離開之前完成

### 透過執行緒使用move閉包

閉包透過move讓thread::spawn執行緒可以使用其他執行緒的資料。

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || { // 透過move讓執行緒可以擁有v變數的所有權
        println!("{:?}", v);
    }); // 在這個執行緒執行完之前v所有權都不會釋放

    handle.join().unwrap();
}
```