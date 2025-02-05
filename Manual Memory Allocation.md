You can tell C explicitly to allocate for you a certain number of bytes that you can use as you please. This bytes will remain allocated until you explicitly free that memory. 
If you manually allocated it, you have to manually free it when you are done with it. 
Automatic local variables are allocated "on the stack", and manually allocated memory is "on the heap". 
## malloc and free
The ``malloc()`` function accepts a number of bytes to allocate, and returns a [[void Pointers|void pointer]] to that block of memory. It is a void*, so you can cast it to whatever pointer type that corresponds to the number of bytes allocated.
After we are done with allocated memory, we can call ``free()`` to indicate we are done with that memory. It is undefined behaviour to use a memory region after you free() it.

```c
int *p = malloc(sizeof(int));
// ...
free(p);
// you can also:
int *a = malloc(sizeof *a);
```
## Errors
All the alloc functions return a pointer to newlallocated memory, or NULL if the memory cannot be allocated for some reason.
```c
int *x;
x = malloc(sizeof(int) * 10);
if (x == NULL){
	// do something to handle if an error occurs
} 
```
## Allocating Space for Arrays
You can just multiply the sizeof the type with number of elements in array, then access them using either pointer or array notation. 
```c
int *p = malloc(sizeof(int) * 10); // allocate 10 * 4 bytes, uninitialized
for (int i = 0; i < 10; i++) 
	p[i] = i * 5; // initialize
// ...
free(p); // free the allocated 10 * 4 bytes
```
## calloc
malloc() for allocating arrays. Pass the size of one element and the number of elements. It initializes to default (seems like the main difference). You still use free() to deallocate. 
```c
// alloc 10 * 4 bytes, initialize all members to 0:
int *p = calloc(10, sizeof(int));

// same thing but with malloc:
int *q = malloc(10 * sizeof(int));
memset(q, 0, 10 * sizeof(int)); // set all elements to 0
```
## Changing Allocated Size with realloc
Grows or shrinks that memory, and returns a pointer to it. Sometimes it might turn the same pointer, or it might return a different one. 
```c
np = realloc(p, new_num * sizeof(type));
p = np; 

// this two are equivalent:
char *p = malloc(3490);
char *p = realloc(NULL, 3490); // this can be convenient if you have allocation loop

// for example:
int *p = NULL;
int length = 0;
while (!done) {
	length += 10;
	p = realloc(p, sizeof *p * length);
	//...
}
```

# Example: Reading in Lines of Arbitrary Lenght:
```c
char *readline(FILE *fp) {
	int offset = 0; // index next char goes in the buffer
	int bufsize = 4;
	char *buf;
	int c;

	buf = malloc(bufsize); // allocate initial buffer
	if (buf == NULL) 
		return NULL;

	while (c = fgetc(fp), c != '\n' && c != EOF) {
		if (offset == bufsize - 1) { // if bufsize not enough
			bufsize *= 2; // increase bufsize
			char *new_buf = realloc(buf, bufsize * sizeof(char));
			if (new_buf == NULL) {
				free(buf);
				return NULL;
			}
			buf = new_buf;
		}
		buf[offset++] = c;
	}

	if (c == EOF && offset == 0) {
		free(buf);
		return NULL;
	}

	if (offset < bufsize - 1) { // if bufsize is bigger, shrink
		char *new_buf = realloc(buf, offset + 1);
		if (new_buf != NULL) {
			buf = new_buf;
		}
	}
	buf[offset] = '\0';
	return buf; // freeing the buffer is up to the caller
}
```


#c #memory 