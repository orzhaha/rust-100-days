## 並行&並發(二)

### channel

通常channel都是搭配並行使用，沒有使用並行就沒有使用channel的意義

「別透過共享記憶體來溝通，而是透過溝通來共享記憶體」。沒錯Golang的口號在Rust也是通用

Rust標準函式庫也有提供類似Golang的Channel函式

Channel函式會回傳兩個變數分別為發送者(transmitter)與接收者(receiver)

Channel可以多個發送者(transmitter)，單一接收者(receiver)

藉由這兩變數來傳遞訊息

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel(); // 建立新的channel，回傳一個元組，拆成兩個變數

    thread::spawn(move || {
        let val = String::from("hello word");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap(); // (Blocking)等到有資料才會繼續往下 
    println!("{}", received);
}
```

channel如果要在同一個通道多個執行緒使用發送者(transmitter)時，會因為所有權的關係造成錯誤，所以可以用clone來製造多個發送者(transmitter)

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();
    let tx1 = tx.clone(); // 複製發送者
    let tx2 = tx.clone(); // 複製發送者
    thread::spawn(move || {
        let val = String::from("hello word tx");
        tx.send(val).unwrap();
    });

    thread::spawn(move || {
        let val = String::from("hello word tx1");
        tx1.send(val).unwrap();
    });

    thread::spawn(move || {
        let val = String::from("hello word tx2");
        tx2.send(val).unwrap();
    });

    for received in rx {
        println!("{}", received);
    }
}

輸出
hello word tx
hello word tx2
hello word tx1
```

雖然channel本身不支援多個接收者(receiver)，但是可以利用上鎖讓多個執行序同時使用接收者