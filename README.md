# congresschallenge
The CPU acts upon two registers, A and B. This allows for arithmetic operations to be 0-operand. The below table shows the opcode, equivalent mnemonic, and the operands or result of an operation (shown with C bitwise operation notation).
| opcode | mnemonic | operands |
|---|---|---|
| 0000 | **NOP** | **None** |
| 0001 | **AND** | **A &= B** |
| 0010 | **OR** | **A \|= B** |
| 0011 | **XOR** | **A ^= B** |
| 0100 | **NOT** | **~A** |
| 0101 | **ADD** | **A += B** |
| 0110 | **SUB** | **A -= B** |
| 0111 | **LD** | **Select, 10-bit address** |
| 1000 | **STR** | **Select, 10-bit address** |
| 1001 | **HALT** | **None** |
| 1010 | **JMP** | **12-bit address** |
| 1011 | **JZ** | **12-bit address** |
| 1100 | **JC** | **12-bit address** |
| 1101 | **JE** | **12-bit address** |

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

JMP: unconditionally jump to specified address

JZ: if(A == 0) jump to specified address

JC: if((A+B) > 255) jump to specified address

JE: if(A == B) jump to specified address

## quirks
Instructions should be entered at memory position 1 and not 0. If a halt instruction is not entered, the program will run until the end of instruction memory and will loop around, causing errors. There are no instructions to enter immediate values, all values needed for a computation must be entered into data memory.
