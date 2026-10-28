# CPU Architectures
### **I. General-Purpose Computing and Mainstream Instruction Set Architectures**

#### **1. x86 Architecture**
* **Core definition**: A proprietary processor architecture based on the complex instruction set (CISC), dominating the desktop and server markets.
* **Derivation logic**: The hardware decodes and executes complex instructions directly at the low level $\rightarrow$ strong single-core performance and high clock frequencies, but complex circuitry with enormous power draw and heat output; long-term binding to the mainstream operating system ecosystem $\rightarrow$ deep low-level coupling between hardware and software.
* **Key takeaway**: Extreme peak performance and very strong historical backward compatibility; the price is high energy consumption, high heat output, and heavy dependence on active cooling.

#### **2. ARM Architecture**
* **Core definition**: A power-efficient processor architecture based on the reduced instruction set (RISC), dominating mobile and embedded computing.
* **Derivation logic**: A streamlined, uniform instruction set $\rightarrow$ fewer execution steps per instruction and low transistor overhead $\rightarrow$ minimal energy loss and extremely high performance per watt (energy efficiency) $\rightarrow$ sustained stable operation over long periods without fan-based active cooling; later, horizontal scaling of compute through integrated dedicated GPUs and AI engines.
* **Key takeaway**: Its core advantage lies in extreme "performance per watt" and low-power control, making it the compute foundation of the mobile internet and all-day battery-life devices.

#### **3. Apple Silicon (Customized ARM SoC Architecture)**
* **Core definition**: A complete system-on-a-chip (SoC) with a unified memory architecture, deeply customized on the ARM instruction set.
* **Derivation logic**: Heterogeneous units such as the CPU, GPU, neural network engine, and media encoders are tightly integrated onto a single chip $\rightarrow$ shorter physical communication distances, greatly reducing transmission latency and wasted power; a unified memory architecture (UMA) breaks the physical separation between system memory and video memory $\rightarrow$ multiple compute units share one high-speed memory pool, eliminating the data-copying bottleneck.
* **Key takeaway**: Through heterogeneous integration and memory sharing, it unifies high throughput, low latency, and high energy efficiency, delivering workstation-class performance and ultra-long battery life in thin-and-light devices.

#### **4. RISC-V Architecture**
* **Core definition**: An open-source, free, modular modern reduced instruction set (RISC) architecture.
* **Derivation logic**: It follows the efficient-execution principle of reduced instruction sets $+$ adopts a building-block design of "a minimal base instruction set $+$ optional extension modules (floating point, security, AI acceleration, etc.)" $\rightarrow$ hardware can be customized on demand, and it completely breaks free of commercial giants' patent barriers and high licensing fees.
* **Key takeaway**: Its core value lies in open-source freedom and architectural flexibility; it greatly lowers the barrier to chip development and dependence on proprietary architectures, and is widely used in research and innovation, scenario-specific customization, and fields requiring independent, controllable technology.

---

### **II. Classic Reduced Instruction Set (RISC) Evolutionary Branches**

#### **1. MIPS Architecture**
* **Core definition**: An early classic fixed-length reduced instruction set (RISC) architecture (Note: the subtitle's 'risk chip' was a speech-recognition error for 'RISC chip').
* **Derivation logic**: A lean, highly regular architecture $\rightarrow$ high instructions-per-cycle efficiency (IPC) and highly predictable pipeline behavior $\rightarrow$ developers can perform extreme low-level optimization within tight memory and hardware budgets, squeezing out the full potential of real-time 3D geometry and physics computation.
* **Key takeaway**: It compensated for hardware shortcomings in absolute compute power and memory capacity with an elegant, compact architecture, playing a key role in the early evolution of 3D graphics computing.

#### **2. PowerPC Architecture**
* **Core definition**: A high-performance reduced instruction set (RISC) architecture pursuing a balance between performance, size, and power consumption.
* **Derivation logic**: It balances 3D graphics, physics simulation, and logic compute within a compact space and strict thermal limits $\rightarrow$ opens up low-level control of memory and graphics pipelines, raising the ceiling for hardware optimization; but because it departed from mainstream general-purpose architecture standards $\rightarrow$ cross-platform software porting costs are high, and it is easily constrained by the bandwidth of peripheral storage media.
* **Key takeaway**: A model of compact engineering trade-offs, combining high compute density with low-noise cooling; however, its non-general-purpose nature limited the expansion of its cross-platform ecosystem.

---

### **III. Dedicated Acceleration and Heterogeneous Computing Architectures**

#### **1. Custom Graphics Processor Architecture (GPU)**
* **Core definition**: A dedicated architecture designed for massively parallel computing and real-time graphics rendering pipelines.
* **Derivation logic**: The CPU handles serial logic and system scheduling, while the GPU takes over highly concurrent graphics workloads (high-resolution textures, ray tracing, complex lighting calculations); fixed hardware specifications $+$ coordination between unified memory and ultra-high-speed storage pipelines $\rightarrow$ eliminates hardware configuration fragmentation, allowing developers to perform targeted low-level optimization against deterministic hardware.
* **Key takeaway**: Integrated hardware-software co-optimization lets fixed-specification hardware deliver graphics performance beyond general-purpose components of the same class, at the cost of sacrificing hardware upgradeability.

#### **2. Dedicated AI Accelerator Architecture (NPU / Tensor Architecture)**
* **Core definition**: A compute acceleration architecture designed specifically for machine learning, deep learning, and large-scale neural networks.
* **Derivation logic**: It strips out redundant general-purpose logic control and integrates massive numbers of parallel compute cores (such as CUDA cores) with dedicated matrix operation units (such as Tensor cores) $\rightarrow$ hardware-level parallel acceleration for low-precision floating-point operations and high-dimensional matrix multiply-accumulate $\rightarrow$ compressing the training and inference time of massive datasets by several orders of magnitude.
* **Key takeaway**: The datacenter-class compute foundation of modern artificial intelligence, trading high hardware cost and power consumption for extreme parallel throughput and AI acceleration capability.

#### **3. FPGA (Field-Programmable Gate Array)**
* **Core definition**: A semi-custom integrated circuit whose hardware logic circuits can be dynamically programmed and reconfigured after manufacture.
* **Derivation logic**: Composed of configurable logic gate arrays and programmable interconnect networks $\rightarrow$ the underlying hardware logic can be changed through reprogramming without replacing the physical chip, enabling pure-hardware parallel processing of multiple data streams with millisecond-level ultra-low latency response; but the programmable redundant structure brings extra area and power overhead $\rightarrow$ its energy efficiency for a single fixed task is lower than that of an application-specific integrated circuit (ASIC), and the hardware engineering barrier is extremely high.
* **Key takeaway**: It combines hardware-level low-latency parallelism with software-level reconfigurable flexibility; it is the optimal solution during the pre-finalization stage of an algorithm (prototype validation, real-time sensor fusion) and in high-reliability, low-latency domains (communications, aerospace, industrial control), and is usually replaced by a custom ASIC once designs are finalized for mass production.

---

### **IV. Frontier Non-Classical Computing Architectures**

#### **1. Superconducting Quantum Computing Architecture**
* **Core definition**: A non-classical computing architecture based on quantum mechanics principles, using the qubit as its basic unit.
* **Derivation logic**: It breaks the mutually exclusive limitation of traditional binary (0 or 1), using quantum superposition and entanglement to simultaneously represent and process an exponentially large state space $\rightarrow$ achieving large-scale parallel probabilistic problem solving; but quantum states are extremely fragile (prone to decoherence) $\rightarrow$ it is highly dependent on an ultra-low-temperature environment near absolute zero and on strict anti-vibration and anti-electromagnetic-interference controls.
* **Key takeaway**: On certain ultra-complex problems (combinatorial optimization, molecular and materials simulation, cryptography), it offers a compute advantage that classical supercomputers can hardly match (some problems theoretically achieve exponential speedup, such as prime factorization of large numbers; the actual speedup for combinatorial-optimization problems remains debated), but limited by physical fragility and demanding operating conditions, it currently serves only as specialized research and specific computing infrastructure.

---

### **V. Core Logic of Architecture Selection: A Comparison**

| Architecture Type | Instruction Set / Computing Paradigm | Core Advantages | Core Bottlenecks / Costs | Typical Positioning |
| :--- | :--- | :--- | :--- | :--- |
| **x86** | CISC (complex instruction set) | Strong single-core peak performance, excellent ecosystem compatibility | High power draw, high heat output, heavy dependence on cooling | Desktop PCs, gaming laptops, general-purpose servers |
| **ARM** | RISC (reduced instruction set) | Extreme performance per watt, low power, long battery life | Legacy desktop software requires translation or rework | Mobile devices, thin-and-light devices, energy-efficient computing |
| **Apple Silicon** | Custom ARM + SoC/UMA | High heterogeneous cooperation efficiency, eliminates the memory/VRAM bottleneck | Highly closed hardware and software, no user upgrades or expansion | High-performance mobile workstations, creative production devices |
| **RISC-V** | Open-source modular RISC | No licensing fees, highly customizable, independent and controllable | High-end performance and software ecosystem still catching up | Embedded systems, research and development, custom chips |
| **GPU / AI accelerator card** | SIMT / matrix parallel computing | Extremely high parallel throughput, extreme acceleration for dedicated workloads | Weak general-purpose logic processing, high power, high cost | Graphics rendering, large-model training and inference |
| **FPGA** | Reconfigurable hardware logic arrays | Hardware-level ultra-low latency, dynamically reconfigurable logic | Mass-production energy efficiency and cost inferior to ASICs, difficult development | Early prototype validation, real-time communications, industrial control |
| **Quantum chip** | Qubit superposition-state computing | Exponential generational compute advantage on specific complex problems | Extremely fragile, requires near absolute zero, poor generality | Molecular simulation, combinatorial optimization, frontier physics research |
