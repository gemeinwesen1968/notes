Before the program gets compiled, it runs through a phase called preprocessing. It outputs the C code, then the code gets compiled.
# Simple Macros
A macro is a identifier that gets expanded to another piece of code before the compiler even sees it. It is a placeholder. We do this with \#define.
```c
#include <stdio.h>
#define HELLO "Hello, world"
#define PI 3.14159
int main(void) {
	printf("%s, %f\n", HELLO, PI);
}
```
You can also define a macro with no value.
```c
#define EXTRA
```
It can also be used to replace or modify keywords.
#c 