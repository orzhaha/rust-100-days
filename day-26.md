## 特徵(Trait)(二)

### 沒有Trait界限的泛型函式

上一篇範例中在宣告泛型函式中使用了where

```rust
where
    T: HasSqrt,
{
```

在泛型函式的宣告中如果沒有where的子句引用類型參數，則該型別就沒有與任何trait關聯

因此只能對該泛型型別的物件做很少的事情

例如

```rust
fn f<T>(x: T) -> T {
    let c: T = x;
    let mut d = c;
    f = f2(d)
    return f
}
```

使用無界限的類型參數"T"只能

- 通過值或引用將其作函式參數傳遞
- 通過值或引用得從函式返回
- 局部變數宣告或是初始化

### 多函式Trait

Trait也可以包含多個函式例如上一篇的範例如果要再增加絕對值abs的函式可以這樣寫

```rust
fn main() {
    trait HasSqrt {
        fn sq(self) -> Self;
        fn abs(self) -> Self;
    }

    impl HasSqrt for f32 {
        fn sq(self) -> Self {
            return f32::sqrt(self);
        }

        fn abs(self) -> Self {
            return f32::abs(self);
        }
    }

    impl HasSqrt for f64 {
        fn sq(self) -> Self {
            return f64::sqrt(self);
        }

        fn abs(self) -> Self {
            return f64::abs(self);
        }
    }

    fn quartic<T>(x: T) -> T
    where
        T: HasSqrt,
    {
        return x.abs.sq().sq();
    }

    let qr32 = quartic(100f32);
    let qr64 = quartic(100f64);

    println!("{} {}", qr32, qr64);
}
```

有時候可能只會需要平方根函式不需要絕對值函式這時候就可以抽兩個Trait會更靈活

```rust
fn main() {
    trait HasSqrt {
        fn sq(self) -> Self;
    }

    trait HasAbs {
        fn abs(self) -> Self;
    }

    impl HasSqrt for f32 {
        fn sq(self) -> Self {
            return f32::sqrt(self);
        }
    }

    impl HasAbs for f32 {
        fn abs(self) -> Self {
            return f32::abs(self);
        }
    } 

    impl HasSqrt for f64 {
        fn sq(self) -> Self {
            return f64::sqrt(self);
        }
    }

    impl HasAbs for f64 {
        fn abs(self) -> Self {
            return f64::abs(self);
        }
    }

    fn quartic<T>(x: T) -> T
    where
        T: HasSqrt + HasAbs , // 這裡界限宣告兩個trait
    {
        return x.abs.sq().sq();
    }

    let qr32 = quartic(100f32);
    let qr64 = quartic(100f64);

    println!("{} {}", qr32, qr64);
}
```

### "self"和"Self"傻傻分不清楚

Rust式區分大小寫得

在前面的範例都使用到self和Self其中

- 小寫開頭的self表示函式的值
- 大寫開頭的Self標示self的型別

"self"和"Self"只能在trait或impl範圍裡面使用並且如果self必須是是方法的第一個參數

```rust
// 這幾個範例是等效的
fn f(self) -> Self
fn f(self: Self) -> Self
fn f(self: i32) -> Self
fn f(self) -> i32
fn f(self: Self) -> i32
fn f(self: i32) -> i32
```

### 預設實作

可以針對Trait預設行為，不必要求每個型別都要實作方法，當然也可以覆蓋這個預設的方法

```rust
fn main() {
    trait HasSqrt {
        fn sq(self) -> Self;
        fn helloWord(&self) -> String { // 預設實作
            return String::from("hello word");
        }
    }

    impl HasSqrt for f32 {
        fn sq(self) -> Self {
            return f32::sqrt(self);
        }
    }

    fn quartic<T>(x: T) -> T
    where
        T: HasSqrt,
    {
        println!("{}", x.helloWord());
        return x.sq().sq();
    }

    let qr32 = quartic(100f32);
}
```