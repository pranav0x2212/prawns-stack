# Computer Architecture Notes - Study Guide

This directory contains comprehensive notes for Computer Organization and Design exam preparation. All syllabus topics are covered with detailed explanations, examples, and diagrams.

## File Organization & Syllabus Mapping

### **arch_and_blocks.md** 
**Covers Syllabus Sections:**
- **2.1** - Basic blocks of Processor, RISC vs CISC, Von Neumann vs Harvard
- **4.2** - Logic Design Conventions

**Topics Included:**
- RISC vs CISC comparison table
- Von Neumann vs Harvard architectures (with analogies & diagrams)
- Processor building blocks (datapath, control unit, memory, I/O)
- Combinational vs sequential logic
- Clocking methodology and timing
- Design principles applied

### **ops_and_operands.md**
**Covers Syllabus Sections:**
- **2.2** - Operations of Computer hardware
- **2.3** - Operands of Computer hardware (Registers, Memory, Constants)
- **2.4** - Signed and Unsigned numbers
- **2.6** - Logical operations
- **2.10** - RISC-V Addressing for wide immediate and addresses

**Topics Included:**
- All operand types (register, immediate, memory)
- Arithmetic and logical operations
- Signed vs unsigned number representation
- Logical operations (AND, OR, XOR, shifts)
- Wide immediate addressing (LUI, AUIPC)
- **Complete Load/Store instructions** (lw, sw, lb, lh, sb, sh, ld, sd)
- Memory alignment and addressing modes
- Sign extension vs zero extension

### **instructions.md**
**Covers Syllabus Sections:**
- **2.5** - Representing Instructions in Computer (R, I, S, B, J, U types)
- **2.7** - Instructions for making decisions
- **2.20** - Rest of the RISC-V Instruction Set

**Topics Included:**
- Stored-program concept
- All RISC-V instruction formats with encoding fields
- Branch instructions (beq, bne, blt, bge)
- Control flow and decision making
- **Pseudo-instructions** (mv, li, nop, la, not, neg)
- **System calls** (ecall, ebreak, CSR instructions)
- Binary representation examples

### **everything_related_to_c.md**
**Covers Syllabus Sections:**
- **2.8** - Supporting procedures in Computer hardware
- **2.13** - A C Sort Example to put it All together
- **2.14** - Arrays versus Pointers

**Topics Included:**
- Function calls and stack management
- Register saving and restoring (caller vs callee saved)
- Procedure linkage (jal, jalr)
- Stack frame layout and calling conventions
- Recursive function examples with full RISC-V assembly
- **C Sort Example** (insertion sort with complete RISC-V implementation)
- **Arrays vs Pointers** (memory layout, pointer arithmetic, performance)

### **processor.md**
**Covers Syllabus Sections:**
- **4.3** - Building a Datapath
- **4.4** - A Simple Implementation Scheme

**Topics Included:**
- RISC-V instruction execution overview
- Complete datapath design and control flow
- Multiplexers and control logic
- ALU control mechanisms (ALUOp, control signals)
- Control signal generation from instruction format
- Single-cycle processor implementation
- Limitations of single-cycle design

### **pipelining.md**
**Covers Syllabus Sections:**
- **4.6** - An overview of pipelining

**Topics Included:**
- 5-stage pipeline (IF, ID, EX, MEM, WB)
- Pipeline advantages and instruction flow
- Pipeline hazards (structural, data, control)
- Forwarding vs stalling examples
- Code reordering to avoid stalls

### **hazard_control.md**
**Covers Syllabus Sections:**
- **4.6** - An overview of pipelining (hazard management)

**Topics Included:**
- Data hazards: forwarding vs stalling (detailed conditions)
- Load-use hazards and pipeline bubbles
- Control hazards: branch delay and flushing
- Branch prediction and early resolution
- Comprehensive forwarding logic

## Quick Reference

### **Need to understand basic concepts?**
-> Start with `arch_and_blocks.md`

### **Working with instructions and operations?**
-> `ops_and_operands.md` + `instructions.md`

### **Learning about procedures and C programming?**
-> `everything_related_to_c.md`

### **Understanding processor implementation?**
-> `processor.md`

### **Studying pipelining?**
-> `pipelining.md` + `hazard_control.md`

## Study Strategy

1. **Foundation:** Start with `arch_and_blocks.md` for core concepts
2. **Instructions:** Move to `instructions.md` and `ops_and_operands.md`
3. **Programming:** Study `everything_related_to_c.md` for procedures and examples
4. **Implementation:** Deep dive into `processor.md`
5. **Advanced:** Master `pipelining.md` and `hazard_control.md`

**All files include:**
- Clear explanations with analogies
- Comprehensive examples
- Assembly code demonstrations
- Practical applications
- Exam-focused content

## Self-Testing and Practice

### **MCQs (Multiple Choice Questions)**
- **`MCQs.md`** - Unit 1 questions covering basic concepts, operations, and processor fundamentals
- **`MCQs_U2.md`** - Unit 2 questions focused on pipelining, hazards, and performance

### **Numericals**
- **`Numericals_U1.md`** - Unit 1 practice problems with step-by-step solutions
- **`Numericals_U2.md`** - Unit 2 pipeline calculations and hazard analysis

Both MCQ and Numerical files include:
- Easy, medium, and hard difficulty levels
- Detailed explanations and solutions
- Collapsible answer sections for self-testing
- Exam-pattern questions

Good luck with your exam lol.