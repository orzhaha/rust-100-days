## 枚舉(enumeration)

枚舉就是列出有窮序列的型別

透過enum關鍵字新增了新的Browser型別在範例中列出了一個組項分別為

Firefox，Chrome，IE，Safari內部值分別為0u8，1u8，2u8，3u8表示

通常都是以整數為內部關聯值

透過枚舉寫以下的代碼

```rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

let browser = Browser::Ie;

match browser {
    Browser::Firefox => println!("F"),
    Browser::Chrome => println!("C"),
    Browser::Ie => println!("I"),
    Browser::Safari => println!("S"),
}
```

不要編寫這樣的代碼

```rust
const FIREFOX: u8 = 0;
const CHROME: u8 = 1;
const IE: u8 = 2;
const SAFARI: u8 = 3;

let browser = IE;

if browser == FIREFOX {
    println!("F");
} else if browser == CHROME {
    println!("C");
} else if browser == IE {
    println!("I");
} else if browser == SAFARI {
    println!("S");
}
```

### 再怎樣也不要寫[magic number](https://zh.wikipedia.org/wiki/%E9%AD%94%E8%A1%93%E6%95%B8%E5%AD%97_(%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88))

### 如何使用枚舉

使用Use的方式

```rust
// 顯式的指定要使用的枚舉
use Browser::{Chrome, Firefox, Ie, Safari};

let browser = Ie;

match browser {
    Firefox => println!("F"),
    Chrome => println!("C"),
    Ie => println!("I"),
    Safari => println!("S"),
}

// 自動的使用Browser內部所有的枚舉
use Browser::*;

let browser = Ie;

match browser {
    Firefox => println!("F"),
    Chrome => println!("C"),
    Ie => println!("I"),
    Safari => println!("S"),
}
```

使用帶有C風格的用法

```rust
enum Browser {
    Firefox,
    Chrome,
    Ie,
    Safari,
}

println!("{}", Browser::Firefox as i32)
```

枚舉不能使用"=="運算子做比較

```rust
let browser = Browser::Ie;

if browser == Browser::Ie {
    println!("{}", "hello word")
}

// 編譯時會出錯
// binary operation `==` cannot be applied to type `Browser`
```