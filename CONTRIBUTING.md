# Contributing

This guide applies to every repository in the Escapement Project: `mculink-verge`, `verge-probe`,
`verge`, `mculink-bench` and `mculink-stock-notes`. Each README lists the repository's build steps
and checks. The firmware's AGENTS.md has additional rules for changes to its code.

## Issues

Say which probe, which firmware build and which host tool you used, and what you expected. For a
wire problem, a capture or the host tool's log helps more than a description.

## Pull requests

- Make one change per pull request.
- Run the repository's checks before you open it. See the commands below.
- Write commit subjects as `scope: what changed`, lowercase after the scope, with no full stop.
  Read the last few commits that touched the same files and match them.
- Back any claim about speed, timing or hardware behaviour with the measurement that shows it, and
  say what ran on real hardware and what did not.

### Checks

For `mculink-verge`, run `uv run tools/gate.py`. It builds with GCC and Clang and checks each image
against its emulator baselines.

For `verge-probe`, run:

```sh
cargo fmt --all -- --check
cargo clippy --locked --all-targets --all-features -- -D warnings
cargo test --locked --all-targets --all-features
```

For `verge`, run:

```sh
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo clippy --workspace -- -D warnings
cargo test --workspace
```

Its CI also checks that production builds exclude the simulator and compiles the Windows target.
The README describes both checks.

For `mculink-bench`, check that the tools start with `uv run tools/throughput.py --help` and
`uv run tools/wireclock.py --help`. Run `cargo clippy` in `tools/swd-throughput`. Rebuild the
measurements page after changing its template or data; `page/README.md` has the command.

For `mculink-stock-notes`, check that each observation identifies the firmware version and how it
was observed. Keep raw captures unchanged.

## Vendor firmware

Describe a vendor's firmware only by what it does over USB or on the wire, and say how you
observed it. Do not contribute anything taken from disassembling or decompiling a vendor's
firmware image.

## Using LLMs

LLMs are welcome here: to aid you in exploring a codebase, debugging, experimenting, and writing
code. You are wholly responsible for the result, however it was written.

- Read everything you submit, and be ready to explain any line of it.
- Keep changes small. LLM output tends to be verbose. Every line it adds is one more for a reviewer
  to read. Remove anything unnecessary, in the code and especially in prose.
- Write tests that check behaviour. A test that restates the implementation proves nothing.
- A model states a figure just as confidently when nobody measured it. Check every number it gives
  you against a measurement before you submit it.

We use LLMs ourselves and hold our contributions to the same standard.

This section borrows from [probe-rs's policy](https://github.com/probe-rs/probe-rs/blob/master/CONTRIBUTING.md#on-ai-contributions).

## Licence

The project uses the Apache License 2.0. Contributions use the same licence unless you state
otherwise, as section 5 sets out.
