## 變數

### 變數宣告

```rust
// 宣告區域變數
let local_var = 123;
```

### 不可變變數

```rust
let immutable_var = 123;

print!("{}", immutable_var);

immutable_var = 456;

print!("{}", immutable_var);
```

上面這段程式碼在編譯時會出現"cannot assign twice to immutable variable"錯誤，表示不可變變數無法重新賦予值

有點類似golang中的const(常數)但又不太一樣，rust本身也有const關鍵字

### 可變變數

```rust
let mut mutable_var = 123;

print!("{}", mutable_var);

mutable_var = 456;

print!("{}", mutable_var);
```

透過在宣告變時增加mut(mutable)來讓這個變數可以重新賦予值

### 未變化的可變變數

```rust
let mut mutable_var = 123;

print!("{}", mutable_var);
```

如果宣告了可變變數，但是後面又沒重新賦予值時編譯會出現"help: remove this `mut`"警告來建議移除mut

### 未初始化的變數

```rust
let immutable_var :i32;

print!("{}", immutable_var);
```

編譯時會出現"use of possibly-uninitialized `immutable_var`"錯誤

初始化變數也可以在宣告變數之後，只要在變數使用之前初始化就可以，例如下面例子

```rust
let immutable_var :i32;

immutable_var = 123;

print!("{}", immutable_var);
```

### 型別和可變化的改變

rust允許在變數宣告後又重新宣告相同名稱的變數，下面這些行為在rust是合法

```rust
let mut var = 123;

print!("{}", var);

var = 456;

print!("{}", var);

// 重新宣告為不可變變數
let var = 789;

print!("{}", var);

// 重新宣告為字串類型
let var = "hello word";

print!("{}", var);
```

### 未使用的變數

```rust
let immutable_var = 123;
```

未使用變數時會出現"help: if this is intentional, prefix it with an underscore: `_immutable_var`"警告，如果不想出現警告可以在變數前面加個下底線"_"

```rust
let _immutable_var = 123;
```

或是單純只是要一個站位符也可以這樣

```rust
let _ = 123;
```