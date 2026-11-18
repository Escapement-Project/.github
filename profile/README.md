# Escapement Project

We build open firmware for debug probes, with the tools to install and measure it.

A probe's firmware decides how fast it talks to a target, which protocols it speaks, and how
reliably it reads what comes back. We publish the measurements behind the performance and behaviour
claims we make, about our firmware and the vendor's. We also publish the benchmark code, so anyone
can reproduce them.

> [!NOTE]
> The project is in development.
> The implementation, benchmarks, and supporting notes will be published with the first release.

NXP's MCU-Link and MCU-Link Pro are the first probes we support. The docs and the measurements are
on our website: [escapement-project.github.io](https://escapement-project.github.io).

## Verge

Verge is our open firmware for NXP's MCU-Link, a CMSIS-DAP probe built on a dual-core LPC55S69.
One Verge image runs on both the standalone MCU-Link and the MCU-Link Pro.

The firmware generates SWD and JTAG transfer code at runtime for the requested clock rate.
At a fast clock rate it calibrates read sampling on attach. Calibration measures the target's read
latency. It generates code for candidate latencies in turn, tests each with DPIDR or IDCODE reads,
and keeps the one every result agrees with.

Through probe-rs, Verge writes over SWD at 3.39× the rate of NXP's firmware V3.174: 2,258 KiB/s at
50 MHz[^1], against 667 KiB/s at 20 MHz, the fastest clock NXP's firmware reads right at[^2].
At 10 MHz, Verge writes 744.8 KiB/s, 42% more than NXP's firmware.

### Repositories

- **[mculink-verge](https://github.com/Escapement-Project/mculink-verge)**: Verge, our open firmware for NXP's MCU-Link.
- **[verge-probe](https://github.com/Escapement-Project/verge-probe)**: `probe-rs`-based library exposing Verge-specific features.
- **[verge](https://github.com/Escapement-Project/verge)**: MCU-Link command-line and graphical tools built on `verge-probe`.
- **[mculink-bench](https://github.com/Escapement-Project/mculink-bench)**: measurements that compare Verge with NXP's firmware, and the tools that take them.
- **[mculink-recordings](https://github.com/Escapement-Project/mculink-recordings)**: logic analyser recordings of MCU-Link probes driving a target's debug pins, which the measurements cite.
- **[mculink-stock-notes](https://github.com/Escapement-Project/mculink-stock-notes)**: observations of what NXP's firmware does, on USB and on the wire.
- **[tickwave](https://github.com/Escapement-Project/tickwave)**: a web viewer for mixed-signal captures. Our pages show the recordings in it.

## Current capabilities

For the standalone MCU-Link and the MCU-Link Pro:

- CMSIS-DAP v2 over high-speed USB
- SWD and JTAG
- cJTAG, IEEE 1149.7 OScan1, OScan2, OScan5, and OScan6
- Read sampling calibration
- SWO capture, and the onboard UARTs as USB serial ports up to 30 Mbaud: one on the standalone MCU-Link, two on the Pro
- The Pro's status LEDs, host GPIO, SPI and I2C bridges, target power and energy measurement
- Updates without the ISP jumper, from NXP's firmware and between our own builds
- Throughput and wire-clock comparisons with NXP's firmware
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
- MCU-Link Pro: the analog trace input
- Evaluate MCU-Link On Board support
- Our own open-hardware probe design

## Licence

We publish code under the Apache License 2.0, tickwave and the website also under MIT. The
recordings and the project's marks are under CC BY 4.0. Use the marks to refer to the project or its
tools, not to brand another project.

<sub>The Escapement Project is not affiliated with NXP or any other probe vendor.
MCU-Link and LinkServer are NXP's names for its products.</sub>

[^1]: When compiled with Clang. GCC peaks lower, at 2,176 KiB/s. Measured on 2026-11-04 with 4 KiB
    writes to an MSPM0L1306, through a build of probe-rs with changes for speed. Every figure is on
    the [measurements page](https://escapement-project.github.io/verge/measurements/).
[^2]: Above 20 MHz NXP's firmware V3.174 reads wrong on this target, and its runs failed.
