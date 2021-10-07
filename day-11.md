## 流程控制-while

類似PHP的while迴圈，計算其後的布林條件如果是值為true則執行大括號下面的語法，會重複條件的檢查執行直到條件值為false為止或是其他原因退出

假設要打印1到100的整數可以使用下面的語法來達到

```rust
let mut i = 1;

while i <= 100 {
    print!("{} ", i);
    i += 1;
}

輸出
1 2 3 4 5 ... 100
```

雖然Rust沒有do while的語法但也有break contiune語法

例如只想印出雙數

```rust
let mut i = 1;

 while i <= 100 {
     i += 1;
     if i % 2 != 0 {
        continue;
     }
     print!("{} ", i);
}

輸出
2 4 6 8 10 ... 100
```

或是碰到50的值就中斷退出

```rust
let mut i = 1;

while i <= 100 {
    if i == 50 {
        break;
    }
    print!("{} ", i);
		i += 1;
}

輸出
1 2 3 4 5 ... 49
```

### 無限循環(loop)

如果要執行無限循環的迴圈直到程式被強制中斷或是通過退出循環語法break，可以透過loop語法

```rust
// 透過whilce 會出現警告
let mut i = 1;

while true {
    if i == 50 {
        break;
    }
    i += 1;
}

// 透過loop
let mut i = 1;

loop {
    if i == 50 {
         break;
    }
    i += 1;
}

```