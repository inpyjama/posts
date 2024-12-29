+++
date  = "2023-09-06"
title = 'Harnessing Volatile and Const in Embedded C'

author = "Zahid Basha S K"
authorImage =""
preferred = "https://www.linkedin.com/in/skzahidbasha/"
linkedin = "https://www.linkedin.com/in/skzahidbasha/"
twitter = ""
blog = ""
email = ""

tags = [
    "c", "volatile", "const"
]

categories = [
    "c"
]

series = ["C Language"]
images = ["/post/volatile-const/0.webp"]
+++

This post explores the usage of the two prime keywords `volatile` and `const` in embedded applications and their significance in ensuring code correctness, reliability, and efficiency.

<!--more-->

![](0.webp)

## Let's start with the `volatile` keyword!

The `volatile` keyword is used to inform the compiler that a variable’s value may change unexpectedly, without the compiler’s knowledge. It tells the compiler not to consider that variable while optimizing the code. In real-time embedded systems this is crucial while dealing with hardware registers, shared memory, and interrupt service routines.

The `volatile` keyword also prevents variable from getting `CACHED` into `CPU REGISTERS`, but again this depends on that particular CPU’s optimization standards. Depending on it, a `volatile` variable can be or cannot be `CACHED` into `CPU REGISTERS`.

Consider an example of a 32-bit status register whose address is `0x40010000`, let’s assume that your embedded application requires to poll this register continuously until it becomes zero. Once it becomes zero then your application is designed for doing some functionality. The code implementation for this would be something like this :

```c
#include <stdio.h>
#include <stdint.h>

// Using “volatile” keyword with status_register.
volatile uint32_t *status_register = (uint32_t *) 0x40010000;

void main() {
    while(1)
    {
           if( *status_register == 0 )
           {
                // Application does some functionality here!
           }
    }

}
```

Since we are using `volatile` keyword for the `status_register`, it will make sure that the compiler fetches the value of status register from memory on each read, so it accurately fetches the hardware’s current state. The assembly (pseudocode) equivalent of the above code is given below:

```asm
Loop:
	LOAD *status_register, R0 ; Load the value of status_register from memory to register R0
	COMPARE R0, 0x0	          ; Compare R0 with 0x0
	JUMP_IF_NOT_EQUAL Loop 	  ; If not equal, jump back to Loop
	; Application does some functionality here
```

Now let’s write the same code without using `volatile` keyword with `status_register`. The code will look something like this:

```c
#include <stdio.h>

// Not using “volatile” keyword with status_register.
uint32_t *status_register = (uint32_t *) 0x40010000;

int main()
{
    while(1)
    {
           if( *status_register == 0 )
           {
                // Application does some functionality here!
           }
    }

    return 0;

}
```

Since we are not using the volatile keyword the compiler might optimize the read operation by caching the value of the status register in a register and not fetching it from memory on every iteration. This can lead to incorrect behavior because the loop may not detect changes in the hardware status register.

The assembly equivalent for the above code would be :

```asm
LoadStatus:
	LOAD *status_register, R0  ; Load the value of status_register from memory to register R0
Loop:
	COMPARE R0, 0x80000000 	; Compare R0 with 0x80000000
	JUMP_IF_NOT_EQUAL Loop 	; If not equal, jump back to Loop
	; Application does some functionality here!
```

## Now, let's understand `const` keyword!

You can use the `const` keyword with a variable to inform the compiler that the value of that variable should not be changed after its first initialization. Let’s take a simple example below to understand how `const` works:

```c
#include <stdio.h>
#include <stdio.h>

void main() {
  uint32_t const number = 10;
  uint32_t value;
  value = number * 2;
  printf( “ The variable value is : %d\n”, value );
}
```

Running the above code would give the below output, let’s try changing the variable `number` in the code.

```bash
❯ gcc main.c
❯ ./a.out
The variable value is : 20
```

```c
#include <stdio.h>

void main() {
  uint32_t const number = 10;
  number = number * 2;
  printf( “The variable number is : %d\n”, number );
}
```

Now this code would throw an error during compilation, since the variable `number` was qualified as `const` but was changed after its first initialization.

```bash
❯ gcc sample.c
❯ sample.c: In function ‘main’:
sample.c:6:10: error: assignment of read-only variable ‘number’
    6 |   number = number * 2;
      |          ^
```

It’s important to understand what are the different ways a const keyword could be used with a variable/pointer.

### Type 1: Constant Integer
Example: `uint32_t const value = 2;`

The above definition of the variable tells that “value” is a constant and its variable cannot be changed in the code.

### Type 2: Pointer To a Constant Integer
Example: `uint32_t const *ptr = (uint8_t *)0x40000000;`

The above definition of the variable tells us that `ptr` a is a pointer pointing to a constant integer variable, the value of the `ptr` could be changed, but the value pointed to by `ptr` could not be changed.

**Tip to read the above variable definition**: Start reading from right to left and it goes in this way - ptr (`ptr`) is a pointer (`*`) pointing to a constant variable (`const`) of type integer (`uint32_t`).

### Type 3: Constant Pointer To an Integer

Example: `uint32_t * const ptr = (uint8_t *)0x40000000;`

The above definition of the variable tells us that `ptr` is a constant pointer pointing to an integer variable. The value of `ptr` is constant and the value pointed by `ptr` could be changed.

### Type 4: Constant Pointer To a Constant integer

Example: `uint32_t const * const ptr = (uint8_t *)0x40000000`

In the above definition, `ptr` is a constant pointer to a constant integer value, in this scenario neither pointer could be modified nor the value pointed by it.

### Now let’s take an example of using `const` in embedded systems:

Consider there is a 32-bit register, whose address is being stored in a pointer called `reg_a`, so `const` would be used here to note that the value of the pointer cannot be changed during the course. The definition of the pointer would be as given below:

```c
uint32_t * const reg_a = (uint32_t *)0x40000000;
```

### Using Volatile and Const together!

Let us assume there is a status register whose address is `0x40000000` particular and this register is polled in the application for a particular value. Now we need the register to be out of optimization since we are polling it and also we don’t want to change or corrupt the variable that stores the address of this register. In this case, we could make use of both volatile and const keywords to get the work done. The definition goes as follows:

```c
uint32_t volatile * const status_reg = (uint32_t *)0x40000000;for
```

Some of the best practices for using volatile and const in Embedded C are as given below:
1. When accessing hardware registers or shared memory that could be modified externally, a volatile keyword could be used for skipping optimization.
1. `const` could be used for read-only variables like configuration settings, LUTs, and mathematical constants.
1. `volatile` could be used when using global variables that are used by interrupt service routines or within a multi-threaded application.
While using a read-only register a combination of volatile and const could be used.
