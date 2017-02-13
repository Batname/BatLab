## Assembly code to executable (Mac OS-X example)

```
.section __DATA,__data
str:
  .asciz "Hello assembly\n"

.section __TEXT,__text
.global _main
_main:
  movl $0x2000004, %eax
  movl $0x1, %edi
  movl str@GOTPCREL(%rip), %esi
  movl $0xf, %edx
  syscall

  movl $0x0, %edi
  movl $0x2000001, %eax
  syscall
```

Run assembly compilation `as -o object.o assembly.s`

Run link process `ld object.o -e _main -o program`

Run program `./program`