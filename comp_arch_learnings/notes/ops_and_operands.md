# Operations, Operands, and Logic 

## Operands

**Definition:** Operands are the values that an instruction operates on. In RISC-V, operands can come from:

1. **Registers** – fast storage inside the CPU (e.g., `x1`, `x2`).
    
    - Example: `add x3, x1, x2` → adds contents of `x1` and `x2`, stores result in `x3`.
        
    - Analogy: Registers are like the small containers on your desk—super fast to access but limited in number.
        
2. **Immediate values** – constants encoded in the instruction itself.
    
    - Example: `addi x3, x1, 10` → adds the value 10 to `x1` and stores in `x3`.
        
    - Analogy: You write a number directly on a sticky note on your desk, ready to use.
        
3. **Memory locations** – values stored in RAM. Accessing them is slower than registers.
    
    - Example: `lw x3, 0(x1)` → loads the value from memory address in `x1` into `x3`.
        
    - Analogy: Memory is like a filing cabinet—you have to open the drawer to fetch something.
        

**Operand Types Summary:**

- **Register operands:** fast, limited number.
    
- **Immediate operands:** encoded constant values.
    
- **Memory operands:** slower, can hold lots of data.
    

---

## Operations

RISC-V instructions operate on operands in **simple, consistent ways**, which is a hallmark of RISC design. Operations include:

1. **Arithmetic:** `add`, `sub`, `mul`, `div`
    
    - Example: `add x3, x1, x2` → `x3 = x1 + x2`
        
    - RISC-V separates addition/subtraction from multiplication/division for simplicity.
        
2. **Logical:** `and`, `or`, `xor`, `not` (via `xori` for immediate)
    
    - Example: `and x3, x1, x2` → bitwise AND of `x1` and `x2`.
        
3. **Shifts:** `sll` (shift left logical), `srl` (shift right logical), `sra` (shift right arithmetic)
    
    - Example: `sll x3, x1, x2` → shift `x1` left by the number of bits in `x2`.
        
4. **Comparison:** `slt` (set less than), `sltu` (set less than unsigned)
    
    - Sets a register to 1 if the condition is true, else 0.
        
    - Example: `slt x3, x1, x2` → `x3 = (x1 < x2 ? 1 : 0)`
        

**Analogy for operations:** Think of your CPU as a kitchen: arithmetic is chopping, logical is straining or filtering, shift is sliding things left or right on a tray, and comparison is taste-testing—does it meet a certain condition?

---

## RISC-V Operand Limitations and Regularity

Unlike some CISC processors (x86), RISC-V instructions typically allow only **2 source operands and 1 destination operand**.

**Example:** You want to compute `(a + b) - (g + h)` in RISC-V:

```asm
add x5, x1, x2     # x5 = a + b
add x6, x3, x4     # x6 = g + h
sub x7, x5, x6     # x7 = (a + b) - (g + h)
```

### Why multiple instructions?

RISC-V favors **simplicity and regularity** over “do everything in one instruction.”

- Each instruction has a fixed format, which simplifies CPU design, pipelining, and decoding.
    
- **Analogy:** Instead of a Swiss army knife that does 10 things at once, RISC-V is like a set of simple, reliable kitchen tools—you can combine them to do anything complex.
    

### CISC Contrast

    

## Signed and Unsigned Numbers

**Signed numbers** in RISC-V are typically represented using two's complement. The most significant bit (MSB) indicates the sign (0 for positive, 1 for negative). Arithmetic instructions like `add`, `sub`, and `slt` operate on signed values by default.

**Unsigned numbers** are non-negative and use all bits for magnitude. Instructions like `sltu` (set less than unsigned) operate on unsigned values. Overflow is handled differently for signed and unsigned operations.

**Example:**
```
addi x5, x0, -1    # x5 = 0xFFFFFFFF (signed -1)
sltu x6, x5, x0    # x6 = 0 (since 0xFFFFFFFF > 0x0 unsigned)
```

## Logical Operations (Expanded)

Logical instructions perform bitwise operations:
- `and x3, x1, x2`   // Bitwise AND
- `or x3, x1, x2`    // Bitwise OR
- `xor x3, x1, x2`   // Bitwise XOR
- `xori x3, x1, imm` // Bitwise XOR with immediate

Logical operations are used for masking, setting, or clearing bits, and for implementing low-level algorithms.

## RISC-V Addressing for Wide Immediate and Addresses

RISC-V uses special instructions for handling wide immediates and addresses:
- `LUI` (Load Upper Immediate): Loads a 20-bit immediate into the upper 20 bits of a register.
- `AUIPC` (Add Upper Immediate to PC): Adds a 20-bit immediate to the program counter and stores the result in a register.

These are used for constructing 32-bit addresses and for position-independent code.

**Example:**
```
lui x1, 0x12345      # x1 = 0x12345000
auipc x2, 0x10       # x2 = PC + 0x10000
```

---

## Load and Store Instructions (Detailed)

### Overview
RISC-V follows a **load/store architecture** where memory is only accessed through explicit load and store instructions. This section covers all variants and their usage patterns.

### Load Instructions

#### **Basic Load Instructions (RV32I)**
| Instruction | Description | Data Size | Sign Extension |
|-------------|-------------|-----------|----------------|
| `lw rd, offset(rs1)` | Load word | 32 bits | N/A |
| `lh rd, offset(rs1)` | Load halfword | 16 bits | Sign-extended to 32 bits |
| `lhu rd, offset(rs1)` | Load halfword unsigned | 16 bits | Zero-extended to 32 bits |
| `lb rd, offset(rs1)` | Load byte | 8 bits | Sign-extended to 32 bits |
| `lbu rd, offset(rs1)` | Load byte unsigned | 8 bits | Zero-extended to 32 bits |

#### **Extended Load Instructions (RV64I)**
| Instruction | Description | Data Size |
|-------------|-------------|-----------|
| `ld rd, offset(rs1)` | Load doubleword | 64 bits |
| `lwu rd, offset(rs1)` | Load word unsigned | 32 bits, zero-extended to 64 bits |

### Store Instructions

#### **Basic Store Instructions (RV32I)**
| Instruction | Description | Data Size |
|-------------|-------------|-----------|
| `sw rs2, offset(rs1)` | Store word | 32 bits |
| `sh rs2, offset(rs1)` | Store halfword | 16 bits (lower 16 bits of rs2) |
| `sb rs2, offset(rs1)` | Store byte | 8 bits (lower 8 bits of rs2) |

#### **Extended Store Instructions (RV64I)**
| Instruction | Description | Data Size |
|-------------|-------------|-----------|
| `sd rs2, offset(rs1)` | Store doubleword | 64 bits |

### Addressing Modes

**Base + Offset Addressing:**
- Address = rs1 + sign_extend(offset)
- Offset range: -2048 to +2047 (12-bit signed immediate)

**Examples:**
```assembly
lw x5, 0(x1)      # Load from address in x1
lw x5, 4(x1)      # Load from address x1 + 4
lw x5, -8(x1)     # Load from address x1 - 8
sw x6, 12(x2)     # Store to address x2 + 12
```

### Memory Alignment

**Alignment Requirements:**
- **Word (32-bit)**: Must be 4-byte aligned (address divisible by 4)
- **Halfword (16-bit)**: Must be 2-byte aligned (address divisible by 2)  
- **Byte (8-bit)**: No alignment requirement
- **Doubleword (64-bit)**: Must be 8-byte aligned (address divisible by 8)

**Misaligned Access:**
- Hardware may trap on misaligned access
- Performance penalty even if supported

### Sign Extension vs Zero Extension

**Sign Extension Example:**
```assembly
# Memory contains: 0xFF at address 100
lb x1, 100(x0)     # x1 = 0xFFFFFFFF (-1 in two's complement)
lbu x2, 100(x0)    # x2 = 0x000000FF (255 unsigned)
```

**Practical Use Cases:**
- **Signed loads** (`lb`, `lh`): For signed integers, characters
- **Unsigned loads** (`lbu`, `lhu`): For unsigned data, bit manipulation

### Load/Store Examples

#### **Array Access**
```assembly
# int arr[100]; access arr[i]
# Assume: x1 = base address of arr, x2 = i
slli x3, x2, 2      # x3 = i * 4 (word size)
add x4, x1, x3      # x4 = &arr[i]
lw x5, 0(x4)        # x5 = arr[i]
```

#### **Structure Field Access**
```c
// struct { int a; short b; char c; } s;
// s.a at offset 0, s.b at offset 4, s.c at offset 6
```
```assembly
# Assume x1 = address of struct s
lw x2, 0(x1)        # Load s.a (int)
lh x3, 4(x1)        # Load s.b (short)
lb x4, 6(x1)        # Load s.c (char)
```

#### **Stack Operations**
```assembly
# Push x5 onto stack
addi sp, sp, -4     # Allocate space
sw x5, 0(sp)        # Store value

# Pop from stack into x6
lw x6, 0(sp)        # Load value
addi sp, sp, 4      # Deallocate space
```

### Performance Considerations

**Cache Effects:**
- Sequential loads/stores are cache-friendly
- Random access patterns cause more cache misses

**Load-Use Hazards:**
```assembly
lw x1, 0(x2)        # Load takes time
add x3, x1, x4      # Must wait for x1 → pipeline stall
```

**Optimizations:**
- Reorder instructions to avoid load-use dependencies
- Use multiple registers to enable parallel loads
