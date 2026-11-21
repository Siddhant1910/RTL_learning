# RTL_Learning

Learning digital design the hard way: write the Verilog, test it with a self-checking testbench, synthesize it in Vivado, and push it here.

**Goal:** build a portfolio of small, verified RTL designs that grow into a larger FPGA project.
**Target device:** Xilinx Artix-7 (synthesis in Vivado)
**Simulation:** Icarus Verilog / EDA Playground, waveforms in GTKWave

---

## Progress

| # | Design | Status |
|---|--------|--------|
| 1 | UART baud-rate generator | In progress |
| 2 | UART transmitter (FSM) | Planned |
| 3 | UART receiver + loopback test | Planned |
| 4 | Synchronous FIFO | Planned |
| 5 | Async FIFO (clock domain crossing) | Planned |

---

## Repository layout

```
RTL_Learning/
├── README.md
└── uart/
    ├── rtl/
    │   └── baud_gen.v        # design
    └── tb/
        └── baud_gen_tb.v     # self-checking testbench
```

---

## 1. UART Baud-Rate Generator

A counter that produces a one-clock-cycle pulse (`tick`) at a fixed rate. The UART transmitter and receiver will use this pulse to time each bit.

### How it works

The counter counts from 0 to `DIV - 1`. When it reaches the end, it raises `tick` for one clock cycle and starts over.

```
DIV = CLK_FREQ / BAUD
    = 50,000,000 / 115,200
    = 434  (rounded)
```

Rounding to 434 gives an actual rate of about 115,207 baud, which is roughly 0.006% off. That is well within UART tolerance.

```
clk   : _|‾|_|‾|_|‾|_|‾|_|‾|_ ... _|‾|_|‾|_|‾|_
count : 0  1  2  3  4  5  ...  433  0  1  2
tick  : ___________________ ... ___|‾|_________
                                    ^ one clock wide, every 434 cycles
```

### Interface

| Signal | Direction | Width | Description |
|--------|-----------|-------|-------------|
| `clk`  | in  | 1 | System clock |
| `rst`  | in  | 1 | Reset (clears the counter) |
| `tick` | out | 1 | One-cycle pulse every `DIV` clocks |

| Parameter | Default | Description |
|-----------|---------|-------------|
| `CLK_FREQ` | 50,000,000 | Clock frequency in Hz |
| `BAUD` | 115,200 | Target baud rate |

### Verification

The testbench is self-checking: it measures the number of clock cycles between two `tick` pulses and compares it with `DIV`. It prints `PASS` or `FAIL`, so no one has to read waveforms to know if it worked.

| Check | Result |
|-------|--------|
| Cycles between ticks equals `DIV` | `[PASS / FAIL]` |
| Tick is exactly one clock wide | `[PASS / FAIL]` |
| Counter clears on reset | `[PASS / FAIL]` |

### Run the simulation

```bash
iverilog -o sim uart/rtl/baud_gen.v uart/tb/baud_gen_tb.v
vvp sim
gtkwave dump.vcd      # optional: view waveforms
```

Or paste both files into [EDA Playground](https://www.edaplayground.com) and run with Icarus Verilog.

### Synthesis results (Vivado, Artix-7)

| Resource | Used |
|----------|------|
| LUTs | `[fill in]` |
| Registers | `[fill in]` |
| Max frequency | `[fill in after implementation]` |

---

## Tools

- Verilog HDL
- Icarus Verilog, EDA Playground, GTKWave
- Xilinx Vivado (Artix-7)
- Git / GitHub

## Learning log

- **Day 1:** Repo setup. Wrote the baud-rate generator and its testbench.
