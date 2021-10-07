## 定義Closure(閉包)

一般來說Rust如果要排序數組會這樣寫

```rust
let mut arr = [10, 5, 9, 7, 6]

arr.sort();

println!("{:?}", arr);

輸出
[5, 6, 7, 9, 10]
```

剛剛升序如果要降序就必須使用sort_by 需要自己寫函式來傳給sort_by排序決定用

```rust
let mut arr = [10, 5, 9, 7, 6]

use std::cmp::Ordering;

fn descFn(a: &i32, b: &i32) -> Ordering {
    if  a < b {
        return Ordering::Greater;
    } else if a > b  {
        return Ordering::Less
    } else {
        return Ordering::Equal
    }
}

arr.sort_by(descFn);

println!("{:?}", arr);

輸出
[10, 9, 7, 6, 5]
```

造上面這樣使用有個缺點就是必須在宣告一個函式，如果只用一次的話就可用更簡短的使用匿名函式

```rust
let mut arr = [10, 5, 9, 7, 6]

use std::cmp::Ordering;

arr.sort_by(|a: &i32, b: &i32|
    if  a < b {
        return Ordering::Greater;
    } else if a > b  {
        return Ordering::Less
    } else {
        return Ordering::Equal
    }
);

println!("{:?}", arr);
輸出
[10, 9, 7, 6, 5]
```

使用一個匿名函式就可以簡短許多不用再宣告新的函式

Rust跟一般語言的匿名函式有個特別的限制就是無法使用外部宣告的變數

例如這樣是不合法的

```rust
let outVal = 10;

fn printOutVal() {
    println!("{}", outVal); // 無法訪問outVal變數
}

printOutVal();

// 編譯時會出現錯誤
```

但是使用靜態變數或是常量例如

```rust
const CONSTVAL: i32 = 10;

fn printOutVal() {
    println!("{}", CONSTVAL); // 合法使用
}

printOutVal();

-----------------------------

static STATICVAL: i32 = 0;

fn printOutVal() {
    println!("{}", CONSTVAL); // 合法使用
}

printOutVal();
```

Rust這樣限制應該基於所有權的設計關係，這樣有個好處就是避免亂引用變數造成BUG也比較好閱讀

當然Closure有更多複雜的使用這邊只簡單介紹最基本的使用方法