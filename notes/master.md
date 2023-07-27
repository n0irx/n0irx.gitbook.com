# How Each GCC Process C/C++ Code

Sometimes, I wonder how the gcc compilation works, I only know we can do something like `gcc file.c -o file.o` and be done with it. It turns out, this command do a lot of things in the back.

I will do this compilation in MacOS, so the output may differ if you use Linux or Windows machine, but I think the overall would be simiar.

```c
// main.c file 

#include <stdio.h>

int main() {
    printf("Hello world");
    return 0;
}
```

The code above will be our example code. Now, let's see how the compilation works from the start:

## Preprocessing (Preprocessor)



The first process of compilation is preprocessing, where the our source code (source.c) is preprocessed by:

1. including files containing definition (#include keyword)
2. Convert values define by #define&#x20;
3. Convert macros
4. Include/exclude #if, #elif, #endif directive

<img src="../.gitbook/assets/file.excalidraw (2).svg" alt="" class="gitbook-drawing">

`gcc main.c -o main.i # preprocess file using gcc`

```c
// output: main.i
...
...
... include directive
...
...
# 52 "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/secure/_stdio.h" 3 4
extern int __snprintf_chk (char * restrict, size_t, int, size_t,
      const char * restrict, ...);
      
extern int __vsprintf_chk (char * restrict, int, size_t,
      const char * restrict, va_list);

extern int __vsnprintf_chk (char * restrict, size_t, int, size_t,
       const char * restrict, va_list);
# 417 "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/stdio.h" 2 3 4
# 2 "main.c" 2

int main() {
    printf("Hello world");
    return 0;
}
```

## Compiling (Compiler)

<img src="../.gitbook/assets/file.excalidraw.svg" alt="" class="gitbook-drawing">

```nasm
// output: main.s

	.section	__TEXT,__text,regular,pure_instructions
	.build_version macos, 13, 0	sdk_version 13, 3
	.globl	_main                           ; -- Begin function main
	.p2align	2
_main:                                  ; @main
	.cfi_startproc
; %bb.0:
	sub	sp, sp, #32
	.cfi_def_cfa_offset 32
	stp	x29, x30, [sp, #16]             ; 16-byte Folded Spill
	add	x29, sp, #16
	.cfi_def_cfa w29, 16
	.cfi_offset w30, -8
	.cfi_offset w29, -16
	mov	w8, #0
	str	w8, [sp, #8]                    ; 4-byte Folded Spill
	stur	wzr, [x29, #-4]
	adrp	x0, l_.str@PAGE
	add	x0, x0, l_.str@PAGEOFF
	bl	_printf
	ldr	w0, [sp, #8]                    ; 4-byte Folded Reload
	ldp	x29, x30, [sp, #16]             ; 16-byte Folded Reload
	add	sp, sp, #32
	ret
	.cfi_endproc
                                        ; -- End function
	.section	__TEXT,__cstring,cstring_literals
l_.str:                                 ; @.str
	.asciz	"Hello world"

.subsections_via_symbols
```



There are two separate process behind the compilation process (**front-end process**: lexical analysis, syntax analysis, semantic analysis, and **backend-process**: code generation, code optimization).

#### Front-end process:

1. Lexical analysis: will tokenize code into smallest (undivisible) form&#x20;
2. Syntax analysis: will check if the order of the token make sense (valid)
3. Semantic analysis: will discover whether syntactically correct statements actually make any sense

#### Back-end process:

1. Code optimization: code optimization will be done before generating the assembly code
2. Code generation: from the optimized and tokenized version of the code, we generate the assembler code (human-readable assembly) that match specified processor code (.s file)

## Assembling (Assembler)

<img src="../.gitbook/assets/file.excalidraw (3).svg" alt="" class="gitbook-drawing">

The assembler code from compilation process would be pass to assembler that convert human-readable assembler code (.s file) into machine-readable assembly code that match specified pvaocessor (object files, .o files).

The output of assembling process (object files) are already in bytes form (machine-readable), but you can reverse the operation of object file back to assembler code by doing this:

```bash
$ objdump -D <filename>.o
```

Object files looks like a tiles waiting to be tiled together (by the linker). It contains&#x20;

## Linking (Linker)



<img src="../.gitbook/assets/file.excalidraw (1).svg" alt="" class="gitbook-drawing">

Linking is the last process of GCC compilation process that produce runnable ELF executable file.

The linking process is divided into two process (tiling, and resolving symbols):

1. Relocating: the analogy of object files are puzzles waiting to be solved, to be arranged, this is the relocating process, it will try to map each of the object files sections into a program memory map, before the address of the component would be in **base-0** for each object file, after the relocating process, it would have more concrete address range, but still not fixed yet, because in this process we also need to resolve some missing address component (uninitialized data, reference in different files, etc.)
2. Resolving references: After relocating all the object files component into abstract memory format, linker will try resolving component that is not resolved locally. It needs to find in the program memory where this variable, reference is declared.

## Loading (Loader)
