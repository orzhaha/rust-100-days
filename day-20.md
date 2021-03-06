## 定義泛型結構

既然有泛型函數當然少不瞭泛型結構

```rust
struct S1<T1, T2> {
   n1: T1,
   n2: T2,
}

let s = S {n1:957, n2:996.1}
```

第一句語法宣告了兩個型別T1和T2參數化的泛型結構S1，第一個字段n1為T1第二字段n2為T2

第二句語法n1字段的參數隱式的替換成i32，n2字段的參數隱式替換成f64

也可以使用泛型元組結構

```rust
struct SE<T1, T2> (T1, T2)

let se = SE (957, 996.1)
```

### 泛型機制

通過下面程式碼來理解編譯泛型機制概念

```rust
fn swap<T1, T2>(p1: T1, p2: T2) -> (T2, T1) {
    return (p2, p1);
}

let x = swap(5i16, 9u16);
let y = swap(6f32, true);

println!("{:?} {:?}", x, y);

輸出
(9, 5) (true, 6.0)
```

第一階段編譯器會掃描程式碼並且每次找到泛型函式宣告(swap)時，它在資料結構中加載該函式內部表示形式檢查泛型程式碼是否有語法錯誤

第二階段編譯器再次掃描程式碼在每次遇到泛型函式調用時，編譯器都會檢查此類用法與泛型宣告的相應內部表示關聯，在確認後再其資料結構中加載這種對應關係

第三階段再將掃描所有泛型函式調用，對於每個泛型函式調用者都定義每個泛型參數的具體型別，這種具體型別可以是在用法中顯式的或者如上面範例一樣用推斷出來的

在確定替換泛型參數的具體型別之後都會產生泛型函式的具體版本對應程式碼如下

```rust
fn swap_i16_u16(p1: i16, p2: u16) -> (u16, i16) {
    return (p2, p1);
}

fn swap_f32_bool(p1: f32, p2: bool) -> (f32, bool) {
    return (p2, p1);
}

let x = swap_i16_u16(5i16, 9u16);
let y = swap_f32_bool(6f32, true);

println!("{:?} {:?}", x, y);
```

泛型編譯有幾個特點

- 與非泛型程式碼相比多階段編譯相對較慢一些
- 生成的程式碼針對每個特定的調用進行高度優化，它完成使用調用者使用的型別無須進行轉換因此優化了運行時性能
- 泛型函式如果執行很多具有不同型別參數的調用，則產生大量的機器代碼，最好不要再單個函式調用太多型別會給CPU緩存造成負擔