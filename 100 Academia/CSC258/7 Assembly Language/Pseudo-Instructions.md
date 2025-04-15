---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "32"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-26
Date created: Sat., Apr. 12, 2025, 2:43:34 am
Date modified: Mon., Apr. 14, 2025, 8:14:38 am
---

# Pseudo-Instructions

- **Pseudo-instructions**
    - Are there for the *convenience* of the programmer
- Assembler translates them into *one or more* real MIPS assembly instructions
    - Real MIPS instructions have **opcodes**
    - Pseudo-instructions do not
    - Assembler often uses special `$at` register (also written as `$1`) when mapping pseudo-instructions to MIPS instructions

## Example: `la` Pseudo-instruction

- **==`la`==**
    - Load address
    - Pseudo-instruction written in the format: `la $d, label`
        - Loads a register `$d` with the *memory address* that `label` corresponds to
- Usually translated by assembler into the following *two* MIPS instructions:
    - & `lui $at, immediate1`
        - Load upper immediate
        - `immediate1` represents the *upper 16 bits* of the memory address `label` corresponds to
            - Bits are loaded in the upper 16 bits of the destination register
        - ![](https://i.imgur.com/K3gdFBd.png)
    - & `ori $d, $at, immediate2`
        - `immediate2` represents the *lower 16 bits* of the memory address `label` corresponds to

## Another Pseudo-instruction Example: `bge`

- Some branch instructions are *pseudo-instructions*
- `bge $s, $t, label`
    - Branch to label iff `$s >= $t`
        - Comparing register contents
    - Implemented by using one of the comparison instructions followed by `beq` or `bne`
        - `slt $at, $s, $t`
            - Set `$at` to 1 is `$s < $st`
        - `beq $at, $zero, label`
            - Branch if `$at == 0`
- % Recall that `$at` register is reserved for the assembler

![](https://i.imgur.com/I3LMrcy.png)
