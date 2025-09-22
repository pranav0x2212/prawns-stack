# RISC vs CISC Design

| Feature                                | RISC (Reduced Instruction Set Computer)              | CISC (Complex Instruction Set Computer)              |
|----------------------------------------|------------------------------------------------------|------------------------------------------------------|
| **Instruction Set Complexity**         | Small, simple, uniform instructions                  | Large set of complex instructions                    |
| **Direct Memory Access vs Load/Store** | Load/Store: only load/store instructions access memory | Direct memory access allowed for many instructions   |
| **Addressing Modes**                   | Few simple addressing modes                          | Many complex addressing modes                        |
| **Clock Cycles per Instruction**       | Mostly single-cycle execution                        | Multiple cycles per instruction                      |
| **Instruction Format**                 | Fixed-length (e.g., 32-bit in RISC-V)                | Variable-length (1–15 bytes in x86)                  |
| **Execution Mode**                     | Pipelined execution is natural                       | Pipelining is harder due to variable-length and complex ops |
| **On-chip Registers**                  | Large number of general-purpose registers            | Fewer registers, more reliance on memory             |
| **Decoder & Control Complexity**       | Simple decoder and control logic                     | Complex decoder, microcode often used                |

## Explanation

- **Instruction Set Complexity:**  
  RISC instructions perform simple, single operations (load, store, add, branch). CISC instructions can perform multiple operations within a single instruction.  

- **Memory Access:**  
  RISC follows a **load/store architecture**: memory is only accessed through explicit load and store instructions. CISC instructions often directly manipulate memory operands.

- **Addressing Modes:**  
  RISC typically has a limited set (e.g., immediate, register, displacement). CISC provides many addressing modes, allowing compact but more complex code.

- **Clock Cycles:**  
  RISC aims for one instruction per cycle (CPI ≈ 1). CISC instructions may take multiple cycles since they perform more work.

- **Instruction Format:**  
  Fixed-length in RISC simplifies fetch/decode and supports pipelining. CISC’s variable length makes decoding harder.

- **Pipelining:**  
  RISC instructions are pipeline-friendly due to their uniformity. CISC requires extra handling, making deep pipelines less efficient.

- **Registers:**  
  RISC designs include many registers to reduce memory traffic. CISC has fewer registers, relying more on memory accesses.

- **Decoder Complexity:**  
  RISC decoding is simple and often hardwired. CISC decoders are complex, frequently relying on microcode to translate instructions into simpler internal ops.

---

# Von Neumann vs Harvard Architecture

## Von Neumann Architecture

- **Single memory space** holds both instructions and data.
- **Single bus** is used for fetching instructions and reading/writing data.
- **Implication:** CPU can either fetch an instruction or access data at any given time → bottleneck known as the **Von Neumann bottleneck**.

**Analogy:**
Imagine a single-lane road where cars represent instructions and data. Only one car can pass at a time → congestion happens if there's a lot of traffic.

**Diagram:**
```
       +----------------+
       |     Memory     |  <- stores both instructions & data
       +----------------+
             |
             |
          +-----+
          | CPU |
          +-----+
```

## Harvard Architecture

- **Separate memory spaces** for instructions and data.
- **Separate buses** allow simultaneous instruction fetch and data read/write.
- **Implication:** Reduces memory access bottlenecks → higher throughput.

**Analogy:**
Imagine a dual-lane road: one lane for instructions, one lane for data. Cars can move simultaneously → smoother traffic.

**Diagram:**
```
       +----------------+       +----------------+
       | Instruction Mem|       |    Data Mem    |
       +----------------+       +----------------+
               |                        |
         Instruction Bus          Data Bus
               |                        |
               +---------- CPU ----------+
                          |
                   ALU / Registers / Control
```

## Comparison Table

| Feature | Von Neumann | Harvard |
|---------|-------------|---------|
| **Memory** | Shared (code + data) | Separate (code & data) |
| **Bus** | Single | Separate buses |
| **Simultaneous access** | No | Yes |
| **Complexity** | Simpler | Slightly more complex |
| **Performance** | Limited by bus | Higher throughput |
| **Typical Use** | General-purpose CPUs | Microcontrollers, DSPs |

## Design Note:

- Modern CPUs often use a **modified Harvard approach**: separate caches for instructions and data but a unified main memory.
- Von Neumann simplicity makes it easier to program, but throughput can be limited.

---

# Basic Blocks of a Processor

```

                   +-------------------+
                   | Instruction Memory|
                   +---------+---------+
                             |
                             v
   +---------------------------------------------+
   |                  Processor                  |
   |                                             |
   |   +---------------+    +----------------+   |
   |   |   Control     |    |    Datapath    |   |
   |   |     Unit      |    |                |   |
   |   +-------+-------+    |  +----+        |   |
   |           |            |  | PC |        |   |
   |           |            |  +----+        |   |
   |           |            |  +-----------+ |   |
   |           |            |  | Register  | |   |
   |           |            |  |   File    | |   |
   |           |            |  +-----------+ |   |
   |           |            |  +-----------+ |   |
   |           +----------->|  |    ALU    | |   |
   |                        |  +-----------+ |   |
   |                        +----------------+   |
   +---------------------------------------------+
                             |
                             v
                   +-------------------+
                   |    Data Memory    |
                   +-------------------+
                             |
                             v
                   +-------------------+
                   |        I/O        |
                   +-------------------+
```

# Basic Blocks of a Processor

- **Datapath**
    
    - **Definition:** The datapath is the part of the CPU that **actually performs operations on data**. It consists of components that carry out arithmetic, logic, data storage, and data transfer. Essentially, it’s the “engine” of the processor that moves and manipulates data according to instructions.
        
    - **ALU (Arithmetic Logic Unit):** Performs arithmetic (add, subtract, multiply, divide) and logical (AND, OR, shifts) operations.
        
    - **Registers:** Small, fast storage inside the CPU used to hold operands, intermediate results, and outputs of computations.
        
    - **Buses:** Pathways (data, address, control) that transfer data between the components of the CPU and memory.
        
- **Control Unit**
    
    - **Control signals:** Direct the datapath (e.g., select ALU operation, enable register write).
        
    - **Instruction decode:** Converts binary instructions into control actions to operate the datapath and memory.
        
- **Memory**
    
    - **Instruction Memory (IMEM):** Stores the program (machine code instructions).
        
    - **Data Memory (DMEM):** Stores program data (variables, arrays).
        
- **I/O Interfaces**
    
    - Handle communication with peripherals (keyboard, display, storage, network).
        
    - Provide input to the processor and output from the processor.
        
- **Pipeline Registers**
    
    - Temporary storage between pipeline stages.
        
    - Ensure smooth instruction flow by holding intermediate results (e.g., IF/ID, ID/EX, EX/MEM, MEM/WB in a 5-stage pipeline).

---

# Logic Design Conventions

## 1. Types of Logic Elements

1. **Combinational Logic**
    
    - Output depends **only** on current input.
        
    - No internal storage.
        
    - Example: ALU, MUXes, adders.
        
    - **Truth table example:**

```text
A B | AND OR XOR
0 0 | 0   0   0
0 1 | 0   1   1
1 0 | 0   1   1
1 1 | 1   1   0
```

2. **Sequential / State Elements**
    
    - Have **internal storage**.
        
    - Output depends on **input + stored state**.
        
    - Example: Registers, program counter, memory elements.
        

---

## 2. Clocking Methodology

- **Edge-triggered** flip-flops commonly used for state elements.
    
- **Setup time**: Minimum time input must be stable **before clock edge**.
    
- **Hold time**: Minimum time input must remain stable **after clock edge**.
    
- Clock period ≥ longest combinational path + setup time.
    

---

## 3. Datapath Conventions

- Multiplexers (MUX) select inputs/outputs for shared hardware resources:
    
    - ALU second operand: register vs immediate
        
    - Register write-back: ALU vs memory
        
    - PC update: sequential (`PC+4`) vs branch/jump
        
- Control signals drive MUXes and functional units consistently.
    
- Example: `ALUSrc = 1` → use immediate as ALU second operand
    

---

## 4. Design Principles Applied

- **Simplicity favors regularity**: same instruction format fields, uniform control signals
    
- **Make the common case fast**: optimize datapath for frequent instructions (`add`, `lw`, `sw`)
    
- **Predictable clocking**: ensures correct sequencing across combinational and sequential elements