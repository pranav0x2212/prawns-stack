# RISC-V Numericals - Unit 2

This file contains numerical problems covering pipelining concepts for exam preparation. Each problem includes step-by-step solutions and explanations.

---

## Q1: Structural Hazard Detection 

**Given:**
- Instructions:
  ```assembly
  I1: lw x1, 0(x2)  
  I2: add x3, x4, x5
  ```
- 5-stage pipeline: IF, ID, EX, MEM, WB
- Assume a single memory unit is used for both instructions

**Task:**
1. Detect if a structural hazard occurs
2. Identify the stage causing the hazard

<details>
<summary>Answer</summary>

**Solution:**
- **Hazard:** Structural hazard
- **Reason:** Both I1 and I2 require the memory unit at the same cycle (I1 in MEM stage, I2 in IF stage if instruction fetch uses the same memory), causing a resource conflict.

**Explanation:**
- In a 5-stage pipeline:
  - I1: MEM stage accesses memory (cycle 4)
  - I2: IF stage accesses memory (cycle 2)
- If there is only one memory port for both instruction and data, simultaneous access is not possible → structural hazard

</details>

---

## Q2: Pipeline Timing Diagram

**Given:**
- Instructions:
  ```assembly
  I1: add x5, x1, x2  
  I2: sub x6, x5, x3
  ```
- 5-stage pipeline with IF, ID, EX, MEM, WB
- Forwarding not implemented

**Task:**
1. Draw the pipeline timing diagram (clock cycles vs stage occupancy)
2. Indicate where stalls occur

<details>
<summary>Answer</summary>

**Solution:**
- Stall occurs because I2 needs x5 which is produced by I1 in WB stage

**Timing Diagram:**
```
Cycle:  1   2   3   4   5   6   7   8
I1:    IF  ID  EX MEM  WB
I2:    IF  ST  ST  ID  EX MEM  WB
```

**Explanation:**
- "ST" = stall
- I2 must wait 2 cycles for I1 to complete and write x5 to register file
- Without forwarding, I2 stalls in IF stage until x5 is available

</details>

---

## Q3: Structural Hazard Detection

**Given:**
- Instructions:
  ```assembly
  I1: lw x1, 0(x2)  
  I2: lw x3, 4(x2)
  ```
- Single memory port for both instruction and data memory (Harvard assumption removed)

**Task:**
1. Identify if a structural hazard occurs
2. Suggest a solution

<details>
<summary>Answer</summary>

**Solution:**
- **Hazard:** Structural hazard
- **Reason:** Both I1 and I2 need memory access in MEM stage simultaneously

**Detailed Analysis:**
- I1 accesses data memory in MEM stage (cycle 4)
- I2 needs instruction fetch in IF stage (cycle 4)
- Single memory port cannot handle both requests

**Solutions:**
1. Add **separate instruction/data memories** (Harvard architecture)
2. Insert a **stall** to serialize memory accesses
3. Use **dual-port memory**

</details>

---

## Q4: Non-Pipelined Delay Calculation

**Given:**
- Instruction stages and delays:
  - IF = 250 ps, ID = 150 ps, EX = 200 ps, MEM = 300 ps, WB = 100 ps
- Instructions:
  ```assembly
  I1: add x1, x2, x3  
  I2: lw x4, 0(x5)
  ```

**Task:**
Calculate clock period and execution time for non-pipelined processor for 2 instructions.

<details>
<summary>Answer</summary>

**Solution:**
- **Clock period** = sum of all stages = 250 + 150 + 200 + 300 + 100 = **1000 ps**
- **Execution time** = 2 instructions × 1000 ps = **2000 ps**

**Explanation:**
- In non-pipelined design, each instruction must complete all stages before next begins
- Clock period determined by sum of all stage delays
- Total time = number of instructions × clock period

</details>

---

## Q5: Pipelined Delay Calculation

**Given:**
- Stage delays same as Q4
- 5-stage pipeline
- Instructions:
  ```assembly
  I1: add x1, x2, x3  
  I2: lw x4, 0(x5)  
  I3: sub x6, x1, x7
  ```
- Assume stall for load-use hazard (1 cycle)

**Task:**
Compute pipeline clock cycle and total execution time for 3 instructions.

<details>
<summary>Answer</summary>

**Solution:**
- **Clock period** = max(stage delay) = max(250, 150, 200, 300, 100) = **300 ps**
- **Ideal cycles** without stalls = instructions + stages - 1 = 3 + 5 - 1 = 7
- **Load-use hazard** introduces 1 stall → total cycles = 8
- **Execution time** = 8 × 300 ps = **2400 ps**

**Detailed Analysis:**
- Pipeline clock limited by slowest stage (MEM = 300 ps)
- I3 depends on I1's result (x1), requiring 1 stall cycle
- Formula: Total time = (instructions + pipeline_depth - 1 + stalls) × clock_period

</details>

---

## Q6: Instruction-Specific Timing

**Given:**
- Stage delays:
  - IF = 250 ps, ID = 150 ps, EX = 200 ps, MEM = 300 ps, WB = 100 ps
- Instruction types:
  - I1: add (R-type) → MEM not used  
  - I2: lw (load) → all stages used  

**Task:**
1. Compute non-pipelined clock period for each instruction type
2. Explain why clock period is determined by the longest path

<details>
<summary>Answer</summary>

**Solution:**
- **Add (R-type) total** = IF + ID + EX + WB = 250 + 150 + 200 + 100 = **700 ps**
- **lw (load) total** = IF + ID + EX + MEM + WB = 250 + 150 + 200 + 300 + 100 = **1000 ps**
- **Non-pipelined processor** must adopt **longest instruction** delay as clock period → **1000 ps**

**Explanation:**
- All instructions share the same clock cycle in non-pipelined design
- Fastest instructions must "wait" for slowest instruction to complete
- This violates the "make common case fast" principle
- Clock period = max(all instruction execution times)

</details>