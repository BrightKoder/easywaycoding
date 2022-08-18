# The Beginner's Handbook: Learn C Programming Language

## Introduction: 
The C programming language is a general purpose, and procedural programming language, which relates closely to the way machines work. Understanding how computer memory works is an important aspect of the C programming language. Although C can be considered as "hard to learn", C is in fact a very simple language, with very powerful capabilities.

The main features of the C language include low-level memory access, a simple set of keywords, and a clean style, these features make C language suitable for system programmings like an operating system or compiler development. 

C is a very common language, and it is the language of many applications such as Windows, the Python interpreter, Git, and many many more.

C is a compiled language - which means that in order to run it, the compiler (for example, GCC or Visual Studio) must take the code that we wrote, process it, and then create an executable file. This file can then be executed, and will do what we intended for the program to do.

### What is Compiler?
Computers understand only one language and that language consists of sets of instructions made of ones and zeros. This computer language is appropriately called machine language.

A single instruction to a computer could look like this:

``` 
00000	10011110 
```
As you can imagine, programming a computer directly in machine language using only ones and zeros is very tedious and error prone. To make programming easier, high level languages have been developed. High level programs also make it easier for programmers to inspect and understand each other's programs easier.

Because a computer can only understand machine language and humans wish to write in high level languages high level languages have to be re-written (translated) into machine language at some point. This is done by special programs called compilers, interpreters, or assemblers that are built into the various programming applications.**Compiler** is a software which converts a program written in high level language (Source Language) to low level language (Object/Target/Machine Language).

![download](https://user-images.githubusercontent.com/83718460/117564220-ac900700-b0c8-11eb-994a-f08856f4ef4c.png)

C,C++ is designed to be a compiled language, meaning that it is generally translated into machine language that can be understood directly by the system, making the generated program highly efficient. For that, a set of tools are needed, known as the development toolchain, whose core are a compiler and its linker.

### What is a compilation?
The compilation is a process of converting the source code into object code. It is done with the help of the compiler. The compiler checks the source code for the syntactical or structural errors, and if the source code is error-free, then it generates the object code [.obj (in Windows) or .o (in Linux)] .

![source-code-and-object-code](https://user-images.githubusercontent.com/83718460/117564224-b87bc900-b0c8-11eb-8dab-327c86f545a3.png)

### Compilation Stages
The following are the four Compilation stages through which our program passes before being transformed into an executable form:
1. Preprocessor
2. Compiler
3. Assembler
4. Linker
 
 The below image describes the entire C compilation process.
 <br>
 ![c-compilation-process](https://user-images.githubusercontent.com/83718460/117564162-54f19b80-b0c8-11eb-9909-7b919d9045ce.png)
 
 To take a deep dive inside the C compilation process let’s compile a C program. Write or copy below C program and save it as ``` compilation.c.```
 
 ```C
 /* Learning C compilation process */
#include <stdio.h>

int main()
{
    printf("Hello, World!");
    return 0;
}

```
To compile the above program open command prompt/terminal and hit below command.
```
gcc -save-temps compilation.c -o compilation
```
The ``` -save-temps ``` option will preserve and save all temporary files created during the C compilation. It will generate four files in the same directory namely.

```
1. compilation.i   (Generated by pre-processor)
2. compilation.s   (Generated by compiler)
3. compilation.o   (Generated by assembler)
4. compilation     (On Linux Generated by linker) or (compilation.exe On Windows)
```
Now lets look into these files and understand different stages of compilation.

#### 1. Pre-processing of source file
The C compilation begins with pre-processing of source file. Pre-processor is a small software that accepts C source file and performs below tasks.

- Remove comments from the source code.
- Macro expansion.
- Expansion of included header files.

After pre-processing it generates a temporary file with .i extension. Since, it inserts contents of header files to our source code file. Pre-processor generated file is larger than the original source file. Also we can generate ```<file-name>.i``` by using the following command:<br><br>
``` gcc -E compilation.c ``` [In Linux]

To view contents of the pre-processed file open ``` <file-name>.i``` in your favourite text editor. As in our case below is an extract of ```compilation.i``` file.
```i
# 1 "compilation.c"
# 1 ""
# 1 ""
# 1 "compilation.c"

# 1 "C:/Program Files (x86)/CodeBlocks/MinGW/include/stdio.h" 1 3
# 293 "C:/Program Files (x86)/CodeBlocks/MinGW/include/stdio.h" 3
 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) fprintf (FILE*, const char*, ...);
 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) printf (const char*, ...);
 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) sprintf (char*, const char*, ...);

 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) scanf (const char*, ...);
 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) sscanf (const char*, const char*, ...);


...
...
...

 int __attribute__((__cdecl__)) __attribute__ ((__nothrow__)) putw (int, FILE*);
# 3 "compilation.c" 2

# 4 "compilation.c"
int main()
{
   printf("Hello, World!");
   return 0;
}

```
You can notice that the statement #include <stdio.h> is replaced by its contents. Comment before the #include line is also removed.
#### 2. Compilation of pre-processed file
In next phase of C compilation the compiler comes in action. It accepts temporary pre-processed ```<file-name>.i``` file generated by the pre-processor and performs following tasks.

Check C program for syntax errors.
Translate the file into intermediate code i.e. in assembly language.
Optionally optimize the translated code for better performance.
After compiling it generates an intermediate code in assembly language as ```<file-name.s>``` file. It is assembly version of our source code.

Also we can generate ```<file-name>.s``` by using the following command:<br><br>
``` gcc -S compilation.c ``` [In Linux]

Let us look into ```compilation.s``` file.

```s
	.file	"compilation.c"
	.text
	.section	.rodata
.LC0:
	.string	"Hello, World!"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	leaq	.LC0(%rip), %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	 1f - 0f
	.long	 4f - 1f
	.long	 5
0:
	.string	 "GNU"
1:
	.align 8
	.long	 0xc0000002
	.long	 3f - 2f
2:
	.long	 0x3
3:
	.align 8
  
  ```
  
#### 3. Assembling of compiled source code
Moving on to the next phase of compilation. Assembler accepts the compiled source code (compilation.s) and translates to low level machine code. After successful assembling it generates ```<file-name.o>``` (in Linux) or ```<file-name.obj>``` (in Windows) file known as object file. In our case it generates the ```compilation.o``` file.

Also we can generate ```<file-name>.o``` by using the following command:<br><br>
``` gcc -c compilation.c ``` [In Linux]

This file is encoded in low level machine language and cannot be viewed using text editors. However, if you still open this in notepad, it look like.
```o
ELF          >                    (          @     @  
 óúUH‰åH=    ¸    è    ¸    ]ÃHello, World!  GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0                GNU   À                 zR x                 E†C
W                               ñÿ                                                                                                                                                         	                                                                                  *                       compilation.c main _GLOBAL_OFFSET_TABLE_ printf                     üÿÿÿÿÿÿÿ             üÿÿÿÿÿÿÿ                       .symtab .strtab .shstrtab .rela.text .data .bss .rodata .comment .note.GNU-stack .note.gnu.property .rela.eh_frame                                                                                           @                                            @               h      0                           &                     `                                      ,                     `                                      1                     `                                     9      0               n       ,                             B                      š                                      R                                                            j                     À       8                              e      @               ˜                	                                       ø       8         
                 	                      0      1                                                    °      t                              
```
#### 4. Linking of object files
Finally, the linker comes in action and performs the final task of compilation process. It accepts the intermediate file ```<file-name.o>``` generated by the assembler. It links all the function calls with their original definition. Which means the function ```printf()``` gets linked to its original definition.

Also we can generate ```<file-name>.o``` by using the following command:<br><br>
``` gcc -o  compilation compilation.c ``` [In Linux]

Linker generates the final executable file (```.exe``` in windows and ```compilation``` in linux).
##
> Note:<br> 
> Valid only in Linux(GCC) <br>
> If we compile code by using below method then it generates `a.out` as executable file.<br>
> `gcc file-name.c`<br>
>  if we compile code by using below method then we can give diffrent name for executable file.<br>
>  ```gcc -o executable-name file-name.c``` <br>
## 

### Basic C Terminology: 
Every C program uses libraries, which give the ability to execute necessary functions. For example, the most basic function called ```printf```, which prints to the screen, is defined in the ```stdio.h``` header file.

To add the ability to run the printf command to our program, we must add the following include directive to our first line of the code:

``` 
#include <stdio.h> 
```
The second part of the code is global variables, which will be accessible through out the program.
```
int gVar; /* global variable */
```
Here ```gVar``` is accessible through out the program.

The third part of the code is the actual code which we are going to write. The first code which will run will always reside in the ```main``` function.

```
int main()
{
  /* main() function body starts here */
  int lVar; /* local variable */
}
```
The ```int``` keyword indicates that the function ```main``` will return an integer - a simple number. The number which will be returned by the function indicates whether the program that we wrote worked correctly. If we want to say that our code was run successfully, we will return the number 0. A number greater than 0 will mean that the program that we wrote failed.

Local variables will be accessible in the functions where they are declared/defined. Here ```lVar``` is accessible only in main function.

For this tutorial, we will return 0 to indicate that our program was successful:
```
return 0;
```

Notice that every statement in C must end with a semicolon, so that the compiler knows that a new statement has started.

Last but not least, we will need to call the function ```printf``` to print our sentence.

### Types of program errors
We distinguish between the following types of errors:

1. Syntax errors: errors due to the fact that the syntax of the language is not respected.
2. Semantic errors: errors due to an improper use of program statements.
3. Logical errors: errors due to the fact that the specification is not respected.

From the point of view of when errors are detected, we distinguish:
1. Compile time errors: syntax errors and static semantic errors indicated by the compiler.
2. Runtime errors: dynamic semantic errors, and logical errors, that cannot be detected by the compiler (debugging).

### Exercise

```c
#include<stdio.h>

int main()
{
  printf("Hello World!\n");
  return 0;
}
```
```
Output:
Hello World!
```

