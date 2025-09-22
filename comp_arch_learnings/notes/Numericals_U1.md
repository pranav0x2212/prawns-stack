# RISC-V Numericals - Unit 1

This file contains numerical problems covering fundamental RISC-V concepts for exam preparation. Each problem includes step-by-step solutions and explanations.

## Quick Reference Guide for C to RISC-V Translation

### **1. Translating Expressions**

- Identify if operands are **registers or immediates**. Use `addi` if one is constant.
    
- Break down **complex expressions** into multiple instructions, e.g.,
    

`c = a + b - d;` -> `add t0, a, b sub c, t0, d`

---

### **2. Conditional Statements**

- C comparisons (`>`, `<`, `>=`, `<=`, `==`, `!=`) often **invert the logic** in ASM:  
    | C | RISC-V instruction | Notes |  
    |---|------------------|-------|  
    | `a < b` | `blt a, b, label` | Branch if a < b |  
    | `a > b` | `blt b, a, label` | Swap operands |  
    | `a <= b` | `bge b, a, label` | Equivalent logic |  
    | `a >= b` | `bge a, b, label` | Direct |  
    | `a == b` | `beq a, b, label` | Branch if equal |  
    | `a != b` | `bne a, b, label` | Branch if not equal |
    
- Always **think about which path jumps**; label goes to **true case or loop body**.
    

---

### **3. Loops**

- **`for` / `while` loops** → labels + conditional branch at end or start.
    
- Example:
    

`for(i = 0; i < n; i++)`

```assembly
li t0, 0        # i = 0 
loop_start:     
    bge t0, a1, loop_end  # i >= n ?     
    ...                   # loop body     
    addi t0, t0, 1     
    jal x0, loop_start 
loop_end:
```

---

### **4. Arrays**

- **Indexing:** multiply index by 4 (`slli idx, idx, 2`) for 32-bit integers.
    
- Example: `arr[i]`
    

```assembly
slli t1, i, 2       # i*4 
add t2, base_addr, t1 
lw t0, 0(t2)        # t0 = arr[i]
```

- For `arr[i] = value`, just `sw t0, 0(t2)`.
    

---

### **5. Functions / Procedures**

- Always **save caller-saved registers** if you need them after a call.
    
- Use **`jal`** to call, **`jalr x0, ra, 0`** to return.
    
- Stack: save **arguments**, **return address**, and **callee-saved registers**.

---

## Q1: Simple Arithmetic Operations

**Given:**
- Registers: x1 = 5, x2 = 3  

**Assembly Code:**
```assembly
add x3, x1, x2
sub x4, x1, x2
```

**Question:** What are the values in `x3` and `x4` after execution?

<details>
<summary>Answer</summary>

**Solution:**
- `x3 = 8` (5 + 3)
- `x4 = 2` (5 - 3)

**Explanation:** 
- `add` instruction adds the values in two source registers
- `sub` instruction subtracts the second register from the first

</details>

---

## Q2: Logical and Shift Operations

**Given:**
- Registers: x1 = 0b1010 (10 in decimal), x2 = 0b1100 (12 in decimal)

**Assembly Code:**
```assembly
and x3, x1, x2
or  x4, x1, x2
sll x5, x1, 2
```

**Question:** What are the values in `x3`, `x4`, `x5` after execution?

<details>
<summary>Answer</summary>

**Solution:**
- `x3 = 0b1000` (8 in decimal) - bitwise AND
- `x4 = 0b1110` (14 in decimal) - bitwise OR  
- `x5 = 0b101000` (40 in decimal) - shift left by 2

**Step-by-step:**
```
AND: 1010 & 1100 = 1000
OR:  1010 | 1100 = 1110
SLL: 1010 << 2   = 101000
```

**Explanation:** 
- AND/OR operate bitwise on corresponding bits
- SLL shifts left, filling with zeros (equivalent to multiply by 4)

</details>

---

## Q3: Load/Store Operations

**Given:**
- Memory: Address 0x100 contains 10, Address 0x104 contains 20
- Registers: x1 = 0x100, x2 = 0x104

**Assembly Code:**
```assembly
lw x3, 0(x1)
lw x4, 0(x2)
add x5, x3, x4
```

**Question:** What is the value in `x5` after execution?

<details>
<summary>Answer</summary>

**Solution:**
- `x3 = 10` (loaded from memory address 0x100)
- `x4 = 20` (loaded from memory address 0x104)
- `x5 = 30` (10 + 20)

**Explanation:** 
- `lw` loads a word (32 bits) from memory into a register
- The effective address is base register + offset
- Final `add` instruction sums the loaded values

</details>

---

## Q4: C to RISC-V Translation (Simple Arithmetic)

**Given C Code:**
```c
int a = 5, b = 3;
int c = a + b;
```

**Question:** Write equivalent RISC-V assembly code.

<details>
<summary>Answer</summary>

**Solution:**
```assembly
li x5, 5        # a = 5
li x6, 3        # b = 3
add x7, x5, x6  # c = a + b
```

**Explanation:** 
- `li` (load immediate) loads constants into registers
- `add` performs the arithmetic operation
- Variables are mapped to registers: a→x5, b→x6, c→x7

</details>

---

## Q5: C to RISC-V Translation (Conditional)

**Given C Code:**
```c
int a = 10, b = 20;
int c;
if(a < b)
    c = b - a;
else
    c = a - b;
```

**Question:** Write RISC-V assembly using branch instructions.

<details>
<summary>Answer</summary>

**Solution:**
```assembly
li x5, 10       # a = 10
li x6, 20       # b = 20
blt x5, x6, less    # if a < b, go to less
sub x7, x5, x6      # else: c = a - b
jal x0, end         # jump to end
less:
    sub x7, x6, x5  # c = b - a
end:
```

**Explanation:** 
- `blt` branches if first register < second register
- `jal x0, label` acts as unconditional jump
- Different paths execute different subtraction operations

</details>

---

## Q6: C to RISC-V Translation (Loop)

**Given C Code:**
```c
int sum = 0;
for(int i = 1; i <= 5; i++)
    sum += i;
```

**Question:** Write equivalent RISC-V assembly code.

<details>
<summary>Answer</summary>

**Solution:**
```assembly
li x5, 0        # sum = 0
li x6, 1        # i = 1
li x7, 5        # limit = 5
loop:
    add x5, x5, x6      # sum += i
    addi x6, x6, 1      # i++
    ble x6, x7, loop    # if i <= 5, continue loop
```

**Explanation:** 
- Loop structure: initialize → body → increment → condition check
- `addi` adds immediate value (increment by 1)
- `ble` branches if first register ≤ second register
- Final sum = 1+2+3+4+5 = 15

</details>

---

## Q7: C to RISC-V Translation (Array Access)

**Given C Code:**
```c
int arr[3] = {2, 4, 6};
int x = arr[1];
```

**Question:** Write RISC-V assembly to load `arr[1]` into a register.

<details>
<summary>Answer</summary>

**Solution:**
```assembly
la x5, arr      # load base address of array
lw x6, 4(x5)    # x6 = arr[1]
```

**Explanation:** 
- `la` (load address) loads the base address of the array
- Each integer is 4 bytes, so `arr[1]` is at offset 4
- `lw 4(x5)` loads from address (x5 + 4)
- Array indexing: arr[i] = base_address + (i × element_size)

</details>

### **Q8: Decode an I-Type Instruction**

**Given:**

- Instruction (32-bit hex): `0x00F50793`
    
- RISC-V base ISA: RV32I
    
- Instruction format: I-type (`opcode[6:0]`, `rd[11:7]`, `funct3[14:12]`, `rs1[19:15]`, `imm[31:20]`)
    

**Task:**

1. Identify each field: `opcode`, `rd`, `funct3`, `rs1`, `imm`
    
2. Determine the corresponding **assembly instruction**
    

**Answer:**

<details><summary>Answer</summary>

- Hex: `0x00F50793` → Binary: `0000 0000 1111 0101 0000 0111 1001 0011`
    

|Field|Bits|Value (bin)|Value (dec/hex)|
|---|---|---|---|
|opcode|[6:0]|0010011|0x13|
|rd|[11:7]|111|x15|
|funct3|[14:12]|000|ADDI|
|rs1|[19:15]|01010|x10|
|imm|[31:20]|000000001111|15|

- **Assembly instruction:** `addi x15, x10, 15`
    

**Explanation:** I-type instruction performs immediate arithmetic using `rs1` and `imm`, result written to `rd`.

</details>

---

### **Q9: Decode a J-Type Instruction**

**Given:**

- Instruction (32-bit hex): `0x0040006F`
    
- RISC-V base ISA: RV32I
    
- Instruction format: J-type (`opcode[6:0]`, `rd[11:7]`, `imm[31:12]`)
    

**Task:**

1. Identify each field: `opcode`, `rd`, `imm`
    
2. Determine the corresponding **assembly instruction**
    

**Answer:**

<details><summary>Answer</summary>

- Hex: `0x0040006F` → Binary: `0000 0000 0100 0000 0000 0000 0110 1111`
    

|Field|Bits|Value (bin)|Value (dec/hex)|
|---|---|---|---|
|opcode|[6:0]|1101111|0x6F|
|rd|[11:7]|00000|x0|
|imm|[31:12]|000000000100|4|

- **Assembly instruction:** `jal x0, 4`
    

**Explanation:** J-type instruction performs an unconditional jump to PC + `imm`, optionally writing the return address to `rd`.

</details>