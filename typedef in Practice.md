## Anonymous Struct
You do not actually need to name your struct in a variety of places. 

```c 
typedef struct { // created an alias (typedef) for given anon struct 
	char *name;
	int speed;
	// ...
} animal; 

animal z; // this works bc animal is an alias      
```

## Changing Multiple Variables Types
``typedef`` can be used to make changing types of multiple variables easier. Instead of changing every type manually, you can change what the alias is for. 

```c
typedef float app_float;

app_float f1, f2, f3, ..., fn;
```

```c
typedef long double app_float;  // changed types of f1, f2, f3, ..., fn in one line  
```

You can also use typedef on arrays and pointers but it abstracts too much unnecessarily.

#c