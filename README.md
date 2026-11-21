

\# RTL\_Learning



Learning digital design the hard way: write the Verilog, test it with a self-checking testbench, synthesize it in Vivado, and push it here.



\*\*Goal:\*\* build a portfolio of small, verified RTL designs that grow into a larger FPGA project.  

\*\*Target device:\*\* Xilinx Artix-7 (synthesis in Vivado)  

\*\*Simulation:\*\* Icarus Verilog / EDA Playground, waveforms in GTKWave



\---



\## Progress



| # | Design                             | Status      |

| - | ---------------------------------- | ----------- |

| 1 | UART baud-rate generator           | In progress |

| 2 | UART transmitter (FSM)             | Planned     |

| 3 | UART receiver + loopback test      | Planned     |

| 4 | Synchronous FIFO                   | Planned     |

| 5 | Async FIFO (clock domain crossing) | Planned     |



\## Repository layout



```text

RTL\_Learning/

├── README.md

└── uart/

&#x20;   ├── rtl/

&#x20;   │   └── baud\_gen.v        # design

&#x20;   └── tb/

&#x20;       └── baud\_gen\_tb.v     # self-checking testbench

```



\---





\## Tools



\- Verilog HDL

\- Icarus Verilog, EDA Playground, GTKWave

\- Xilinx Vivado (Artix-7)

\- Git / GitHub



