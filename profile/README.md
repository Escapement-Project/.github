# Escapement Project

We build open firmware for debug probes, with the tools to install and measure it.

A probe's firmware decides how fast it talks to a target, which protocols it speaks, and how
reliably it reads what comes back. We publish the measurements behind every claim we make, about
our firmware and about the vendor's. The code that took them is published too, so anyone can
reproduce them.

NXP's MCU-Link is the first probe we support.

## MCU-Link

A CMSIS-DAP probe built on a dual-core LPC55S69.

- **[mculink-verge](https://github.com/Escapement-Project/mculink-verge)**: open firmware for NXP's MCU-Link, with two-pin cJTAG and up to 2.4x throughput through probe-rs.
- **[verge](https://github.com/Escapement-Project/verge)**: MCU-Link update tool, as a command line or a window.
- **[mculink-bench](https://github.com/Escapement-Project/mculink-bench)**: measurements that compare our firmware with stock, and the tools that take them.
- **[mculink-stock-notes](https://github.com/Escapement-Project/mculink-stock-notes)**: observations of what NXP's stock firmware does, on USB and on the wire.

## Roadmap

Towards the first release, for the MCU-Link:

- [x] CMSIS-DAP v2 over high-speed USB
- [x] SWD and four-wire JTAG, with the transfer code built at runtime for the clock the host asks for
- [x] Two-pin cJTAG, IEEE 1149.7 OScan1
- [x] Each target's reply delay measured as it connects, so reads sample at the right moment
- [x] SWO capture, and the onboard UART as a USB serial port
- [x] Updates without the ISP jumper, from stock and between our own builds
- [x] Throughput and wire-clock comparisons with stock
- [ ] MCU-Link Pro support
- [ ] cJTAG's other scan formats, OScan2 and OScan5
- [ ] The update tool, with batch updates and signed releases
- [ ] The first public release

After it:

- [ ] Our own USB IDs, from pid.codes
- [ ] SWD reads at 50 MHz, to match writes
- [ ] Faster block transfers, and the onboard UART above 2 Mbaud
- [ ] More probes
- [ ] Our own open-hardware probe design

## Licence

We publish under the Apache License 2.0.

<sub>The Escapement Project is not affiliated with NXP or any other probe vendor.
MCU-Link and LinkServer are NXP's names for its products.</sub>
