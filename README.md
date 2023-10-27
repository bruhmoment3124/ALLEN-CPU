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
| 0111 | **LD** | **Select, 10b address** |
| 1000 | **STR** | **Select, 10b address** |
| 1001 | **HALT** | **None** |
| 1010 | **JMP** | **12b address** |
| 1011 | **JZ** | **12b address** |
| 1100 | **JC** | **12b address** |
| 1101 | **JE** | **12b address** |

The instructions and data are entered into memory as hexadecimal and here is the instruction form:

**MSB XXXXXXXXXXX X XXXX LSB**

The first (starting from LSB) four bits are opcode bits. The singular bit is used for instructions that need to select register A or B. The last 11 bits are address bits. In the case of jump instructions, the selector bit is used as a part of the address to achieve 12-bit (4k) addressing. In the case of data access, only 10 bits are used, so the last address bit (MSB) is not used; the selector is needed for data memory operations so it is not used for addressing.
