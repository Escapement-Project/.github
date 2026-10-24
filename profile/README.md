# Escapement Project

We build open firmware for debug probes, with the tools to install and measure it.

A probe's firmware decides how fast it talks to a target, which protocols it speaks, and how
reliably it reads what comes back. We publish the measurements behind the performance and behaviour
claims we make, about our firmware and the vendor's. We also publish the benchmark code, so anyone
can reproduce them.

> [!NOTE]
> The project is in development.
> The implementation, benchmarks, and supporting notes will be published with the first release.

NXP's MCU-Link and MCU-Link Pro are the first probes we support.

## MCU-Link

A CMSIS-DAP probe built on a dual-core LPC55S69. One firmware image runs on both the MCU-Link
and the MCU-Link Pro.

The firmware generates SWD and JTAG transfer code at runtime for the requested clock rate.
At a fast clock rate it calibrates read sampling on attach. Calibration measures the target's read
latency. It generates code for candidate latencies in turn, tests each with DPIDR or IDCODE reads,
and keeps the one every result agrees with.

4.39× stock firmware's peak probe-rs write throughput over SWD: 2,286 KiB/s at 50 MHz[^1]; stock
peaks at 521 KiB/s at 10 MHz[^2].
At the same 10 MHz rate, the Clang build reached 737 KiB/s, 41% faster than stock.

### Repositories

- **[mculink-verge](https://github.com/Escapement-Project/mculink-verge)**: open firmware for NXP's MCU-Link.
- **[verge-probe](https://github.com/Escapement-Project/verge-probe)**: `probe-rs`-based library exposing Verge-specific features.
- **[verge](https://github.com/Escapement-Project/verge)**: MCU-Link command-line and graphical tools built on `verge-probe`.
- **[mculink-bench](https://github.com/Escapement-Project/mculink-bench)**: measurements that compare our firmware with stock, and the tools that take them.
- **[mculink-stock-notes](https://github.com/Escapement-Project/mculink-stock-notes)**: observations of what NXP's stock firmware does, on USB and on the wire.

## Current capabilities

For the MCU-Link and the MCU-Link Pro:

- CMSIS-DAP v2 over high-speed USB
- SWD and four-wire JTAG
- Two-pin cJTAG, IEEE 1149.7 OScan1, OScan2, OScan5, and OScan6
- Read sampling calibration
- SWO capture, and the onboard UARTs as USB serial ports up to 19.2 Mbaud: one on the MCU-Link, two on the Pro
- The Pro's status LEDs, host GPIO bridge, target power and energy measurement
- Updates without the ISP jumper, from stock and between our own builds
- Throughput and wire-clock comparisons with stock
- Update tool board detection, signature verification, batch updates and flashing

## Roadmap

Before the first release:

- [ ] Firmware signing in CI for the updater
- [ ] Update tool CI builds
- [x] MCU-Link Pro support
- [x] cJTAG OScan2, OScan5, and OScan6 support

Longer term:

- Our own USB IDs, from pid.codes
- Faster block transfers
- MCU-Link Pro: host SPI and I2C bridges
- MCU-Link Pro: the analog trace input
- Evaluate MCU-Link On Board support
- Our own open-hardware probe design

## Licence

We publish under the Apache License 2.0.

<sub>The Escapement Project is not affiliated with NXP or any other probe vendor.
MCU-Link and LinkServer are NXP's names for its products.</sub>

[^1]: When compiled with Clang. GCC peaks lower, at 2,130 KiB/s.
[^2]: Stock figures were measured using NXP MCU-Link firmware version V3.174.
