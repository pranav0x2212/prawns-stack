**Q1:** Which of the following is true for a non-pipelined processor?  
A) Multiple instructions are executed simultaneously  
B) Each instruction completes before the next starts  
C) Clock cycle is determined by shortest instruction  
D) Pipeline registers are heavily used

<details><summary>Answer</summary>B — Each instruction completes before the next starts. Non-pipelined design executes instructions sequentially.</details>

---

**Q2:** In a RISC-V processor, which stage calculates the effective address for load/store instructions?  
A) IF  
B) ID  
C) EX  
D) MEM

<details><summary>Answer</summary>C — The ALU in the EX stage computes the effective memory address.</details>

---

**Q3:** In a pipelined processor, the clock cycle is generally determined by:  
A) Sum of all stage delays  
B) Maximum delay among all stages  
C) Minimum stage delay  
D) Average of all stage delays

<details><summary>Answer</summary>B — Pipeline clock cycle is limited by the slowest stage (critical path).</details>

---

**Q4:** Which of the following is a primary benefit of pipelining?  
A) Reduced CPI  
B) Reduced total execution time for single instruction  
C) Eliminates hazards completely  
D) Reduces stage delays

<details><summary>Answer</summary>A — Pipelining reduces average CPI (Clock Cycles Per Instruction), allowing multiple instructions to overlap.</details>

---

**Q5:** Which of the following is **not** a RISC-V pipeline stage in a simple 5-stage design?  
A) IF  
B) ID  
C) EXE  
D) WB

<details><summary>Answer</summary>C — The correct stage name is EX (Execute), not EXE.</details>

---

**Q6:** What is the primary function of the IF stage?  
A) Execute ALU operations  
B) Read operands from registers  
C) Fetch instruction from memory  
D) Write results to registers

<details><summary>Answer</summary>C — IF (Instruction Fetch) fetches instructions from instruction memory.</details>

---

**Q7:** Which of the following causes a pipeline **stall**?  
A) Branch misprediction  
B) ALU addition  
C) Load instruction executed  
D) Register write

<details><summary>Answer</summary>A — Branch hazards may require stalling until branch outcome is known.</details>

---

**Q8:** In a non-pipelined processor, CPI is generally:  
A) Less than 1  
B) Equal to 1  
C) Equal to total stages per instruction  
D) Greater than 1

<details><summary>Answer</summary>B — Single-cycle non-pipelined processors complete one instruction per clock cycle (CPI = 1).</details>

---

**Q9:** Which register temporarily holds data read from memory in a load instruction?  
A) ALUOut  
B) MDR  
C) PC  
D) IR

<details><summary>Answer</summary>B — Memory Data Register (MDR) holds data fetched from memory before writing to the register file.</details>

---

**Q10:** In a 5-stage pipeline, which stage is responsible for updating the program counter after a branch?  
A) IF  
B) ID  
C) EX  
D) MEM

<details><summary>Answer</summary>C — EX stage computes branch target and decides whether PC should change.</details>

---

**Q11:** Which of the following correctly represents **non-pipelined execution time**?  
A) Max(stage delays) × instructions  
B) Sum(stage delays) × instructions  
C) Sum(stage delays)  
D) Max(stage delays)

<details><summary>Answer</summary>B — Non-pipelined execution time = number of instructions × sum of all stage delays.</details>

---

**Q12:** What is a **structural hazard**?  
A) Conflict over hardware resources  
B) Delay due to branch instruction  
C) Conflict over register values  
D) Compiler scheduling issue

<details><summary>Answer</summary>A — Structural hazards occur when two instructions need the same hardware resource at the same time.</details>

---

**Q13:** In a pipelined processor, a load-use hazard occurs when:  
A) Branch is mispredicted  
B) ALU takes too long  
C) Instruction immediately after load uses the loaded value  
D) Two instructions write to same register

<details><summary>Answer</summary>C — Load-use hazards require stalling until the loaded value is available.</details>

---

**Q14:** If each stage delay is 2 ns and there are 5 stages, approximate speedup of a pipelined processor (ignoring hazards) over non-pipelined is:  
A) 1×  
B) 2×  
C) 5×  
D) 10×

<details><summary>Answer</summary>C — Ideal speedup ≈ number of stages = 5×.</details>

---

**Q15:** Which of the following best describes the purpose of pipeline registers?  
A) Hold instructions permanently  
B) Reduce clock period  
C) Store intermediate values between stages  
D) Manage hazards automatically

<details><summary>Answer</summary>C — Pipeline registers store intermediate data and control signals between stages.</details>

---

**Q16:** In a 5-stage pipeline with stage delays: IF=2, ID=1, EX=2, MEM=3, WB=1 (ns), what is **non-pipelined execution time** for 10 instructions?  
A) 9×?  
B) 10×?  
C) 90 ns  
D) 100 ns

<details><summary>Answer</summary>C — Non-pipelined: sum of stage delays = 2+1+2+3+1=9 ns per instruction. Total = 10×9=90 ns.</details>

---

**Q17:** Same pipeline as Q16, what is **pipelined execution time** for 10 instructions (ignoring hazards)?  
A) 18 ns  
B) 30 ns  
C) 33 ns  
D) 36 ns

<details><summary>Answer</summary>B — Pipelined: clock cycle = max(stage delay) = 3 ns, total = 3×(10+5−1)=42 ns if considering filling latency; ignoring filling, approximate = 3×10=30 ns.</details>

---

**Q18:** Which of the following reduces branch penalties most effectively?  
A) Increasing clock speed  
B) Forwarding  
C) Branch prediction  
D) Stall insertion

<details><summary>Answer</summary>C — Branch prediction reduces unnecessary stalls by guessing branch outcomes.</details>

---

**Q19:** Forwarding is used in pipelines to:  
A) Reduce structural hazards  
B) Resolve data hazards without stalling  
C) Increase stage delays  
D) Manage memory alignment

<details><summary>Answer</summary>B — Forwarding allows using ALU results directly in next instruction to avoid stalls.</details>

---

**Q20:** In a pipelined processor, the **total speedup** is limited by:  
A) Number of instructions  
B) Stage with maximum delay  
C) Branch frequency  
D) Register file size

<details><summary>Answer</summary>B — Clock cycle time is limited by the slowest stage (critical path), limiting ideal speedup.</details>