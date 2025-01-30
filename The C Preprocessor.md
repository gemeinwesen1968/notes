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
Overall, this gives you a way to define const values that are effectively global and can be used any place.
# Conditional Compilation
## \#ifdef and \#endif
```c
#include <stdio.h>
#define EXTRA // we defined it 
int main(void) {
#ifdef EXTRA // it is defined 
	printf("Defined!\n"); // will be printed
#endif
	printf("OK!\n");
}
```
To prevent including header files more than one:
```
#infdef HEADERNAME_H // if used, pass to the end of file
#define HEADERNAME_H // define it once
// code
#endif
```
There is also an \#else we can throw.
```
#ifdef EXTRA
	printf("Defined!\n");
#else 
	printf("Undefined!\n");
#endif
```
## General Conditional
Normal \#if \#elif \#else. This is about boolean expression rather than some value is defined or not. 
# Built-in Macros
| Macro     | Description                                       |
| --------- | ------------------------------------------------- |
| \__DATE__ | The date of compilation in Mmm dd yyyy            |
| \__TIME__ | The time of compilation in hh:mm:ss               |
| \__FILE__ | A string containing files name                    |
| \__func__ | The name of the function this macro appears on    |
| \__STDC__ | Defined with 1 if standard C compiler             |
| \__LINE__ | The line number of the file this macro appears on |
```c
#include <stdio.h>
int main(void) {
	printf("%s\n", _func_);
	printf("%s\n", __FILE__);
	printf("%d\n", __LINE__);
	printf("%s %s\n", __DATE__, __TIME__);
	printf("%ld\n", __STDC_VERSION__); // C version
}
```
![[Optimal Macros.png]]
# Macros with Arguments
You can set them up to take arguments that are substituted in. But just use functions.
```c
#define SQR(x) ((x) * (x))
printf("%d\n", SQR(3));
#define TRIANGLE_AREA(x, h) (0.5 * (x) * (h))
printf("%d\n", TRIANGLE_AREA(2, 5));
```
## Macros with Variable Arguments
There is also a way to have a variable number of arguments passed to macro, using ellipses (...) after known, named arguments. When the macro is expanded. all of the extra arguments will be in a comma-separated list in the \_\_VA_ARGS__ macro. 
```c 
#define X(a, b, ...) (10 *(a) + 20*(b)), __VA_ARGS__
// ...
	printf("%d, %f, %s, %d\n", X(5, 4, 3.14, "Hi!", 12))
```
You can also stringfy it by putting \#.
```c
#define X(...) #__VA_ARGS__
printf("%s\n", x(1, 2, 3)); // prints "1, 2, 3"
```
You can actually turn any arg into a string by preceding with a \#.
```c
#define STR(x) #x
```
We can also concatenate two arguments together with \#\#.
```c
#define CAT(a, b) a ## b
// ...
	printf("%f\n", CAT(3.14, 1592)); // 3.141592
```
## Multiline Macros
- Escapes at the end of every line except the last one to indicate that the macro continues.
- The whole thing is wrapped in a ``do-while(0)`` loop with {} brackets.
```c 
#define PRINT_NUMS_TO_PRODUCT(a, b) do { \
	int product = (a) * (b); \
	for (int i = 0; i < product; i++) { \
		printf("%d\n", i); \
	} \
} while(0)
```
## \#error Directive
This directive causes the compiler to error out as soon as it sees it. This is used inside a conditional to prevent compilation unless some prerequisites are met:
```c 
#ifndef __SOME_MACRO__
	#error Can not compile!
#endif
```

#c 