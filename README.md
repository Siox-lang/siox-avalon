# siox-avalon

The Intel Avalon family interfaces for [siox](https://github.com/Siox-lang/sioxc). Each
module has one width-generic struct with every signal and one view per
endpoint role.

| Module | Interface | Struct | Views |
|---|---|---|---|
| `avalon::mm` | Avalon-MM | `AvalonMm<A, D>` | `Host` / `Agent` |
| `avalon::st` | Avalon-ST | `AvalonSt<D>` | `Source` / `Sink` |

The clock and reset are not part of the structs; they stay ordinary ports of
the endpoint entities. Each module's header says what is left out.

## Use

Until siox has a package manager, put the `avalon/` directory at your project
root (copy, symlink or submodule) and import from it:

```siox
use avalon::mm::{AvalonMm, Agent};

entity Peripheral { clk: Bit in, bus: AvalonMm<16, 32> Agent }
```

A parent connects a `Host` and a `Agent` through one `let wire: AvalonMm<16, 32>;`.

## Test

Each `<module>_test.siox` drives a small endpoint from a testbench:

```bash
sioxc --std <sioxc>/std --test mm_test.siox -o mm_test && ./mm_test
```

Needs a `sioxc` with Siox-lang/sioxc#68: width-parameterised struct ports.
