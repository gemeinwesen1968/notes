## Initializers of Nested structs and Arrays
If you have a nested substructure, you can initialize members of that substructure by following the variable names down the line:
```c
struct inside_struct {
	int inside_strct_1;
	int inside_strct_2;
};
struct out_struct {
	char *name;
	struct inside_struct child;
};
int main(void) {
	struct out_struct s = {
		.name = "Out Struct",
		.child.inside_strct_1 = 8,
		.child.inside_strct_2 = 12
	};
}

// another option
struct out_struct s { 
	.name = "Out Struct",
	.child = {
		.inside_strct_1 = 6,
		.inside_strct_2 = 3,
	}
};
```
## Self-Referential Struct
Used alot in graph-like structures. Example of imp. in C:
```c
#include <stdlib.h>

struct node {
	int data;
	struct node *next;
}

int main(void) {
	struct node *head;
	head = malloc(sizeof(struct node));
	head->data = 1;
	head->next = malloc(sizeof(struct node));
	head->next->data = 2;
	// ...
}
```
## Flexible Array Members
You can malloc extra space at the end after the struct, and then let the data overflow into that space. 
```c
struct len_string {
	int length;
	char data[8];
};
struct len_string *s = malloc(sizeof *s + 40); // 40 more bytes for string data to overflow
// this trick works only if the array is the last field of structure
```
```c
strcpy(s->data, "Hello, world!"); // data more than 8 bytes but program wont crash probably because of the extra 40 bytes.
```
But actually accessing the data beyond the end of that array is undefined behavior. But we can get the same effect with C99 and later, its legal:
```c
struct len_string {
	int length;
	char data[]; // the size will be determined at malloc
};
struct len_string *s = malloc(sizeof *s + len); // len is for the string
s->length = len;
memcpy(s->data, c, len); 
```
## Padding Bytes
C is allowed to add padding bytes within or after a struct as it sees fit. The total of sizeofs of fields can be smaller than the sizeof struct.
### offsetof
You can measure amount of padding bytes with offsetof, defined in <stddef.h>.
```c
printf("%zu\n", offsetof(struct foo, an_filed_of_foo));
printf("%zu\n", offsetof(struct foo, another_field_foo));
```
## Fake OOP
There is a slightly abusive thing thats sort of OOP-like that you can do with structs. Since the pointer to the struct is the same as a pointer to the first element of the struct, you can freely cast a pointer to the struct to a pointer to the first element.
```c
struct parent {
	int a, b;
};
struct child {
	struct parent super; // MUST be first
	int c, d;
};
```
Than we are able to pass a pointer to a struct child to a function that expects either that or a pointer to a struct parent.
```c
void print_parent(void *p) {
	struct parent *self = p;
	printf("Parent: %d, %d\n", self->a, self->b);
}
void print_child(struct child *self) {
	printf("Child: %d, %d\n", self->c, self->d);
}
int main(void) {
	struct child c = {.super.a = 1, .super.b = 2, .c = 3, .d = 4};
	print_child(&c);
	print_parent(&c); // works even though its a struct child
}
```
This works because pointer to a structure is pointer to the first field (similar to arrays). 
# Unions
These are just like structs, except the fields overlap in memory. The union will be only large enough for the largest field, and you can use only one field at a time. It is a way to reuse the same memory space for different types of data. 
```c
union foo {
	int a, b, c, d, e, f;
	float g, h;
	char i, j, k, l;
};
// if foo was a struct, it would take 36 bytes
// but now it takes only 4 bytes (max sizeof of fields)
// the tradeoff is you can use only one field at any time
```
You can non write to one union field and read from another. Doing so is called type punning. Usually used in low-level programming. 
Since the members of a union share the same memory, writing the one member necessarily affects the others. And if you read from one what was written to another, you get some weird effects. 
## Common Initial Sequences in Unions
If you have a union of structs and all those structs begin with a common initial sequence, its valid to access members of that sequence from any of the union members.
```c
struct a {
	int x;
	float y;
	
	char *p;
};
struct b {
	int x;
	float y;

	double *p;
	short z;
};
union foo {
	struct a sa;
	struct b sb;
};
```
What this rule tells us is that we are guaranteed that the members of the common initial sequences are interchangeable in code. That is: 
- f.sa.x is the same as f.sb.x.
- f.sa.y is the same as f.sb.y.
Because fields x and y are the both in the common initial sequence.
This allows us a way to safely add some shared information between structs in the union.
#c