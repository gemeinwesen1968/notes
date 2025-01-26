# Type Qualifiers
## const
Means the variable is constant, and any attempt fo modify it will result in failure.
You can use const with pointers:
```c
int x[] = {10, 20};
const int *p = x; // == int const *p = x; 
p++; // can modify p, no problem
*p = 30; // ERROR! Cannot change the object it points to

// the other way around:
int *const p; // cannot modify p with pointer arithmetic
p++; // compiler error!
*p = 20; // no problem, the pointed object is not const

// both:
const int *const p; // cannot modify both p and *p
```
If you try to make a pointer to a int point to a const int, the compiler will warn:
```c
const int x = 20;
int *p = &x; // p is int* but &x is const int*, undefined behavior 
```
## restrict
A hint to the compiler that a particular piece of memory will only be accessed by one pointer and never another. If you try to access to a restricted object by another way (via another pointer), the behavior is undefined. You guarantee that one single pointer is the only way to access the object, and C uses that information to perform optimizations. restrict has block scope, it lasts for the scope it is used.
```c 
void swap(int *restrict a, int *restrict b) {
	int t;
	t = *a;
	*a = *b;
	*b = t;
}
int main(void) {
	int x = 10, y = 20;
	swap(&x, &y); // ok! a and b point to different objects
	swap(&x, &x); // undefined behavior! a and b point to same thing
}
```
If you are using array notation instead pointer notation:
```c
void foo(int[restricted])
void foo(int[restricted 10])
```
## volatile
Unlikely to see or need unless dealing with hardware direclty. Tell the compiler that a value might change behind its back and should be looked up every time (e.g. some hardware timer). You are telling the compiler the thing volatile points at might change at any time for reasons outside of the source code.
```c
volatile int *p;
```

# Storage-Class Specifiers
## auto
Indicates that this object has automatic storage duration. It exists in the scope in which it is defined, and is automatically deallocated when the scope is exited. Their value is indeterminate until you explicitly initialize them. Always initialize all automatic variables before use! 
```c
int a; // auto is the default
auto int a; // this is redundant
```
## static
### static in Block Scope
A single instance of the variable to exist, shared between calls. Will only be initialized one time on program startup, not each time the function is called.
```c
void function(void) {
	static int count = 1; // initialized one time, not on every call
	// ...
}
int main(void) {
	function(); // 
	function(); // already initialized before call, every call shares it
	// ... 
}
```
### static in File Scope
The variable is not visible outside of this particular source file. Like "global", but only for this file. You can declare a function static if you only want it visible in a single source file.
## extern
Gives us a way to refer to objects in other source files.
```c
// bar.c
int a = 37;

// foo.c
extern int a;
int main(void) {
	printf("%d\n", a); // 37, from bar.c
	a = 99;
	printf("%d\n", a); // 99 now, again from bar.c
}
```
## register
This keyword hints to compiler that the variable is frequently-used, and should be made as fast as possible to access. The compiler does not have to agree to it. Its rare since modern C compiler optimizers are pretty effective at figuring it out themselves. You can not take the address of a register.
#c 