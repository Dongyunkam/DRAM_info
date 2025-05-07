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


## 2. Collections for DRAM access info

| Link | Name | Accelerator type | DRAM category | DRAM read energy | DRAM write energy | DRAM bandwidth | DRAM source |
| --- | --- | --- | --- | --- | --- | --- |
| [ISSCC'25](https://ieeexplore.ieee.org/document/10904793) | T-REX | ASIC/Sim | LPDDR3 SDRAM | 3.7pJ/b (per pin) | ? | 6.4GB/s | [ISSCC'12](https://ieeexplore.ieee.org/document/6176871) |