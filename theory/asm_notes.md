# ASSEMBLY

## syntax
* Intel vs AT&T

|                |                   AT&T                        |                    Intel                |
| -----|----|----|
| Parameter order| Source before the destination `move $5, %eax` | Destination before source `move eax, 5` |
| Parameter size | Mnemonics are suffixed `addl $4, %esp`        | Derived from the name of the register (e.g. rax, eax, ax) `add esp, 4` |
| Sigils          | Immediate values prefixed with a "$", registers prefixed with a "%" | The assembler auto detect |
| Effective addresses | General syntax of DISP(BASE,INDEX,SCALE). `movl mem_location(%ebx,%ecx,4), %eax` | Arithmetic expressions in square brackets `mov eax, [ebx + ecx*4 + mem_location]`|


## References
* [Assembly language](https://en.wikipedia.org/wiki/Assembly_language)
* [x86 assembly language](https://en.wikipedia.org/wiki/X86_assembly_language)
* [Microsoft Macro Assembler](https://en.wikipedia.org/wiki/Microsoft_Macro_Assembler)
* [X86 Assembly/GAS Syntax](https://en.wikibooks.org/wiki/X86_Assembly/GAS_Syntax)

