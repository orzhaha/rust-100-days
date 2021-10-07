## 定義函式Function(一)

如果編寫多次相同的代碼，則可以把代碼封裝在一個塊中，然後為該代碼命名

通過這種方式就定義了函式，然後可以通過命名的名稱來調用該函式

要訂一個函式需要使用"fn"關鍵字後面接著函式的名稱跟圓括號然後是一個大括號塊

大括號的塊稱為函式體，函式體之前都稱為簽名

下面例子簡單使用函式

```rust
fn hello_word() {
    println!("hello word!")
}

hello_word();
hello_word();
hello_word();

輸出
hello word!
hello word!
hello word!
```

### 後定義函式

```rust
a;
let a = 5; // 非法使用變數

hello_word(); // 合法使用後定義的函式
fn hello_word() {
    println!("hello word!")
}
```

### 函式屏蔽其他函式

```rust
fn hello_word() {}
fn hello_word() {} // 多次定義函式

編譯時會出錯
the name `hello_word` is defined multiple times
```

但是可以包在塊裡多次定義fn

```rust
{
    fn hello_word() {
        println!("hello word 1")
    }

    hello_word();
}
{
    fn hello_word() {
        println!("hello word 2")
    }

    hello_word();
}

輸出
hello word 1
hello word 2
```

每個定義函式只能在塊裡面有效，下面是不合法的

```rust
{
    fn hello_word() {
        println!("hello word 2")
    }
}

hello_word();

編譯時會出錯
cannot find function `hello_word` in this scope
```

也可以在屏蔽外的塊級定義另一個函式

在main外部定義函式hello_word()，因其內部也定義了hello_word，所以永遠用不到外部的定義函式

通常編譯器會警告

```rust
fn hello_word() {
    println!("hello word 1")
}

fn main() {
    hello_word();

    {
        hello_word();
        fn hello_word() {
            println!("hello word 2")
        }
    }

    hello_word();
    fn hello_word() {
        println!("hello word 3")
    }
}

這邊輸出
hello word 3
hello word 2
hello word 3
```