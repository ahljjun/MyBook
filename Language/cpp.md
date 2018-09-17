# CPP



目标文件\(Object File/Executable File\)

```
int printf(const char* format, ...);

int global_var1 = 100;
int global_var2;

void func1(int i) 
{
    printf("%d\n", i);
}

int main() {
    static int static_var1 = 200;
    static int static_var2;

    int a = 1;
    int b;
    func1(global_var1);
    func1(static_var1 + static_var2);
    func1(a + b);
    return a;
}
```

**objdump -h SimpleSection.o **

```
SimpleSection.o:     file format elf64-x86-64
```

```
Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         00000066  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .data         00000008  0000000000000000  0000000000000000  000000a8  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000004  0000000000000000  0000000000000000  000000b0  2**2
                  ALLOC
  3 .rodata       00000004  0000000000000000  0000000000000000  000000b0  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  4 .comment      00000043  0000000000000000  0000000000000000  000000b4  2**0
                  CONTENTS, READONLY
  5 .note.GNU-stack 00000000  0000000000000000  0000000000000000  000000f7  2**0
                  CONTENTS, READONLY
  6 .eh_frame     00000058  0000000000000000  0000000000000000  000000f8  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
 
```

**Size SimpleSection.o **

```
   text    data     bss     dec     hex filename
    194       8       4     206      ce SimpleSection.o
    
```

**objdump -s -d impleSection.o  //  -s: 将所有段内容按字符串打印出来   -d\(disassemble\): 将多有包含指令的段反汇编**

```

SimpleSection.o:     file format elf64-x86-64

Contents of section .text:
 0000 554889e5 4883ec10 897dfc8b 45fc89c6  UH..H....}..E...
 0010 bf000000 00b80000 0000e800 000000c9  ................
 0020 c3554889 e54883ec 10c745fc 01000000  .UH..H....E.....
 0030 8b050000 000089c7 e8000000 008b1500  ................
 0040 0000008b 05000000 0001d089 c7e80000  ................
 0050 00008b45 f88b55fc 01d089c7 e8000000  ...E..U.........
 0060 008b45fc c9c3                        ..E...          
Contents of section .data:
 0000 64000000 c8000000                    d.......        <<<<<--- 100 = 0x64; 200 = 0xc8
Contents of section .rodata:
 0000 25640a00                             %d..            <<<<<--- printf 使用的%d是个常数变量
Contents of section .comment:
 0000 00474343 3a202853 55534520 4c696e75  .GCC: (SUSE Linu
 0010 78292034 2e382e33 20323031 34303632  x) 4.8.3 2014062
 0020 37205b67 63632d34 5f382d62 72616e63  7 [gcc-4_8-branc
 0030 68207265 76697369 6f6e2032 31323036  h revision 21206
 0040 345d00                               4].             
Contents of section .eh_frame:
 0000 14000000 00000000 017a5200 01781001  .........zR..x..
 0010 1b0c0708 90010000 1c000000 1c000000  ................
 0020 00000000 21000000 00410e10 8602430d  ....!....A....C.
 0030 065c0c07 08000000 1c000000 3c000000  .\..........<...
 0040 00000000 45000000 00410e10 8602430d  ....E....A....C.
 0050 0602400c 07080000                    ..@.....        

Disassembly of section .text:

0000000000000000 <func1>:
   0:   55                      push   %rbp
   1:   48 89 e5                mov    %rsp,%rbp
   4:   48 83 ec 10             sub    $0x10,%rsp
   8:   89 7d fc                mov    %edi,-0x4(%rbp)
   b:   8b 45 fc                mov    -0x4(%rbp),%eax
   e:   89 c6                   mov    %eax,%esi
  10:   bf 00 00 00 00          mov    $0x0,%edi
  15:   b8 00 00 00 00          mov    $0x0,%eax
  1a:   e8 00 00 00 00          callq  1f <func1+0x1f>
  1f:   c9                      leaveq 
  20:   c3                      retq   

0000000000000021 <main>:
  21:   55                      push   %rbp
  22:   48 89 e5                mov    %rsp,%rbp
  25:   48 83 ec 10             sub    $0x10,%rsp
  29:   c7 45 fc 01 00 00 00    movl   $0x1,-0x4(%rbp)
  30:   8b 05 00 00 00 00       mov    0x0(%rip),%eax        # 36 <main+0x15>
  36:   89 c7                   mov    %eax,%edi
  38:   e8 00 00 00 00          callq  3d <main+0x1c>
  3d:   8b 15 00 00 00 00       mov    0x0(%rip),%edx        # 43 <main+0x22>
  43:   8b 05 00 00 00 00       mov    0x0(%rip),%eax        # 49 <main+0x28>
  49:   01 d0                   add    %edx,%eax
  4b:   89 c7                   mov    %eax,%edi
  4d:   e8 00 00 00 00          callq  52 <main+0x31>
  52:   8b 45 f8                mov    -0x8(%rbp),%eax
  55:   8b 55 fc                mov    -0x4(%rbp),%edx
  58:   01 d0                   add    %edx,%eax
  5a:   89 c7                   mov    %eax,%edi
  5c:   e8 00 00 00 00          callq  61 <main+0x40>
  61:   8b 45 fc                mov    -0x4(%rbp),%eax
  64:   c9                      leaveq 
  65:   c3                      retq   
   
  
```

.data : 保存一些已经初始化的全局静态变量和局部静态变量  
.bass: 存放未初始化的全局变量和局部静态变量  
.rodata: 存放只读数据，如字符串常量，全局const变量  
.init: 程序初始与终结代码段， C++全局构造与析构  
















