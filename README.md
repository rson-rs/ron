## Rust Object Notation

[![Build Status](https://travis-ci.org/rson-rs/rson.png?branch=master)](https://travis-ci.org/rson-rs/rson)
[![Crates.io](https://img.shields.io/crates/v/rson.svg)](https://crates.io/crates/rson_rs)
[![Docs](https://docs.rs/rson/badge.svg)](https://docs.rs/rson)
[![Gitter](https://badges.gitter.im/rson-rs/rson.svg)](https://gitter.im/rson-rs/rson)

RSON is a simple readable data serialization format that looks similar to Rust syntax.
It's designed to support all of [Serde's data model](https://serde.rs/data-model.html), so
structs, enums, tuples, arrays, generic maps, and primitive values. RSON is a fork of
[RON](https://github.com/ron-rs/ron) library, but provides a more appropriate Rust-lang syntax. 

### Example in JSON

```json
{
   "materials": {
        "metal": {
            "reflectivity": 1.0
        },
        "plastic": {
            "reflectivity": 0.5
        }
   },
   "entities": [
        {
            "name": "hero",
            "material": "metal"
        },
        {
            "name": "moster",
            "material": "plastic"
        }
   ]
}
```

Notice these issues:
  1. Struct and maps are the same
    - random order of exported fields
      - annoying and inconvenient for reading
      - doesn't work well with version control
    - quoted field names
      - too verbose
    - no support for enums
  2. No trailing comma allowed
  3. No comments allowed

### Same example in RSON

```rust
/*
 * Scene object example
 */
Scene { // class name is optional
    materials: { // this is a map
        "metal": {
            reflectivity: 1.0,
        },
        "plastic": {
            reflectivity: 0.5,
        },
    },
    entities: [ // this is an array
        { // this is a object
            name: "hero",
            material: "metal",
        },
        {
            name: "monster",
            material: "plastic",
        },
    ],
}
```

The RSON format uses `{`..`}` brackets for *heterogeneous* structures (classes) and
*homogeneous* maps, where classes are different from maps by keys: in classes those
are identifiers, but in maps those are values. Additionally, it uses `(`..`)` brackets
for heterogeneous tuples, and `[`..`]` for homogeneous arrays. This distinction allows
to solve the biggest problem with JSON.

### Same example in RON

```rust
Scene( // class name is optional
    materials: { // this is a map
        "metal": (
            reflectivity: 1.0,
        ),
        "plastic": (
            reflectivity: 0.5,
        ),
    },
    entities: [ // this is an array
        (
            name: "hero",
            material: "metal",
        ),
        (
            name: "monster",
            material: "plastic",
        ),
    ],
)
```

Unlike RSON, the RON format uses `(`..`)` brackets for all *heterogeneous* structures (classes
and tuples), while preserving the `{`..`}` for maps, and `[`..`]` for *homogeneous* arrays. This
is non-traditional syntax for classes of both the JSON and the native Rust representation.

### RSON heterogeneous structures syntax

Here are the general rules to parse the heterogeneous structures:

| class is named? | fields are named? | what is it?               | example             |
| --------------- | ------------------| ------------------------- | ------------------- |
| no              | no                | tuple                     | `(a, b)`            |
| yes/no          | no                | tuple struct              | `Name(a, b)`        |
| yes             | no                | enum value                | `Variant(a, b)`     |
| yes/no          | yes               | struct                    | `{f1: a, f2: b,}`   |

### Specification

There is a very basic, work in progress specification available on
[the wiki page](https://github.com/rson-rs/rson/wiki/Specification).

### Appendix

Why not XML?
  - too verbose
  - unclear how to treat attributes vs contents

Why not YAML?
  - significant white-space 
  - specification is too big

Why not TOML?
  - alien syntax
  - absolute paths are not scalable

Why not RON?
  - non-traditional syntax for classes
  - confuse between class and map syntax
  - does not support block comments

Why not XXX?
  - if you know a better format, tell me!

## License

RSON is dual-licensed under Apache-2.0 and MIT.

