# Development Rules

These are the common rules across the entire project development.
This content is intended for everyone, whether LLM or human.
Top priority is placed on maintaining a state where the person in charge can comprehensively manage each codebase.
For scenes where the rules below do not apply, please make judgments that serve this goal.

---

## Operations

### Base files

| Filepath | Content |
|-|-|
| `./README.md` | The first document to read. Describes the software, release schedule, quick start, provided API, etc. |
| `./LICENSE` | States the license, either Apache-2.0 or GPL-3.0-only, and the authors' names. |
| `./CONTRIBUTING.md` | The common entrypoint for all developers. See [Beginning of CONTRIBUTING.md](#beginning-of-contributingmd) below. |
| `./.gitignore` | As below. Explicitly opt in only the files that need to be indexed. |

```gitignore
# ./.gitignore
/*.md
!README.md
!CONTRIBUTING.md

/docs/*
```

#### Beginning of CONTRIBUTING.md

Read [ORG_CONTRIBUTING.md](./ORG_CONTRIBUTING.md).
If "ORG_CONTRIBUTING.md" does not exist in the repository root of your working environment, download it by executing the following.

```bash
curl -fsSL -H "Accept: application/vnd.github.raw+json" "https://api.github.com/repos/animagram-jp/.github/contents/.github/CONTRIBUTING.md?ref=main" -o "ORG_CONTRIBUTING.md"
```

### Issue

- Anyone can create issues at any time. Maintainers may clean these up at any time.
- Attatch tags as below:

| Label | Description | Color |
|-|-|-|
| bug | Something isn't working. | #FF2800 |
| controverse | What we should talk about. | #5B2F91 |
| improvement | A way to improve the repository. | #35A16B |

### Git

- Deploy targets must always be versioning tags without a v-prefix. In principle, they should be created on the "main" (or "staging") branch.
- Depending on the scale of the repository, additional branches such as "develop" or "feature/{ISSUE_NUMBER}" may be used ([reference: Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)). Delete unnecessary branches as you go.
- Deploy targets for "staging" use version tags in the `0.x.x` series.

---

## Writing

This is for easy reading by human and computers.
The following example uses Rust. When using a different stack, adapt it accordingly.

- Use 4 spaces (0x20) for indentation.
- State on the 1st line `// This file includes untranslated text (ja).` when needed.

### Naming

Computers can fail to handle uppercase, full-width characters, and names starting with a digit.

- **Single word naming is always best.**
- **In many cases, it is preferable to use singular names rather than plural ones.**
- Using abbreviations of common nouns for the purpose of reducing character count is prohibited.
- Abbreviations follow the same rule as others.
- When kebab-case is allowed, combine it with snake_case to clarify the relation between compound words:
    e.g., file-system_architecture
- Follow the table below as much as possible, rather than the conventions of some stacks.

| Category | Field | Rule | Description |
|-|-|-|-|
| directory | dirname  | snake_case | Kebab-case is also good unless for scripts. |
| document  | file     | CamelCase  |-|
|           | outline  | Capitalized with space |-|
|           | sentence | Capitalized with space and . |-|
| script    | file     | snake_case |-|
|           | function | snake_case |-|
|           | constant | UPPER_SNAKE |-|
|           | variable | snake_case |-|
| data file | file     | snake_case |-|
|           | key      | snake_case |-|

### Comment

- Write an outer line DocComment for each public item.
- Write inline comments only where necessary, for non-obvious reasons (why / why not).
- Write a DocTest for each public fun. Skip meaningless tests(e.g., Item:new) and comments.

### Test

- Write unit tests that is not duplicating with DocTest.
- Tests can depend on std::fs and the examples directory. Avoid inline dataset definitions.
- Names of test functions should follow the format `{target}_{condition}` (omit test_).
- When integration test, using the examples directory, in-memory mock implementations to verify exported functions.

### Literal quote

Except for shell scripts, always follow the below. 
When nesting occurs, keep the outer quote double and use single quotes for the inner one.
- slicable string: "" e.g., "", "ab"
- unslicable char: '' e.g., 'a'

### Dependency declaration

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

### Error

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

### Html

- Tag id rules:
    - Automatically determined based on the parent tag after the body and its sequence number.
    - "_" = Parent-child segment separator. (e.g., main_div_section-1)
    - "-N" = Sequence number within the same tag. (e.g., span-3, th-2)
    - No sequence number = Only one in that hierarchy. (e.g., thead_tr, legend_h5)
- Formatting rules:
    - Follow this order: `<tag, id, standard attribute, aria-label, class, cutom attribute>`.
    - Do not insert a line break before a closing tag.
    - Insert a line break before the start of every tag.
