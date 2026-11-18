# Contributing

This guide applies to every repository in the Escapement Project: `mculink-verge`, `verge-probe`,
`verge`, `mculink-bench`, `mculink-stock-notes`, `mculink-recordings`, `tickwave`,
`escapement-project.github.io` and this one, `.github`. Each README lists the repository's build
steps and checks. verge keeps its checks in its CONTRIBUTING.md.

## Issues

Say which probe, which firmware build and which host tool you used, and what you expected. For a
wire problem, an analyser recording or the host tool's log helps more than a description.

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

For `verge-probe`, run the commands its README lists before you submit a change.

For `verge`, run the checks in its CONTRIBUTING.md, under "Testing". Its CI also checks that
production builds exclude the simulator, and compiles the Windows target.

For `mculink-bench`, check that the tools start with `uv run tools/throughput.py --help` and
`uv run tools/wireclock.py --help`. Run `cargo clippy` in `tools/swd-throughput`. Rebuild the
measurements page after changing its template or data. `page/README.md` has the command.

For `mculink-stock-notes`, check that each observation identifies the firmware version and how it
was observed. Keep the raw files unchanged.

For `mculink-recordings`, check each recording's annotations against it, such as
`tickwave annotations check gcc-swd-reads.tickwave gcc-swd-reads.annotations.toml`.

For `tickwave`, run `cargo fmt --check`, `cargo clippy --workspace --all-targets -- -D warnings`
and `cargo test --workspace`, then `npm run check` in `web/`, which needs the two WASM builds first.
Its README has the steps.

For `escapement-project.github.io`, run `uv run design/build.py --check`, which checks that the
generated files match their sources. Then copy in Verge's docs with `uv run .github/verge-docs.py .`
and run `zola build`, which checks the links between pages. The docs come from checkouts of
`mculink-verge`, `verge` and `verge-probe` beside the site's.

## Vendor firmware

Describe a vendor's firmware only by what it does over USB or on the wire, and say how you
observed it. Do not contribute anything taken from disassembling or decompiling a vendor's
firmware image.

## Using LLMs

LLMs are welcome here, to help you explore a codebase, debug, experiment and write code. You are
wholly responsible for the result, however it was written.

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
