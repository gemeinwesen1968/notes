- You can get larger numbers in an unsigned variable than you can in a signed ones. (approx. 2 times)
-  Deep down, char is just a small int, an integer that uses just a single byte of mem. 
```c
char c = 'B';
printf("%d\n", c); // 66
```
- char is just a number, so you can do math on it.
```c
int main(void) {
	char a = 10;
	char b = 'B' // ASCII value 66
	printf("%d\n", a + b); 76
}
```
- The header file ``<limits.h>`` defines macros that hold the minimum and maximum integer values of type; rely on that to be sure.

| Type               | Min. Bytes | Min Macro | Max Macro  |
| ------------------ | ---------- | --------- | ---------- |
| char               | 1          | CHAR_MIN  | CHAR_MAX   |
| signed char        | 1          | SCHAR_MIN | SCHAR_MAX  |
| short              | 2          | SHRT_MIN  | SHRT_MAX   |
| int                | 2          | INT_MIN   | INT_MAX    |
| long               | 4          | LONG_MIN  | LONG_MAX   |
| long long          | 8          | LLONG_MIN | LLONG_MAX  |
| unsigned char      | 1          | 0         | UCHAR_MAX  |
| unsigned short     | 2          | 0         | USHRT_MAX  |
| unsigned int       | 2          | 0         | UINT_MAX   |
| unsigned long      | 4          | 0         | ULONG_MAX  |
| unsigned long long | 8          | 0         | ULLONG_MAX |
- C provides bunch of macros in ``<float.h>`` that lets you see how many significant decimal digits can it store in a given floating point type.  

| Type        | Decimal Digits You Can Store | Minimum |
| ----------- | ---------------------------- | ------- |
| float       | FLT_DIG                      | 6       |
| double      | DOUBLE_DIG                   | 10      |
| long double | LDBL_DIG                     | 10      |

#c 