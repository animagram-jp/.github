//! this file contents partially includes contents that untranslated (Japanese) expressions.

# animagram-rule

Rule of all project of animagram

## Contribution

### file

保守の都合、日本語原文だけの部分のあるドキュメント1行目には、以下のように明示する

```text
//! this file contents partially includes contents that untranslated (Japanese) expressions.
```

### code

以下、Rustを例に使用。コーディング言語が違う場合は適切に読み替えること

### use宣言

- `use`で宣言できるものは**全てファイル先頭に列挙**する。本文中の`core::`/`alloc::`/`std::`インライン参照は禁止
- 順序: `core` → `alloc` → `std`(cfg付き) → `crate`
- 複数importは同一crateをネストしてまとめる
- `std`依存は`#[cfg(feature = "std")]`で明示。テスト限定なら`#[cfg(all(test, feature = "std"))]`
- `f64`メソッド(`.sqrt()`等)のcfgガードはskip（Rustの実装詳細）
- `vec!`/`format!`マクロもno_std下では`use alloc::{vec, format};`が必要
- `std` featureはCargo.tomlで`default = ["std"]`にしておき、no_std確認時は`--no-default-features`で行う
- `#![no_std]`はlib.rs先頭に宣言。確認前の作業中はコメントアウトで進めてよい

```rust
// use宣言 実装例
use core::{
    cmp::Ordering,
    f64::consts::PI,
};
use alloc::{
    collections::BTreeSet,
    format,
    string::String,
    vec,
    vec::Vec,
};
#[cfg(all(test, feature = "std"))]
use std::fs;
#[cfg(feature = "std")]
use std::time::{SystemTime, UNIX_EPOCH};
use crate::my_module;
```

### エラー型

- `std::error::Error` implはno_std非対応のため使わない
- モジュール固有のエラー型(`SvdError`等)はそのmodで定義し、crateのpub Error enumでラップ(`Error::Svd(SvdError)`)
- lib.rs(ports.rsまたはports/provided.rs)にpub Error / pub Resultを集約することで、利用者がエラー処理を網羅実装できる

## テンプレート

```rust
#![no_std]
extern crate core;
extern crate alloc;
use core::{
    primitive::{
        u8, u16, u32, u64, u128, usize,
        i8, i16, i32, i64, i128, isize,
        f32, f64,
        bool,
        char,str,
    },
    num,
    ops,
    cmp,
    convert,
    mem,
    ptr,
    alloc::{GlobalAlloc, Layout},
    slice,
    option,
    result,
    iter,
    sync::atomic::{AtomicUsize, AtomicBool, Ordering},
    cell::{Cell, RefCell, UnsafeCell},
    fmt,
    hint,
    marker::{Send, Sync, Copy, PhantomData},
    borrow,
    hash::{Hash, Hasher},
    panic::Panicinfo,
};
use alloc::{
    vec::Vec,
    boxed::Box,
    sync::Arc,
};
use log::{Log, Record};

#[cfg(feature = "nightly")]
use core::intrinsics::abort;

struct Allocator;

unsafe impl GlobalAlloc for Allocator {
    unsafe fn alloc(&self, layout: Layout) -> *mut u8 {
        core::ptr::null_mut()
    }
    unsafe fn dealloc(&self, ptr: *mut u8, layout: Layout) { }
}

#[global_allocator]
static ALLOCATOR: Allocator = Allocator;


macro_rules! debug_log {
    ($($arg:tt)*) => {{
        #[cfg(feature = "logging")]
        { log::debug!($($arg)*); }
    }};
}

#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    debug_log!("panic: {}", info);
    #[cfg(feature = "nightly")]
    unsafe { abort() };
    loop {}
}
```
