# Escapement Project

We build open firmware for debug probes, with the tools to install and measure it.

A probe's firmware decides how fast it talks to a target, which protocols it speaks, and how
reliably it reads what comes back. We publish the measurements behind the performance and behaviour
claims we make, about our firmware and the vendor's. We also publish the benchmark code, so anyone
can reproduce them.

> [!NOTE]
> The project is in development.
> The implementation, benchmarks, and supporting notes will be published with the first release.

NXP's MCU-Link is the first probe we support.

## MCU-Link

A CMSIS-DAP probe built on a dual-core LPC55S69.

The firmware generates SWD and JTAG transfer code at runtime for the requested clock rate.
At a fast clock rate it calibrates read sampling on attach. Calibration measures the target's read
latency. It generates code for candidate latencies in turn, tests each with DPIDR or IDCODE reads,
and keeps the one every result agrees with.

Through probe-rs over SWD at 20 MHz, it reads memory up to 2.4× faster than the stock firmware.

### Repositories

- **[mculink-verge](https://github.com/Escapement-Project/mculink-verge)**: open firmware for NXP's MCU-Link.
- **[verge](https://github.com/Escapement-Project/verge)**: MCU-Link update tool, with command-line and graphical interfaces.
- **[mculink-bench](https://github.com/Escapement-Project/mculink-bench)**: measurements that compare our firmware with stock, and the tools that take them.
- **[mculink-stock-notes](https://github.com/Escapement-Project/mculink-stock-notes)**: observations of what NXP's stock firmware does, on USB and on the wire.

## Current capabilities

For the MCU-Link:

- CMSIS-DAP v2 over high-speed USB
- SWD and four-wire JTAG
- Two-pin cJTAG, IEEE 1149.7 OScan1
- Read sampling calibration
- SWO capture, and the onboard UART as a USB serial port
- Updates without the ISP jumper, from stock and between our own builds
- Throughput and wire-clock comparisons with stock
- Update tool board detection, signature verification, batch updates and flashing

## Roadmap

Before the first release:

- [ ] Firmware signing in CI for the updater
- [ ] Update tool CI builds
- [ ] MCU-Link Pro support
- [ ] cJTAG OScan2 and OScan5 support

Longer term:

- [ ] Our own USB IDs, from pid.codes
- [ ] SWD reads at 50 MHz, to match writes
- [ ] Faster block transfers, and the onboard UART above 2 Mbaud
- [ ] More probes
- [ ] Our own open-hardware probe design

## Using LLMs

LLMs are welcome here: for exploring a codebase, debugging, experimenting, and writing code. You
are responsible for the result, however it was written.

- Read everything you submit, and be ready to explain any line of it.
- Keep changes small. LLM output tends to be verbose. Every line it adds is one more for a reviewer
  to read. Remove anything unnecessary, in the code and especially in prose.
- Make one change per pull request.
- Write tests that check behaviour. A test that restates the implementation proves nothing.
- Back any claim about speed, timing or hardware behaviour with the measurement that shows it. Say
  what ran on real hardware and what did not. A model states a figure just as confidently when
  nobody measured it.

We use LLMs ourselves and hold our contributions to the same standard.

This section borrows from [probe-rs's policy](https://github.com/probe-rs/probe-rs/blob/master/CONTRIBUTING.md#on-ai-contributions).

## Licence

We publish under the Apache License 2.0.

<sub>The Escapement Project is not affiliated with NXP or any other probe vendor.
MCU-Link and LinkServer are NXP's names for its products.</sub>
