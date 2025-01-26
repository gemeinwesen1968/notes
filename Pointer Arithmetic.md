```c
int a[5] = {11, 22, 33, 44, 55};
int *p = a; // an array is a pointer to the first element

printf("%d\n", *p); // 11
printf("%d\n", *(p + 1)); // prints 22! (a[1]) 
```

Compiler knows p is a pointer of type int. So it knows the ``sizeof`` of an int. It skips the amount of that bytes multiplied by constant and gives the intended address. 

```c
*(p + 1) // dereferences the value in addres p + 4 bytes (sizeof int == 4)
```

This is actually how the array notation of getting value works. Think of it as a syntax sugar. **A pointer is an index into memory!**

This syntax sugar makes it possible to increment or decrement the pointer and move in array.

```c
int a[] = {11, 22, 33, 44, 55, 999};
int p = a;

while (*p != 999) {
	printf("%d\n", *p);
	p++; // move p 4 bytes ahead
}
```

You can also substract two pointers to find the difference between them. Like using the difference to find how many variable of type is in between. This would only work with a single array.

```c
char *s = "Hello, world!";
char *p = s;
while (*p != '\0') {
	p++;
}
int diff = p - s; // length of the string s
```


#c