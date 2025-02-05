``brk()`` and ``sbrk()`` change the location of the program break, which defines the end of the process's data segment. The program break is the first location after the end of the uninit. data segment. Increasing the program break allocates memory to the process; decreasing the break deallocates. 
- brk() => sets the end of the data segment to the value specified by addr.
- sbrk() => increments the program's data space by increment bytes. Calling with an increment 0 can be used to find the current location of the program break.
```c
#include <unistd.h>
int brk(void *addr);
void *sbrk(intptr_t increment);
```
#c #memory