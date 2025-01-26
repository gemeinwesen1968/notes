Two use cases:
- Function is going to operate byte-by-byte. ``memcpy()`` copies bytes of memory from one pointer to another, but those pointers can point to any type. No matter what the type the object is, function is iterating through the bytes. 
- Another function passes the object as ``void*`` and you convert it to the type you need. (a method to achieve generics)

```c
char s[] = "Goats!";
char t[100];
// strcpy is more efficient to copy strings!
memcpy(t, s, 7) // inclde the NUL terminator!
printf("%s\n", t); // Goats!

int a[] = {11, 22, 33};
int b[3];
memcpy(b, a, 3 * sizeof(int)); // copy 3 int of data
printf("%d\n", b[1]); // 22
```

You cannot do [[Pointer Arithmetic]] on a void pointer. You cannot dereference a void pointer. You cannot use arrow operator on a void pointer, since it is actually a dereference. You cannot use array notation on a void pointer, since it is also a dereference. 

#c 