# 👨‍👩‍👦‍👦 DRAM access info 👨‍👩‍👦‍👦

* This repository contains energy information for DRAM access based on some publications and simulators.


## 1. DRAM (Dynamic Random Access Memory)
DRAM is a type of volatile memory that stores data as electric charge in capacitors and requires periodic refresh cycles. It offers high density and cost-effectiveness, making it widely used in computing systems. Various DRAM standards have evolved to meet different performance, power, and application requirements.

### 1.1 SDRAM (Synchronous Dynamic Random Access Memory)
SDRAM is a type of DRAM that operates in synchronization with the system bus clock, allowing for more predictable and efficient memory access compared to earlier asynchronous DRAMs. It laid the foundation for modern memory interfaces and was widely used before the introduction of DDR technology. Although now largely obsolete in mainstream computing, SDRAM played a key role in the evolution of memory standards and is still used in some embedded or legacy systems.

### 1.2 DDR (Double Data Rate)
DDR memory transfers data on both the rising and falling edges of the clock signal, effectively doubling the data rate compared to traditional SDRAM. It is used as the main memory in desktops, laptops, and servers. DDR technology has evolved through several generations, from DDR to DDR5, offering increased bandwidth and capacity.

### 1.3 LPDDR (Low Power Double Data Rate)
LPDDR is optimized for low power consumption and is widely used in mobile and embedded devices such as smartphones, tablets, and ultra-thin laptops. It incorporates various power-saving features like lower operating voltage and deep power-down modes. The latest generations, such as LPDDR5 and LPDDR5X, support high-speed, energy-efficient operation suitable for AI and multimedia tasks.

### 1.4 HBM (High Bandwidth Memory)
HBM uses 3D-stacked DRAM dies connected via Through-Silicon Vias (TSVs) and placed close to the processor using a silicon interposer. It provides extremely high bandwidth and energy efficiency, making it ideal for high-performance computing (HPC), AI accelerators, and GPUs. HBM3 and HBM3E represent the latest iterations with significant improvements in speed and density.

### 1.5 GDDR (Graphics DDR)
GDDR is a high-speed DRAM variant tailored for graphics and parallel processing workloads. It offers high bandwidth and operates at higher clock rates compared to standard DDR. GDDR is widely used in GPUs, gaming consoles, and deep learning inference systems, with GDDR6 and GDDR6X being the most recent generations.

## 2. Basic Operations at the cell-level
There are two operations that are common to every DRAM: data access and refresh. The two operations self explains the name DRAM (Dynamic Random Access Memory) and enables DRAM to function as a reliable memory. (The [reference](https://stacks.stanford.edu/file/yp843xn4828/Heonjae_Thesis-augmented.pdf) is a good paper to understand DRAM operations.)

### 2.1 Data Access
Once the bit-line sense amplifier (BLSA) is completely precharged, a sequence of three commands, namely activate, read/write and precharge, can be issued to access data stored on the cell.

### 2.2 Refresh
The charge stored on the DRAM cell leaks over time and the data stored on the cell will eventually flip bit if the cell continues to leak charge. Hence, it is important to periodically read the value stored on the cell and write the restore value back to the cell for DRAM to function as a reliable memory. This requirement is the reason why DRAM is classified as Dynamic RAM and the overall operation to fulfill such requirements is called Refresh.
To refresh the cells, only the functionality of activate and precharge commands are needed.
there are two different types of refresh modes. The first and the default type is auto-refresh, where the memory controller issues refresh commands periodically to indicate when the refresh should occur while the DRAM determines the rows to refresh.
Another type of refresh is self-refresh, which refreshes data when the DRAM is in a low power sleep mode. In this mode, refreshes are issued automatically by the DRAM without any additional command issued from the memory controller.

### 2.3 Data Transfer
Modern DRAM uses Double Data Rate (DDR) protocol to exchange data between the DRAM and the memory controller.

**page size** is the data fetched to the bitline sense amplifier or the number of cells connected to a selected row by the activate command. (1KB or 2KB)
Page size also denotes the number of sense amp required for supporting one wordline.

**Peak bandwidth** is calculated by a multiplication of data rate (per pin) and total pins.

**Bank grouping** makes a coupe of banks as a group, physically separating IO wires and allowing data to be sent independently.

**Channel** is a path to communicate between memory device and memory controller (global IO wire?), where an independent channel requires an independent memory controller.

### 2.4 Power consumption
We can break DRAM power into four categories; background, row, column, and refresh.
The background power is mostly leakage and depends on how many banks are activated and whether the device is in a special power-down mode.
The other three power categories are the dynamic power dissipated to perform the specific DRAM operations. Row power relates to activate and precharge commands, Column is the power spent to read and write data in and out of the DRAM, and
Refresh power is dissipated whenever periodic refresh is issued.

## 3. DRAM Design
### 3.1 Structural Organization
The cells are then densely packed with each other, forming a 2D-matrix of cells called a MAT.
The tight pitch of the cells leads to high resistance, which limits the size of a MAT (A typical configuration of MAT has 512 wordlines and 512 bitlines storing a total of 256 K bits).
Today’s Gb-scale DRAM contains thousands of MATs, where around 16 MATs are grouped into a sub-array and sub-arrays are then stacked on top of each other to form a sub-bank and depending on the size of the DRAM one or more sub-banks combine to form a bank.
Similar to the MAT, the bank also has a set of peripheral circuits labeled row and column decoder to select a group of cells and transfer data into or out of these selected cells.
A chip has multiple banks to provide the bank-level parallelism crucial to high performance DRAM and additional periphery circuit that contains pads or TSVs to communicate with the system.
The memory module used in modern computer systems is simply a collection of multiple DRAM chips with 64 bits of I/O per rank for the DDR4 DRAM.

### 3.2 Core Design
...

## 4. Collections for DRAM access info

| Link | Paper name | Accelerator type | DRAM category | DRAM read energy | DRAM write energy | DRAM bandwidth | DRAM info source |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSCC'25](https://ieeexplore.ieee.org/document/10904793) | T-REX | ASIC/Sim | LPDDR3 SDRAM | 3.7pJ/b (per pin) | ? | 6.4GB/s | [ISSCC'12](https://ieeexplore.ieee.org/document/6176871) |
| [NVIDA docs](https://research.nvidia.com/sites/default/files/pubs/2017-10_Fine-Grained-DRAM%3A-Energy-Efficient/oconnor_and_chatterjee.micro2017.pdf) | FGDRAM | ASIC | HBM/DDR | 14.0pj/bit | ? | 536GB/s | Figure1: bandwidth vs energy trade-off |
| [Stanford Thesis](https://stacks.stanford.edu/file/yp843xn4828/Heonjae_Thesis-augmented.pdf) | 32Gb LPDDR4 (SDP) | simulation | LPDDR4 | 14.0pj/bit | ? | 3.2Gbps/pin | DramDSE (simulation) |
| [Stanford Thesis](https://stacks.stanford.edu/file/yp843xn4828/Heonjae_Thesis-augmented.pdf) | 4x8Gb LPDDR4 (QDP) | simulation | LPDDR4 | 13.0pj/bit | ? | 3.2Gbps/pin | DramDSE (simulation) |
| [MICRO'10](https://ieeexplore.ieee.org/document/5695550) | 2G DDR3 | simulation | simulation (65nm)  | 9.0bJ/bit | ? | 2.6GB/s | IDD7RW |
| [MICRO'10](https://ieeexplore.ieee.org/document/5695550) | 8G DDR4 | simulation | expected (45nm)  | 4.5bJ/bit | ? | 6.2GB/s | IDD7RW |

## 5. DRAM/SRAM Simulators

### 5.1 Papers

#### 5.1.0 Fundamentals

1. [ISSCC '14] [Computing's Energy Problem](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6757323): This paper presents the DRAM access energy as about 10pJ/b if the DRAM access I/O is improved.


#### 5.1.1 CACTI

1. [ISCA '18] [Bitfusion](https://github.com/hsharma35/bitfusion): This uses CACTI for on-chip buffers (SRAM).

2. [MICRO '22] [ANT](https://github.com/clevercool/ANT-Quantization): This uses the same thing as Bitfusion.

3. [TCOM '21] [ZigZag](https://github.com/KULeuven-MICAS/zigzag): This uses CACTI for all memory access energies.

4. [HPCA '24] [Spark](https://ieeexplore.ieee.org/abstract/document/10476472)

5. [ASPLOS '20] [Interstellar](https://github.com/xuanyoya/Interstellar-CNN-scheduler): This work provides energy table, and this work uses CACTI for SRAM.

6. [ISCA '23] [SPADE](https://dl.acm.org/doi/pdf/10.1145/3579371.3589054): This work uses CACTI for SRAM (L2).

7. [TACO '22] [SCNN](https://dl.acm.org/doi/full/10.1145/3532863): This work uses CACTI for SRAM/DRAM.

8. [TCASAI '25] EvGNN (no Github): This work uses CACTI for SRAM/DRAM.

9. [ISCA '24] Trapezoid (no Github): This work uses CACTI for SRAM and RAMBUS report for HBM (HBM3E 768GB/s).

10. [MICRO '22] [Timeloop + Accelergy](https://github.com/Accelergy-Project/accelergy-timeloop-infrastructure)


#### 5.1.2 DRAMsim
1. [ASPLOS '24] [NeuPIM](https://github.com/casys-kaist/NeuPIMs): This work uses Micron's report as well.

2. [HPCA '25] [BitMod](https://github.com/abdelfattah-lab/BitMoD-HPCA-25/tree/main/bitmod_sim/mem): This work uses CACTI for on-chip buffer (SRAM) and DRAMsim for DRAM power.

3. [HPCA '25] [Prosperity](https://github.com/dubcyfor3/Prosperity): This work uses CACTI for on-chip buffer (SRAM) and DRAMsim for DRAM power.



#### 5.1.3 USIMM
1. [ISCA '22] Hydra (no github): This work uses CACTI for SRAM and USIMM for DRAM.



#### 5.1.4 Ramulator/DRAMpower
1. [HPCA '24] [CoMeT](https://github.com/CMU-SAFARI/CoMeT)

2. [MICRO '21] ESCALATE (no github): This work uses CACTI for SRAM and DRAMpower/ramulator for DRAM.

3. [HPCA '24] MEGA (no github): This work uses the HyGCN's methodology.

4. [HPCA '20] HyGCN (no github): This work uses CACTI for SRAM, Ramulator for HBM simulation, 7 pJ/b for HBM access energy. 

5. [ArXiv '25] [SCALE-Sim v3](https://github.com/scalesim-project/scale-sim-v3): This work uses Ramulator for DRAM behavior and energy.



### 5.2 CACTI results
 CACTI has analytical models for all the basic building blocks of a memory: decoder, sense-amplifier, crossbar, on-chip wires, DRAM/SRAM cell, and latch.
 I wrote this section based on the paper ([CACIT version 7.0](https://dl.acm.org/doi/10.1145/3085572)) and the [gitbub](https://github.com/HewlettPackard/cacti).

```console
git clone https://github.com/HewlettPackard/cacti
cd cacti
make
./cacti -infile cache.cfg
```

The simulation results are reported in cache.cfg.out.

Note that Bitfusion uses 4096b page size.

| Ref | MEM type | DRAM type | Cache type | Cache level | Size (B) | Tech (nm) | Block size (B) | In/Out width (b) | Dynamic read energy (pJ/b) | Dynamic write energy (pJ/b) | Leakage power per bank (mW) | Area (mm^2) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Bitfusion | SRAM | DDR3 | ram | L3 | 512 | 45 | 4 | 32 | 0.0331553125 | 0.0730840625 | 1.03183 | 0.007603 |
| Bitfusion | SRAM | DDR3 | ram | L3 | 128 | 45 | 4 | 32 | 0.021365 | 0.028906 | 0.259873 | 0.00149898 |
| Bitfusion | SRAM | DDR3 | ram | L3 | 8192 | 28 | 64 | 512 | 0.0347046 | 0.048606640625 | 5.75335 | 0.0555835 |
| Bitfusion | SRAM | DDR3 | ram | L3 | 4096 | 28 | 64 | 512 | 0.03299296875 | 0.039730859375 | 3.11199 | 0.0460667 |
| Bitfusion | SRAM | DDR3 | ram | L3 | 128 | 28 | 4 | 32 | 0.0092170 | 0.0129976 | 0.136585 | 0.00102563 |

Note that CACTI is limited by 1 GB (8Gb) !!!!


### 5.3 MICRON power calculation

MICRON provides EXCEL files to calculate dram power, which is used for the [paper](https://stacks.stanford.edu/file/yp843xn4828/Heonjae_Thesis-augmented.pdf).  

$`\text{ACT energy per cmd} = (VDD * (IDD0-IDD3N) + VPP * (IPP0-IPP3N)) * tRAS`$  
$`\text{PRE energy per cmd} = (VDD * (IDD0-IDD2N) + VPP * (IPP0-IPP2N)) * tRP`$  
$`\text{READ energy per cmd} = (VDD * (IDD4R-IDD3N) + VPP * (IPP4R-IPP3N)) * (nBL/8) * tCK`$  
$`\text{WRITE energy per cmd} = (VDD * (IDD4W-IDD3N) + VPP * (IPP4W-IPP3N)) * (nBL/8) * tCK`$  
$`\text{REF energy per cmd} = (VDD * (IDD5B-IDD3N)  + VPP * (IPP5B-IPP3N) / tREFI) * tRFC`$  
$`\text{ACT BG powerdown power} = (VDD * IDD3P + VPP * IPP3N) * tCK`$  
$`\text{ACT BG standby power} = (VDD * IDD3N + VPP * IPP3N) * tCK`$  
$`\text{IDLE BG standby power} = (VDD * IDD2N + VPP * IPP3N) * tCK`$  
$`\text{PRE BG powerdown power} = (VDD * IDD2P + VPP * IPP3N) * tCK`$   


### 5.4 DRAMpower power calculation

DRAMpower is a trace-based DRAM simulator, considering both the DRAM core power and the interface power.  
DRAMpower is based on ***[TN‑40‑07: “Calculating Memory Power for DDR4 SDRAM”](https://www.mouser.com/pdfDocs/tn4007_ddr4_power_calculation.pdf?srsltid=AfmBOop4ORXwCwigcSsRLG93RlsHfWM2eV5EJg8Ywips-wEnosf8Fj8j)*** and ***JEDEC’s DDR4 standard (JESD79‑4)***.  
DRAMpower provides DDR4/DDR5/LPDDR4/LPDDR5 IDD specifications.

$`I_{rho} = rho * (IDD3N-IDD2N) + IDD2N`$  
$`I_{\theta} = (IDD0 * (tRP + tRAS) - I_{beta} * tRP) / tRAS`$  
$`I1 = (IDD3N + (nBANK - 1) * (rho * (IDD3N-IDD2N) + IDD2N)) / nBANK`$  
$`\text{ACT energy per cmd} = VDD * (I_{\theta}-I1) * tRAS`$  
$`\text{PRE energy per cmd} = VDD * (I_{beta} - IDD2N) * tRP`$  
$`\text{READ energy per cmd} = VDD * (IDD4R-IDD3N) * BurstLength / DateRate * tCK`$  
$`\text{WRITE energy per cmd} = VDD * (IDD4W-IDD3N) * BurstLength / DateRate * tCK`$  
$`\text{REF ab energy per cmd} = VDD * (IDD5B-IDD3N) / nBANK * tRFC`$  
$`\text{ACT BG shared energy per cycle} = VDD * I_{rho} * nDevice * tCK`$  
$`\text{ACT BG star energy per cycle} = VDD * (IDD3N-I_{rho}) / B * tCK`$  
$`\text{PRE BG energy per cycle} = (VDD * IDD2N) / B * tCK`$  

### 5.5 Ramulator2 power calculation

Ramulator2 is also a trace-based DRAM simulator.  
Ramulator2 provides only DDR4/DDR5 IDD specifications for the built-in power calculator.  
For LPDDR and HBM power/energy estimation, you need to get their IDD specifications or you have to use another tool like DRAMpower.  
  
$`\text{ACT energy per cmd} = (VDD * (IDD0-IDD3N) + VPP * (IPP0-IPP3N)) * tRAS`$  
$`\text{PRE energy per cmd} = (VDD * (IDD0-IDD2N) + VPP * (IPP0-IPP2N)) * tRP`$  
$`\text{READ energy per cmd} = (VDD * (IDD4R-IDD3N) + VPP * (IPP4R-IPP3N)) * nBL * tCK`$  
$`\text{WRITE energy per cmd} = (VDD * (IDD4W-IDD3N) + VPP * (IPP4W-IPP3N)) * nBL * tCK`$  
$`\text{REF energy per cmd} = (VDD * IDD5B + VPP * IPP5B) * tRFC`$  
$`\text{ACT BG energy per cycle} = (VDD * IDD3N + VPP * IPP3N) * tCK`$  
$`\text{PRE BG energy per cycle} = (VDD * IDD2N + VPP * IPP2N) * tCK`$  
  

### 5.6 IDD specifications

| Source | Ramulator2  | 
| ---    | ---         |
| MEM    | DDR4_8Gb_x8 |
| VDD    | 1.2V        |
| IDD0   | 60 mA       |
| IDD2N  | 50 mA       |
| IDD3N  | 55 mA       |
| IDD4R  | 145 mA      |
| IDD4W  | 145 mA      |
| IDD5B  | 362 mA      |
| VPP    | 2.5 V       |
| IPP0   | 3 mA        |
| IPP2N  | 3 mA        |
| IPP3N  | 3 mA        |
| IPP4R  | 3 mA        |
| IPP4W  | 3 mA        |
| IPP5B  | 48 mA       |


### 5.7 DRAM command timing

Each of Read/Write commands is used for a burst access.
For example, for 8 burst lengths, each of Read/Write commands takes 4 cycles (8b x 8 = 64b access).

<center><img src="./figures/row-access time.png" width="100%" height="100%"></center>

**Row Column Delay (RCD)**: Time for activating row -> sense amplifier after issuing row access command. After tRCD, it is possible to communicate between sense amplifier and memory controller.  
**Row Access Strobe (RAS)**: Time for requiring prechage command after issuing row access command.  
**Row Precharge Delay (RP)**: Time interval between precharge and activate.  
**Write Recovery (WR)**:  
**Column Access Strobe (CAS) == Column Access Strobe Latency (CL)**: BUS Delivery time after a read command.  
**Column-to-Column delay (CCD)**: Time for the least interval between read command.  
**Burst time (BURST)**: BUS time for Burst data.  
**Column Write Delay (CWD) == Column Write Latency (CWL)**: BUS Delivery time after a write command.  
**Refresh Cycle (RFC)**: Time for performing the refresh command.  
**Refresh time Interval (REFI)**


### 5.8 DRAM energy results

Based on Ramulator2, I consider "open row for (tRAS-tRCD)" and "only max(tCCD,tBURST) for stream commands" ([ISCA'23](https://dl.acm.org/doi/pdf/10.1145/3579371.3589101), [ISCA'12](https://ieeexplore.ieee.org/document/6237032), and [Blog Post](https://ryotta-205.tistory.com/86)).  
But, in fact, Ramulator2 uses four stream column access commands for one active commands.  
Activated cycles are accumulated by total RAS time.  
Idle cycles also includes cycles for ACT cmd -> PRE cmd (nRP).  

| Source                        | Ramulator2  | 
| ---                           | ---         |
| MEM                           | DDR4_8Gb_x8 |
| Spec                          | 2400R       |
| CLK (ns)                      | 0.833       |
| Access type (Trace)           | Stream      |
| Transmitted bits per cmd      | DQ x BL = 64|
| nBL                           | 8 (4 cycles)|
| nRS                           | 16          |
| nRCD                          | 16          |
| nCCD                          | 16          |
| nCL                           | 4           |
| nRC                           | 55          |
| nRAS                          | 39          |
| nRP                           | 16          |
| nRFC                          | 433         |
| nREFI                         | 9364        |
| Sim RD ACT->PRE cycles (# cmd)| 43 (4 RD)   |
| Sim RD ACT->ACT cycles (# cmd)| 59 (4 RD)   |
| Sim WR ACT->PRE cycles (# cmd)| 58 (4 WR)   |
| Sim WR ACT->ACT cycles (# cmd)| 84 (4 WR)   |
| READ energy (pJ/b)            | 5.62275     |
| Write energy (pJ/b)           | 5.62275     |
| avg. ACT energy (pJ/b)        | 0.75973516  |
| avg. PRE energy (pJ/b)        | 0.62475     |
| avg. REF energy (pJ/b)        | 0.619968    |
| Act background (pJ/cycle)     | 61.2255     |
| Idle background (pJ/cycle)    | 56.2275     |

#### Background energy calculation (ex. Ramulator2):  
(READ) Activation Background energy = Total read bit / 256 * 43 * 61.2255 (pJ)  
(WRITE) Activation Background energy = Total write bit / 256 * 58 * 61.2255 (pJ)  
CORE total latency = CORE total cycle * CORE clock (ns)  
MEM total latency = CORE total latency  
MEM total cycles = MEM total latency / 0.833  
MEM idle cycles = MEM total cycles - (Total read bit / 256 * 43 + Total write bit / 256 * 58)  
idle Background energy = MEM idle cycles * 56.2275 (pJ)  