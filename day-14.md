## Match控制流運算子

Match是使用枚舉的基本工具，類似Golang的Switch語法

Match取值後對每個條件進行比較依照順序比較，一但匹配成功就對右側求值，並結算Match語法

每個條件分支右側都必須是單個表達式

例 

前面三個都是無效的表達式

```rust
match browser {
    Firefox => let var = 777;, // 無效的表達式
    Chrome => let var = 777, // 無效的表達式
    Ie =>  fu funct() {}, // 無效的表達式
    Safari => println!("S"),
}
```

如果需要在右側好幾個表達式或是非表達式可以用塊來包起來

例

```rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let browser = Browser::Ie;

match browser {
    Browser::Firefox => {
        browser = Browser::Ie;
        println!("F");
    },
    Browser::Chrome => {
        let var = 777;
        println!("C")
    },
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

處理所有可能

```rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let browser = Browser::Ie;

match browser {
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}

// 編譯時會出
patterns `Firefox` and `Chrome` not covered
```

因為少了Firefox和Chrome條件分支，Rust要求mtch需要顯式的處理所有可能的情況

可以使用空處理

```rust
let browser = Browser::Ie;

match browser {
    Browser::Firefox => {}, // 使用空處理
    Browser::Chrome => {}, // 使用空處理
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

或是使用"_"下底線，類似Golang的Default

```rust
let browser = Browser::Ie;

match browser {
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
    _ => {},
}
```

如果Default放在條件最上面會因為順序關係什麼都不處理

```rust
let browser = Browser::Ie;

match browser {
    _ => {}, // 永遠都跑到這裡
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

### 對型別使用Match

match除了對枚舉之外也可以對一般型別

```rust
match "value" {
    "value" => println!("value"),
    _ => println!("other"),
}

match 1 {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("other"),
}
```

因為型別不像枚舉有可窮舉出所有，所以一定需要"_"(Default) 條件，不然編譯會出現錯誤