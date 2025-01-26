## String Conversions
### Numeric to String
You can use either ``sprintf()`` or `snprintf()`. They output to a string, and you can print it later.
```c
char s[10];
float f = 3.142334;
snprintf(s, 10, "%f", f);
printf("String value: %s\n", s);
```
### String to Numeric
For basic conversion from a string to a number, try the atoi functions from `<stdlib.h>`. Use them carefully. atoi, atof, atol, atoll.
```c
char *pi = "3.14159";
float f = atof(pi);
printf("%f\n", f);

int x = atoi("what"); // undefined behaviour
```
For better error handling, you can use strtol functions. It also comes with more type conversions.
```c
char *s = "3490";
unsigned long int x = strtoul(s, NULL, 10); // last arg is base of counting
char *s_base2 = "101010";
unsigned long int x = strtoul(s_base2, NULL, 2); // base 2 
```
Second arg is to see what went wrong! If nothing went wrong, it points to (it is actually a pointer to a pointer to a char) NUL terminator.
```c
char *s = "3490";
char *badchar;
unsigned long int x = strtoul(s, &badchar, 10);
if (*badchar == '\0') {
	// success, continue!
} else {
	// failed, do something!
	printf("Invalid char: %c\n", *badchar); // where things went wrong
}
```
## char Conversions
You can use basic arithmetic.
```c
// we can subtract '0' to get numeric value
char c = '6';
int x = c; // numeric equivalent in ASCII
int y = c - '0'; // difference is number 6

// or other way
int x = 6;
char c = x + '0'; // c is '6'
```

#c 