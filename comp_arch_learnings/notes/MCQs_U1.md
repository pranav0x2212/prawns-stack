# Computer Architecture MCQs

This file contains MCQs for Unit-1 (a mixture of easy + mid + hard)

**Q1: Which of the following is NOT a basic block of a processor?**
A) ALU
B) Register File
C) Compiler
D) Control Unit

<details><summary>Answer</summary> C) Compiler Explanation: The compiler is software that generates instructions; it is not part of the processor hardware. </details>

---

**Q2: RISC stands for:**
A) Reduced Instruction Set Computer
B) Random Instruction Storage Computer
C) Rapid Instruction Set Control
D) None of the above

<details><summary>Answer</summary> A) Reduced Instruction Set Computer Explanation: RISC focuses on a smaller, simpler set of instructions for faster execution. </details>

---

**Q3: Which register in RV32I stores the return address?**
A) x0
B) x1
C) x2
D) x10

<details><summary>Answer</summary> B) x1 Explanation: x1 is the `ra` register, used to store the return address during function calls. </details>

---

**Q4: In Von Neumann architecture:**
A) Separate memory for instructions and data
B) Shared memory for instructions and data
C) No memory is used
D) Uses registers only

<details><summary>Answer</summary> B) Shared memory for instructions and data Explanation: Von Neumann architecture uses a single memory for both instructions and data, which can create the "Von Neumann bottleneck." </details>

---

**Q5: Which of the following is a logical operation?**
A) ADD
B) SUB
C) AND
D) LW

<details><summary>Answer</summary> C) AND Explanation: Logical operations include AND, OR, XOR, and NOT; ADD/SUB are arithmetic, LW is memory. </details>

---

**Q6: Two's complement is used to represent:**
A) Only positive numbers
B) Signed numbers
C) Floating point numbers
D) Memory addresses

<details><summary>Answer</summary> B) Signed numbers Explanation: Two's complement allows representation of positive and negative integers in binary. </details>

---

**Q7: Which RISC-V instruction is used for branching if two registers are equal?**
A) jal
B) beq
C) add
D) lw

<details><summary>Answer</summary> B) beq Explanation: `beq` stands for "branch if equal," and it updates the PC if the comparison is true. </details>

---

**Q8: The program counter (PC) holds:**
A) The data value to be loaded
B) The address of the next instruction
C) The ALU operation code
D) The stack pointer

<details><summary>Answer</summary> B) The address of the next instruction Explanation: The PC always points to the next instruction to fetch from memory. </details>

---

**Q9: Harvard architecture differs from Von Neumann because:**
A) Uses a stack
B) Combines ALU and register file
C) Has separate memories for instructions and data
D) Has no control unit

<details><summary>Answer</summary> C) Has separate memories for instructions and data Explanation: Harvard architecture separates instruction and data memory to allow simultaneous access, improving speed. </details>

---

**Q10: An operand can be:**
A) A register
B) A memory location
C) A constant value
D) All of the above

<details><summary>Answer</summary> D) All of the above Explanation: Operands can come from registers, memory, or immediate constants depending on the instruction. </details>

---

**Q11: In RISC-V, which instruction format uses imm[11:0], rs1, funct3, rd, and opcode fields?**
A) R-type
B) I-type
C) S-type
D) B-type

<details><summary>Answer</summary> B) I-type Explanation: I-type instructions include immediate values and one source register for operations like `addi` and `lw`. </details>

---

**Q12: If the ALUOp signal is 01, which type of operation does the ALU perform?**
A) Always ADD
B) Always SUBTRACT
C) Use funct3/funct7 for operation
D) AND

<details><summary>Answer</summary> B) Always SUBTRACT Explanation: ALUOp `01` is used for branch instructions (like `beq`), which need subtraction for comparison. </details>

---

**Q13: Which register(s) are used to pass the first four function arguments in RISC-V?**
A) x10–x13 (a0–a3)
B) x0–x3
C) x5–x8 (t0–t3)
D) x2–x5

<details><summary>Answer</summary> A) x10–x13 (a0–a3) Explanation: RISC-V calling convention uses a0–a7 for passing up to eight arguments. </details>

---

**Q14: What happens in a RISC-V processor if a load instruction is executed on the single-cycle processor?**
A) ALU adds two registers
B) Data memory is accessed
C) PC is decremented
D) Nothing

<details><summary>Answer</summary> B) Data memory is accessed Explanation: Load instructions calculate the effective address via the ALU, then read from memory to write to the register file. </details>

---

**Q15: In Von Neumann architecture, the main drawback is:**
A) Complexity of instructions
B) Single memory for code and data can slow access
C) Requires more registers
D) Branch instructions are impossible

<details><summary>Answer</summary> B) Single memory for code and data can slow access Explanation: The shared bus for instructions and data creates a bottleneck, limiting throughput. </details>

---

**Q16: Which of the following is true about immediate values in S-type instructions?**
A) They are always 32-bit wide
B) They are split across two fields in the instruction
C) They are not used for store instructions
D) They only store unsigned numbers

<details><summary>Answer</summary> B) They are split across two fields in the instruction Explanation: S-type instructions split the immediate across `imm[11:5]` and `imm[4:0]` to encode offsets for store instructions. </details>

---

**Q17: Which statement best describes "simplicity favors regularity" in processor design?**
A) Complex instructions improve performance
B) Uniform instruction formats make control logic simpler
C) More registers reduce clock speed
D) ALU complexity is unrelated to instruction format

<details><summary>Answer</summary> B) Uniform instruction formats make control logic simpler Explanation: Regular instruction formats simplify hardware design and control signal generation. </details>

---

**Q18: What is the primary reason for having two read ports in a RISC-V register file?**
A) To fetch instructions faster
B) To allow reading two operands simultaneously for ALU operations
C) To write back results faster
D) To support pipelining

<details><summary>Answer</summary> B) To allow reading two operands simultaneously for ALU operations Explanation: Arithmetic and logical instructions typically require two source operands at the same time. </details>

---

**Q19: When a function modifies a callee-saved register but does not restore it:**
A) Nothing happens
B) Caller sees the modified value, potentially causing bugs
C) Processor automatically restores the register
D) Stack overflows

<details><summary>Answer</summary> B) Caller sees the modified value, potentially causing bugs Explanation: Callee-saved registers must be preserved across calls; otherwise, the calling function may get incorrect values. </details>

---

**Q20: Why does the single-cycle processor design violate "make the common case fast"?**
A) All instructions take the same long clock cycle
B) Only branch instructions are slow
C) Memory is faster than ALU
D) Registers are too small

<details><summary>Answer</summary> A) All instructions take the same long clock cycle Explanation: The clock cycle must accommodate the slowest instruction (like load), slowing down simple operations like ADD. </details>