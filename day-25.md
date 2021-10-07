## 特徵(Trait)(一)

### 什麼是特徵

根據官網的解釋就是

特徵會告訴編譯器特定型別與其他型別共享的功能。可以使用特徵定義來抽象出共同行為。可以使用特徵界限(trait bounds)來指定泛型型別為擁有特定行為的任意型別。

簡單講就是其他語言中的介面(interface)，只是有少許不同而已

### 特徵的需求

假設我們需要一個函式需計算四次平方根，可以用標準庫sqrtx來寫

```rust
fn main() {
    fn quartic(x: f64) -> f64 {
        return x.sqrt().sqrt();
    }

    let qr = quartic(100.);

    println!("{} {}", qr * qr * qr * qr, qr);
}

輸出
100.00000000000003 3.1622776601683795
```

但假如要還需要一個計算32位元浮點數的四次平方根又要再寫一個quarticf32函式

```rust
fn quarticf64(x: f64) -> f64 {
    return x.sqrt().sqrt();
}

fn quarticf32(x: f32) -> f32 {
    return x.sqrt().sqrt();
}
```

這時候可能會寫一個泛型函式來取代前面兩個函式

```rust
fn main() {
    fn quartic<T>(x: T) -> T {
        return x.sqrt().sqrt();
    }

    let qrf32 = quartic(100f32);
		let qrf64 = quartic(100f64);

    println!("{} {}", qrf32, qrf64);
}

會出現編譯錯誤
no method named `sqrt` found for type parameter `T` in the current scope
```

會出現這樣的錯誤是因為x變數是屬於泛型型別T，是剛剛在建立出來的並沒有sqrt的mehtod

這時候就可以特徵派上用場的時候了，可以這樣使用解決剛剛的問題

```rust
fn main() {
    trait HasSqrt {
        fn sq(self) -> Self;
    }

    impl HasSqrt for f32 {
        fn sq(self) -> Self {
            return f32::sqrt(self);
        }
    }

    impl HasSqrt for f64 {
        fn sq(self) -> Self {
            return f64::sqrt(self);
        }
    }

    fn quartic<T>(x: T) -> T
    where
        T: HasSqrt,
    {
        return x.sq().sq();
    }

    let qr32 = quartic(100f32);
    let qr64 = quartic(100f64);

    println!("{} {}", qr32, qr64);
}

輸出
3.1622777 3.1622776601683795
```

一開先宣告HasSqrt trait然後使用impl關鍵字替f32和f64實現sq函式