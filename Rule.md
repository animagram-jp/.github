// This file includes untranslated text (ja).

# animagram-rule

Common for projects in animagram

## Contribution

### Writing

#### Name format

This is for easy reading by human and computers. 

**Single word naming is always best.**

- directory:  snake_case (I think kebab-case is also good unless for scripts.)
- file:
  - Document: CamelCase
  - script:   snake_case
- md outline: Capitalized with space
- sentence:   Capitalized with space and .

Abbreviations follow the same rule as others.

### Document

When written in Japanese for maintainability, state on the 1st line the following:

```text
// This file includes untranslated text (ja).
```

### Script

Write scripts with only ASCII.
The following example uses Rust. When using a different stack, adapt it accordingly.

#### Dependency

- 依存関係とは、単に第三者のモジュールだけでなく、そのfile内に記述されない全てのスクリプトを指す。
- `use`で明示可能な依存は**全てファイル先頭に列挙**する。本文中の単独インライン参照は禁止
- 順序: `core` → `alloc` → `std` → `crate` -> attribute付き (`core` → `alloc` → `std` → `crate`)
- 同一モジュールはまとめて列挙する。数個程度なら改行する
- std制限が予想されるプロジェクトでは、予めコメントアウトしたno_std宣言をルートファイルに書いておく
- 必ずstdやno_stdをfeature化する必要はない。どちらでも動く1通りが最善

```rust
// examples
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

#[cfg(test)]
use std::fs;

use crate::module;
```

#### Error

- `std::error::Error` implはno_std非対応のため使わない
- モジュール固有のエラー型(`SvdError`等)はそのmodで定義し、crateのpub Error enumでラップ(`Error::Svd(SvdError)`)
- ルートファイル等にpub Error / pub Resultを集約することで、利用者がエラー処理を網羅実装できる

```rust
// examples

/// Error type for SVD operations
#[derive(Debug, Clone)]
pub enum SvdError {
    DimensionMismatch,
    ConvergenceFailed,
    InvalidInput(&'static str),
}

impl fmt::Display for SvdError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match self {
            SvdError::DimensionMismatch => write!(f, "dimension mismatch"),
            SvdError::ConvergenceFailed => write!(f, "convergence failed"),
            SvdError::InvalidInput(msg) => write!(f, "invalid input: {}", msg),
        }
    }
}
```

#### Comment

- pub fn: doctestを書く。意義のあるtestを書き、newなどはコメント・docTest無しでよい
- pub struct等, (private) fn: その存在の趣旨を1行コメント(///)で説明
- fn内、必要な個所には適宜インラインコメント(//)で意図を名言する

#### Test

- doctestと重複が無いようにUnitTestを作成する
- testはstd及びexamples/以下に依存して構わない。むしろ、インラインのデータセット定義を避ける
- グループをコメントで区切る（e.g., `// --- traverse ---`）
- fn名は `{対象}_{条件}` 形式（test_ 不要）
- エラーケース・境界ケースを明示的に書く
- integration testとして、examples/にて受け入れ側をインメモリモックimplとデータセットファイルで徹底して再現した上で、 提供portsを検証する
