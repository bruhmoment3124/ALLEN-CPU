# ALLEN-CPU
![image](https://github.com/bruhmoment3124/ALLEN-CPU/assets/73769756/83d4578c-7d26-479a-856f-035bf964a7f5)
The CPU acts upon two registers, A and B. This allows for arithmetic operations to be 0-operand. The below table shows the opcode, equivalent mnemonic, and the operands or result of an operation (shown with C bitwise operation notation).
| opcode | mnemonic | operands and result |
|---|---|---|
| 0000 | **NOP** | **None** |
| 0001 | **AND** | **A &= B** |
| 0010 | **OR** | **A \|= B** |
| 0011 | **XOR** | **A ^= B** |
| 0100 | **NOT** | **~A** |
| 0101 | **ADD** | **A += B** |
| 0110 | **SUB** | **A -= B** |
| 0111 | **LD** | **Select, 10-bit address; load value at 10-bit address into A or B** |
| 1000 | **STR** | **Select, 10-bit address; store A or B into data memory at 10-bit address** |
| 1001 | **HALT** | **None; halt program** |
| 1010 | **JMP** | **12-bit address; jump unconditionally to 12-bit address** |
| 1011 | **JZ** | **12-bit address; jump to 12-bit address if A is zero** |
| 1100 | **JC** | **12-bit address; jump to 12-bit address if A and B exceed 8 bits when added together (see quirks)** |
| 1101 | **JE** | **12-bit address; jump to 12-bit address if A and B are equal** |

The instructions and data are entered into memory as hexadecimal and here is the instruction form:

(An X means that that bit is not used for a given instruction.)

**For ALU operations:**

**MSB XXXXXXXXXXXX 0000 LSB**

ALU operations are 0-operand as described above.

**For data memory operations:**

**MSB X0000000000 0 0000 LSB**

The singular bit selects the register used in the operation chosen by the 4-bit opcode. The next 10 bits are used as data memory address bits. 

**For jumps:**

**MSB 000000000000 0000 LSB**

The 4-bit opcode specifies the instruction and the 12 bits is the address to be jumped to in instruction memory. Here are the jump conditions:

## Quirks
- Instructions should be entered at memory position 1 and not 0.
- If a halt instruction is not entered, the program will run until the end of instruction memory and will loop around, causing errors.
- There are no instructions to enter immediate values, all values needed for a computation must be entered into data memory.
- The JC instruction must be called before actually carrying. This is because it adds together A and B and tests if it carries, so the values must be in A and B.

## Example 1
This program computes two 8-bit Fibonacci numbers:
| plaintext instruction (decimal) | hex instruction |
|---|---|
| **0 0 LD** | **7** |
| **1 1 LD** | **37** |
| **ADD** | **5** |
| **0 0 STR** | **8** |
| **4 0 LD** | **87** |
| **ADD** | **5** |
| **0 1 LD** | **17** |
| **ADD** | **5** |
| **1 0 STR** | **28** |
| **2 0 LD** | **47** |
| **3 1 LD** | **77** |
| **SUB** | **6** |
| **2 0 STR** | **48** |
| **16 JZ** | **10b** |
| **1 JMP** | **1a** |
| **4 1 LD** | **97** |
| **3 0 STR** | **68** | 
| **HALT** | **9** |

Load these values into data memory (starting at 0):

| 0 | 1 | X | 1 |

X is the number of iterations; I recommend 6 to get the two largest 8-bit Fibonacci numbers.
## Example 2
This example adds two 16-bit numbers using the "jump if carry" (JC) instruction.
| plaintext instruction (decimal) | hex instruction |
|---|---|
| **0 0 LD** | **7** |
| **2 1 LD** | **57** |
| **ADD** | **5** |
| **0 0 STR** | **8** |
| **1 0 LD** | **27** |
| **3 1 LD** | **77** |
| **9 JC** | **9c** |
| **16 JMP** | **10a** |
| **ADD** | **5** |
| **1 0 STR** | **28** |
| **0 0 LD** | **7** |
| **4 1 LD** | **97** |
| **ADD** | **5** |
| **0 0 STR** | **8** |
| **18 JMP** | **12a** |
| **ADD** | **5** |
| **1 0 STR** | **28** |
| **5 0 LD** | **a7** |
| **5 1 LD** | **b7** |
| **2 0 STR** | **48** |
| **3 0 STR** | **68** |
| **4 0 STR** | **88** |
| **HALT** | **9** |

Load these values into data memory (starting at 0):

| X0 | X1 | Y0 | Y1 | 1 |

X0 and Y0 are the most significant 8 bits and X1 and Y1 are the least significant 8 bits.
