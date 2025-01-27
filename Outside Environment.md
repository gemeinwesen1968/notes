# Command Line Arguments
- `argc` => *argument count*, including the program name itself. 
- `**argv` => list of the arguments, a pointer to string (a pointer to a pointer to a char)
```c
// the structure of a program with command line arguments
int main(int argc, char **argv) {
 // ...
}
```
- argv after the last string is a pointer to NULL.
```c
argv[argc] == NULL // true 
```
- because that arrays are also pointers, you can also write argv as: 
```c 
int main(int argc, char *argv[]) { // array of strings
	// ...
}
```
- argv being a pointer means that you can apply [[Pointer Arithmetic]]to it
- You can modify argc, argv, or any of the strings that argv points to. Do not make those strs longer than they already are. (Just dont modify?)

# Exit Status
Special case for main(): if execution reaches the end of main() without finding a return, it automatically does a ``return 0``;
You are running your program from another program:

- The shell launches your program.
- The shell typically goes to sleep.
- Your program runs.
- Your program terminates.
- The shell wakes up and waits for another command.

Between the last two steps, the program can return a status value that the shell can interrogate. This value is used to indicate the success or failure of your program.
- EXIT_SUCCESS or 0 => Program terminated successfully.
- EXIT_FAILURE => Program terminated with an error.
```c
#include <stdio.h>
#include <stdlib.h>
int main (int argc, char **argv) {
	if (argc != 3) {
		printf("usage: mult x y\n");
		return EXIT_FAILURE;
	}
	printf("%d\n", atoi(argv[1]) * atoi(argv[2]));
	return 0;
}
```
In Bash or another POSIX shell, you can use `echo $?` to see exit status of program.
```bash
$ ./mult
usage: mult x y
$ echo $?
1
$ # EXIT_FAILURE is 1
```
# Environment Variables
The shell you run your program in can make its own variables. It is possible to get these variables from inside your C program.
```c
char *val = getenv("HEHE"); //try to get variable HEHE from shell
if (val == NULL) { // if could not find the variable
	// do something
	return EXIT_FAILURE;
}
```
On a Unix-like system, you can also:
```c
extern char **environ; // get list of environment variables
// terminated with a NULL
```
Each string of environ is in the form of "key=value" so you will have to split it and parse it yourself.
Another way to get environment variables, common in Unix-like.
#c 