# Supporting Procedures in Computer Hardware

## 1. Function Calls and the Stack

- **Procedure (function)**: A reusable block of code that can be called from multiple places.
    
- **Stack**: LIFO memory structure used to manage:
    
    - Return addresses
        
    - Local variables
        
    - Saved registers
        

### Stack Operations

- **Push**: Store a value on top of the stack (`sp -= size; store value`).
    
- **Pop**: Remove the top value from the stack (`load value; sp += size`).
    

**Analogy:** Think of the stack as a stack of trays — you can only take the top tray off first.

## 2. Register Saving and Restoring

When a procedure is called:

1. **Caller-saved registers**: Saved by the calling function if needed after call (temporary registers like `t0–t6`).
    
2. **Callee-saved registers**: Saved by the called function if it will modify them (saved registers `s0–s11`).
    

**Example:**

```assembly
addi sp, sp, -8     # make space
sw s0, 0(sp)        # save callee-saved register
...
lw s0, 0(sp)        # restore before return
addi sp, sp, 8
ret
```

## 3. Procedure Linkage

- **jal** (jump and link): Calls a function and saves return address in `ra` (`x1`).
    
- **jalr** (jump and link register): Returns to caller using stored `ra`.
    

**Example:**
```assembly
# Caller
jal my_func        # jump to function

# Function
my_func:
    addi sp, sp, -8
    sw ra, 4(sp)
    sw s0, 0(sp)
    ...
    lw s0, 0(sp)
    lw ra, 4(sp)
    addi sp, sp, 8
    jalr x0, x1, 0   # return
```

## 4. Stack Frame Layout

```text
Higher Addresses
+-----------------+
| Previous RA      |
| Saved s0..sN     |
| Local variables  |
+-----------------+
Lower Addresses
sp -> points here at function start
```

- `sp` always points to the **bottom** of the current function's stack frame.
    
- Allows nested or recursive function calls safely.

## 5. Calling Conventions

- **Argument passing**: Registers `a0–a7` hold the first 8 arguments.
    
- **Return values**: Registers `a0–a1` are used for return values.
    
- **More than 8 arguments**: Remaining arguments are passed on the stack.
    

**Example:**
```c
int foo(int a,b,c,d,e,f,g,h,i);
```
`a → a0, b → a1, … h → a7, i → stack.`

## 6. Stack Pointer Management

- **Alignment**: RISC-V requires `sp` to be 16-byte aligned at function calls.
    
- **Frame pointer** (`fp` / `s0`): Optional register to reliably reference locals if the stack moves.
    
- **Stack overflow**: Must be checked by OS/runtime; hardware doesn't prevent it.
    

## 7. Nested / Recursive Calls

- Each function call pushes its own stack frame.
    
- Recursive calls grow the stack with each call.
    

**Stack Growth Example:**
```text
Call factorial(3)
+-----------------+
| return addr 1   | <- ra for factorial(3)
| n=3             |
+-----------------+
Call factorial(2)
+-----------------+
| return addr 2   | <- ra for factorial(2)
| n=2             |
+-----------------+
Call factorial(1)
+-----------------+
| return addr 3   | <- ra for factorial(1)
| n=1             |
+-----------------+
```

## 8. Practical Examples

### 8.1 Leaf Procedure

Doesn't call any other function → may skip saving `ra`.

```assembly
leaf_func:
    addi a0, a0, 1  # simple operation
    ret
```

### 8.2 Non-leaf Procedure

Calls other functions → must save `ra` on the stack.

```assembly
non_leaf:
    addi sp, sp, -16
    sw ra, 12(sp)   # save return address
    sw s0, 8(sp)    # save frame pointer
    mv s0, sp       # new frame pointer
    jal other_func  # call another function
    lw ra, 12(sp)
    lw s0, 8(sp)
    addi sp, sp, 16
    ret
```

## 9. Example: Recursive Function (Factorial)

**C code:**
```c
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);
}
```

**RISC-V assembly:**
```assembly
# factorial(n) in RV32I
# Input: a0 = n
# Output: a0 = factorial(n)

factorial:
    addi sp, sp, -16       # reserve stack space
    sw ra, 12(sp)          # save return address
    sw a0, 8(sp)           # save n
    ble a0, x1, base_case  # if n <= 1, go to base_case

    addi a0, a0, -1        # n-1
    jal factorial          # recursive call

    lw t0, 8(sp)           # original n
    mv t1, a0              # factorial(n-1)
    li a0, 0               # accumulator for multiplication

mul_loop:
    beq t1, x0, end_mul    # if t1 == 0, done
    add a0, a0, t0         # accumulate: a0 += t0
    addi t1, t1, -1        # decrement t1
    jal x0, mul_loop       # repeat

end_mul:
    lw ra, 12(sp)
    addi sp, sp, 16
    ret

base_case:
    li a0, 1
    lw ra, 12(sp)
    addi sp, sp, 16
    ret
```

---

# Arrays vs Pointers 

## Array Memory Layout

- Arrays occupy **contiguous memory**.
    
    `int arr[4] = {1,2,3,4};`
    
    Memory (assuming 4-byte ints):
    
    `addr+0: 1 addr+4: 2 addr+8: 3 addr+12:4`
    
- Pointers store **addresses**, not values:
    
    `int *p = arr;`
    

## Pointer Arithmetic in RISC-V

- Incrementing pointer by 1 → moves by element size (not 1 byte):
    
    `addi t0, t0, 4  # move to next int`
    
- Accessing array via pointer:
    
    `lw t1, 0(t0)    # load *p`
    

## Array Indexing vs Pointer Dereferencing

|Operation|Assembly Example|
|---|---|
|`arr[i]`|`slli t1, i, 2` → `add t2, arr_base, t1` → `lw t3, 0(t2)`|
|`*(p+i)`|Same as above using pointer `p`|

- **Key point:** Arrays **decay to pointers** when passed to functions.
    
- Example: `void sum(int *arr, int n)` → accesses `arr[i]` like a pointer.
    

## Performance Implications

- Access via pointer or array indexing is similar in assembly.
    
- Contiguous memory ensures cache-friendly access.
    
- Pointer arithmetic avoids extra multiplication (`i*size`) if compiler knows type size.

---

# C Sort Example: Putting It All Together

## Insertion Sort in C

```C
void insertion_sort(int arr[], int n){
    for(int i = 1; i < n; i++){
        int key = arr[i];
        int j = i - 1;
        while(j >= 0 && arr[j] > key){
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}
```

---

## RISC-V Assembly (RV32I) Key Points

- `a0` → array base, `a1` → n
    
- Outer loop: `i = 1` to `n-1`
    
- Inner loop: `j >= 0 && arr[j] > key`
    
- Temporary registers `t0-t3` used for key, j, and addresses
    

```assembly
# a0 = arr base, a1 = n
li t0, 1          # i = 1
outer_loop:
    bge t0, a1, done_outer   # if i >= n, done
    slli t1, t0, 2           # i*4
    add t2, a0, t1           # arr[i] address
    lw t3, 0(t2)             # key = arr[i]
    addi t4, t0, -1          # j = i-1

inner_loop:
    blt t4, x0, insert_key   # if j < 0, insert key
    slli t5, t4, 2
    add t6, a0, t5
    lw t7, 0(t6)             # arr[j]
    ble t7, t3, insert_key   # if arr[j] <= key, insert key
    addi t8, t4, 1
    slli t8, t8, 2
    add t9, a0, t8
    sw t7, 0(t9)             # arr[j+1] = arr[j]
    addi t4, t4, -1
    jal x0, inner_loop

insert_key:
    addi t4, t4, 1
    slli t4, t4, 2
    add t5, a0, t4
    sw t3, 0(t5)             # arr[j+1] = key
    addi t0, t0, 1
    jal x0, outer_loop

done_outer:
```