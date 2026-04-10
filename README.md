// this file includes contents that untranslated expressions (ja).

# animagram-rule

Common for projects in animagram

## Contribution

### file

保守の都合、日本語原文だけの部分のあるドキュメント1行目には、以下のように明示する

```text
// this file includes contents that untranslated expressions (ja).
```

### code

Write scripts with only ASCII. Because English is suitable for naming and ASCII decoder is ubiquitous.
以下、Rustを例に使用。異なるコーディング言語は、適切に読み替えること

### use宣言

- `use`で宣言できるものは**全てファイル先頭に列挙**する。本文中の単独インライン参照は禁止
- 順序: `core` → `alloc` → `std` → `crate` -> cfg付き (以下、順序再帰)
- 同一モジュールはまとめて列挙する。数個なら改行する
- アトリビュート`#[cfg(all(test, feature = "std"))]`などで制御
- std制限が予想されるプロジェクトでは、no_std宣言をlibやmainの冒頭でコメントアウトしておくと、意識してコーディングしやすい
- 必ずstdやno_stdをfeature化する必要はない。どちらでも動く1通りが最善

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
