These are the common rules for development. This content applies to everyone, whether human or not.  
The top priority is keeping the codebase in a state where the person in charge can stay comprehensively aware of every element in it.  
For cases where the rules below don't apply, use judgment that serves this goal.  

# Operations

## Base paths

| Path | Content |
|-|-|
| `./README.md` | The first document to read. Describes the software, release schedule, quick start, provided API, etc. |
| `./LICENSE` | States the license, either Apache-2.0 or GPL-3.0-only. |
| `./CONTRIBUTING.md` | The common entrypoint for all developers. See [Beginning of CONTRIBUTING.md](#beginning-of-contributingmd) below. |
| `./src/` | Root directory for scripts maintainers write. For projects with multiple stacks, use `<stack_name>/src/` instead. |
| `./reference/` | Root directory for reference material. |
| `./artifact/` | Root directory for artifacts: files that can be regenerated. |
| `./distribution/` | Root directory for what this repo distributes to users. Preferred over `./reference/` and `./artifact/` when content fits either. |
| `./.gitignore` | As shown below. Explicitly opt in only the files that need to be tracked. |

```gitignore
# ./.gitignore
/*.md
!README.md
!CONTRIBUTING.md

/reference/*

/artifact/*

/distribution/*
```

## Beginning of CONTRIBUTING.md

Read [ORG_CONTRIBUTING.md](./ORG_CONTRIBUTING.md).  
If "ORG_CONTRIBUTING.md" does not exist in the repository root of your working environment, download it by executing the following.  

```bash
curl -fsSL -H "Accept: application/vnd.github.raw+json" "https://api.github.com/repos/animagram-jp/.github/contents/.github/CONTRIBUTING.md?ref=main" -o "ORG_CONTRIBUTING.md"
```

## Issue

- Anyone can create issues at any time. Maintainers may clean these up at any time.
- Attach tags as below:

| Label | Description | Color |
|-|-|-|
| bug | Something isn't working. | #FF2800 |
| controversy | What we should talk about. | #5B2F91 |
| improvement | A way to improve the repository. | #35A16B |

## Git branch

- Deploy targets must always be versioning tags without a v-prefix. In principle, they should be created on the "main" (or "staging") branch.
- Depending on the scale of the repository, additional branches such as "develop" or `feature/<ISSUE_NUMBER>` may be used ([reference: Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)). Delete unnecessary branches as you go.
- Deploy targets for "staging" use version tags in the `0.x.x` series.

## Commit message

- Use the simplified Conventional Commits format `<type>[(<scope>)][: <keyword>]`.
    e.g., `add(rs): geometry`, `fix(lib.rs, app.rs)`, `clean(toml, md)`
- At least one of `scope` or `keyword` is required.

| Field | Value | Meaning |
|-|-|-|
| type | add | Adding a new item. |
|  | clean | Deleting, tidying up, and unifying the format, combined in one change. |
|  | delete | Deleting an item. |
|  | fix | Fixing a bug. |
|  | rename | Changing a name. |
|  | update | Changing the content of an existing item. |
| scope | `<extension>` | e.g., rs, css, toml, md |
|  | `<filename>` | e.g., lib.rs |
|  | `<module name>` | e.g., calendar, server |
| keyword | `<function name>` | e.g., new |
|  | `<feature>` | e.g., PDF export |

## Sign-off

- Every commit must carry a `Signed-off-by` trailer of its author. `git commit -s` adds it.
- With the trailer, the contributor asserts the [Developer Certificate of Origin 1.1](https://developercertificate.org/) for that commit.
- CI rejects a pull request that contains a commit without the trailer.

## Pull request

- A change reaches the "main" branch only through a pull request.
- A pull request is accepted when all of the following hold:
    - It follows [Writing](#writing).
    - It adds or updates tests as stated in [Test](#test).
    - Every status check passes.
    - Every commit follows [Commit message](#commit-message) and [Sign-off](#sign-off).

## Role

| Role | Member | Responsibility | Access |
|-|-|-|-|
| Maintainer | [Andyou](https://github.com/Andyou007) | Review and merge of pull requests, release, vulnerability response, administration of the organization. | Owner of the GitHub organization, administrator of every repository, owner of the release destinations (crates.io, Cloudflare, GitHub Pages) and of the linked services (Codecov). |
| Contributor | Anyone | Issues and pull requests. | None. |

## Dependency

- Selection: prefer no dependency. Add one only when writing and maintaining the function inside the repository costs more than auditing the dependency. A dependency must have a public repository and a license compatible with Apache-2.0.
- Obtainment: Rust crates come from crates.io through cargo, npm packages from the npm registry through `npm ci`, and GitHub Actions from GitHub pinned by a full commit SHA. Lock files (`Cargo.lock`, `package-lock.json`) are committed for what a repository builds and deploys.
- Tracking: Dependabot opens weekly update pull requests for every ecosystem declared in `.github/dependabot.yml`. CodeQL and OpenSSF Scorecard run in CI. Third-party works copied into a repository are listed in `./NOTICE`.

## Release

- A release is a version tag (see [Git branch](#git-branch)) and a GitHub Release of the same name.
- The version follows [Semantic Versioning](https://semver.org/) and is never reused.
- The release note lists the functional changes and the security fixes of the release, and the upgrade impact.
- Releases are immutable. GitHub signs a release attestation that covers the tag, the commit and the assets. Verify it with `gh release verify <tag> --repo animagram-jp/<repository>`.

# Writing

This is for easy reading by human and computers.
The following example uses Rust. When using a different stack, adapt it accordingly.

- Use 4 spaces (0x20) for indentation.
- When a document is written in a language other than English, state it in the front matter: `lang: ja` and `translations: none`.

## Name

Computers can fail to handle uppercase, full-width characters, and names starting with a digit.

- **Single word naming is always best.**
- **Use singular names** rather than plural ones in most cases.
- Using abbreviations of common nouns for the purpose of reducing character count is prohibited.
- Abbreviations follow the same rule as others.
- When kebab-case is allowed, combine it with snake_case to clarify the relation between compound words:
    e.g., `file-system_architecture`
- Follow the table below as much as possible, rather than the conventions of some stacks.

| Category | Field | Rule | Description |
|-|-|-|-|
| directory | dirname  | snake_case |  Kebab-case is also allowed unless for scripts. |
| document  | file     | CamelCase  |-|
|           | outline  | Capitalized with space | This includes table header cells. |
|           | sentence | Capitalized with space and . |-|
| script    | file     | snake_case |-|
|           | function | snake_case |-|
|           | constant | UPPER_SNAKE |-|
|           | variable | snake_case |-|
| data file | file     | snake_case | Kebab-case is allowed. |
|           | key      | snake_case | As above. |

## Function

- Name a function, excluding test functions, in the format `<verb | preposition>[_<adjective>]_<object>`.
    e.g., `read_status`, `parse_allowlist`
- Do not include in the name a word that already appears in the upper namespace (module or type), nor the object that appears in the arguments.
    e.g., `file::read` rather than `file::read_file`
- Name a test function with a declarative sentence that has a subject and a verb, and states the specification it verifies.
    e.g., `decode_rejects_truncated_frame`
- Do not put `test` or the module name as a prefix of a test function.
- Use `_and_` in a test function name only when the truth of the two sides can never diverge in theory.

## Quotation mark

Where the choice between single and double quotes is not constrained, follow the below.

- Always use double quotes where no nesting occurs.
- Where nesting occurs, always fix the outer quote to double and use single quotes for the inner one.
    e.g., `<div :class="'is-active'">`

## Comment

- Write comments in English.
- Prefer a syntax that needs no closing delimiter (e.g., `//`, `#`) for a comment that is semantically complete within a single line.
- When writing a divider comment, repeat `-` or `=` exactly three times in a row (e.g., `// --- size ---`).
- Write an outer line DocComment for each public item.
- Write inline comments only where necessary, for non-obvious reasons (why / why not).
- Write a DocTest for each public function. Skip meaningless tests (e.g., Item:new) and comments.

## Test

- Write unit tests that is not duplicating with DocTest.
- Tests can depend on std::fs and the examples directory. Avoid inline dataset definitions.
- When integration test, using the examples directory, in-memory mock implementations to verify exported functions.

## Dependency declaration

- To ensure that all dependencies, including those not required, can be identified, declare all dependency references at the beginning of each file.
- Additionally, to ensure consistency in the order and granularity of these declarations, verify this thoroughly for every file you edit upon completion of each task.

```rust
// examples

#![no_std]
extern crate core;
extern crate alloc;
#[cfg(test)]
extern crate std;

use core::{
    primitive::{u8, u64, usize, i64, str},
    fmt::{self, Display, Formatter},
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
};
```

## Error

An error is a variable representation of the point in a script where the code gives up on continuing or completing the intended operation, and instead transitions to safety-oriented handling. Safety-oriented handling means:
1. Guaranteeing, via the variable itself, an appropriate response to the user (e.g. displaying an error message).
2. Emitting a log so developers can retroactively retrieve information about the error's occurrence at runtime.

- Error must reflect detail known to the thrower. Generic naming is prohibited.
- Define an item for each module, and wrap them in the public Error item:

```rust
// examples
use core::fmt::{self, Display, Formatter};

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
    fn fmt(&self, f: &mut Formatter<'_>) -> fmt::Result {
        write!(f, "{:?}", self)
    }
}
```

## Html

### Id

- Automatically determined based on the parent tag after the body and its sequence number.
- "_" = Parent-child segment separator. (e.g., main_div_section-1)
- "-N" = Sequence number within the same tag. (e.g., span-3, th-2)
- No sequence number = Only one in that hierarchy. (e.g., thead_tr, legend_h5)

### Format

- Follow this order: `<tag, id, standard attribute, aria-label, class, custom attribute>`.
- Do not insert a line break before a closing tag.
- Insert a line break before the start of every tag.
