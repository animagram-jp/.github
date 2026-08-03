# Contributing

Here are common rules to keep a repository comprehensible as a whole — so that maintainers can stay aware of every element within the repository at all times. When a rule seems ambiguous, favor the interpretation that best preserves this comprehensibility.

---

## Repository

### Base files

| Filepath | Description |
|-|-|
| `./README.md` | The first document. Description of the softwear, release schedule, quick start, provided api, etc. |
| `./LICENSE` | Includes Apache-2.0 or GPL-3.0-only and authors' names.  |
| `./CONTRIBUTING.md` | The common entrypoint for all developers. Requirement, function, system diagram, public port, internal port, data layout, etc. |
| `./.gitignore` | As below. Opt in files that need to be indexed. |

```
/*.md
!README.md
!CONTRIBUTING.md
/docs/*
```

### Issue

- Anyone can create issues at any time. Maintainers may clean these up at any time.
- tags

| Label | Description | Color |
|-|-|-|
| bug | Something isn't working. | #d73a4a |
| controverse | What we should talk about. | #5B2F91 |
| improvement | A way to improve the repository. | #259d63 |

### Git

- Git branches treat versioning tags without a v-prefix as release targets. In principle, tags should be created on the "main" branch.
- Depending on the scale of development, additional branches such as "develop" or "feature/{ISSUE_NUMBER}" ([reference](https://nvie.com/posts/a-successful-git-branching-model/)) may be used. Clean up unnecessary branches as you go.

---

## Writing

This is for easy reading by human and computers.

- Use 4 spaces (0x20) for indentation.

### Naming

- **Single word naming is always best.**
- **In many cases, it is preferable to use singular names rather than plural ones.**
- Abbreviations follow the same rule as others.
- State on the 1st line `// This file includes untranslated text (ja).` when needed.
- Follow the table below as much as possible, rather than the conventions of some stacks.

| Category | Field | Rule | Description |
|-|-|-|-|
| directory | dirname  | snake_case | Kebab-case is also good unless for script repos. |
| document  | file     | CamelCase  |-|
|           | outline  | Capitalized with space |-|
|           | sentence | Capitalized with space and . |-|
| script    | file     | snake_case |-|
|           | variable | snake_case | And with kebab-case for 2 different degrees of relation. |
| data file | file     | snake_case |-|
|           | key      | snake_case | Following the script variables. |

### Script

The following example uses Rust. When using a different stack, adapt it accordingly.

#### Dependency declaration

- To ensure that all dependencies, including those not required for normal operation, can be identified, please declare all dependency references at the beginning of each file.
- Additionally, to ensure consistency in the order and granularity of these declarations, verify this thoroughly for every file you edit upon completion of each task.
- Order: `core` → `alloc` → `std` → `crate` → those with attributes (`core` → `alloc` → `std` → `crate`).
- To confirm  temporarily to ensure explicit `use` declarations. Please comment out after finishing the modifications.
- You can ensure these practices are followed throughout the entire repository by temporarily adding #![no_std] or #![no_implicit_prelude], such as right before creating each tag.

```rust
// examples

// #![no_std]
extern crate core;
extern crate alloc;
extern crate std;
use core::{
    primitive::{u8, u64, usize, i64, str},
    fmt::{Display, Result, Formatter},
};
use alloc::{
    collections::{BTreeMap, BTreeSet},
    string::{String, ToString},
    vec::Vec,
};

#[cfg(test)]
use std::fs;

use crate::{
    list::{List, VariableList},
    debug_log,
}
```

#### Error

- Do not use `std::error::Error` implementations as they are not compatible with no_std.
- Define an item for each module (such as `ListError`), and wrap them in the public Error item (e.g., `VariableListError::List(ListError)`).

```rust
// examples

#[derive(Debug)]
pub enum ListError {
    OutOfBounds,
    NotExist,
}
#[derive(Debug)]
pub enum VariableListError {
    List(ListError),
    Compact,
}

impl Display for ListError {
    fn fmt(&self, f: &mut Formatter<'_>) -> Result {
        write!(f, "{:?}", self)
    }
}
```

#### Literal quote

- slicable string: "" e.g., "", "ab"
- unslicable char: '' e.g., 'a'

#### Comment

- Write an outer line DocComment for each public item.
- Write a DocTest for each public fun. Skip meaningless tests(e.g., Item:new) and comments.
- Write a DocTest when needed for each private fun.
- Inside functions, clearly state the intent with inline comments where necessary.

#### Test

- Write unit tests that is not duplicating with DocTest.
- Tests can depend on std::fs and the examples directory. Avoid inline dataset definitions.
- Names of test functions should follow the format `{target}_{condition}` (omit test_).
- When integration test, using the examples directory, in-memory mock implementations to verify exported functions.

#### Html

- Tag id rules:
    - Automatically determined based on the parent tag after the body and its sequence number.
    - "_" = Parent-child segment separator. (e.g., main_div_section-1)
    - "-N" = Sequence number within the same tag. (e.g., span-3, th-2)
    - No sequence number = Only one in that hierarchy. (e.g., thead_tr, legend_h5)
- Formatting rules:
    - Follow this order: `<tag, id, standard attribute, aria-label, class, cutom attribute>`.
    - Do not insert a line break before a closing tag.
    - Insert a line break before the start of every tag.
