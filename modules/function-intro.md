# Function introduction

Functions can be used to encapsulate some work, for code re-factoring, etc.

A function has a single return value and multiple input parameters.

- if you also need to extract an error code with the value or get multiple
  return values that needs to be done via passing data via reference (more on
  that when we have pointers)
- i.e. this is not like Go that returns the error along with the data

A function *declaration* is only declaring the API without the function body.
It is called a *function prototype*:

```C
int digit(int c);
int space(int c);
float myfun(int c, int i, float f);
```

The default return value type is an `int` but a compiler will warn if it is
missing.  You should always specify the type in
[C99+](/modules/c99-standard.md).

Write your function declarations at the beginning of a C file or include those
into a separate header file.

If a compiler hits a function whose prototype is unknown, warnings are issued.
Remember [src/hello-world1.c](/src/src/hello-world1.c)?  See also here:

```
$ cat test.c
int
main(void)
{
	fn();
}

float
fn(void)
{
	return (1.0);
}
$
$ gcc test.c
test.c:4:2: warning: implicit declaration of function 'fn' is
invalid in C99
      [-Wimplicit-function-declaration]
	fn();
	^
test.c:8:1: error: conflicting types for 'fn'
fn(void)
^
test.c:4:2: note: previous implicit declaration is here
	fn();
	^
1 warning and 1 error generated.
```

Argument names may be omitted in prototypes, i.e.:

```C
int myfn(int, int);
```

As you can see, function arguments are separated by commas and the function body
is inclosed in `{}` (just like the `main` function).

A variable defined in function parameters or locally within the function
overrides global variables.  Each identifier is visible (= can be used) only
within a region of program text called its *scope*.  See
[C99](/modules/c99-standard.md), section *6.2.1  Scopesof identifiers* for more
information.

You may use a function argument as any other local variable.

```C
int
myfn(int i, int j)
{
	i = i * j;
	return (i);
}
```

As **arguments are passed by value**, the variable value is not changed in scope
that called the function.

```C
/* myfn defined here */

int
main(void)
{
	int i = 3;

	myfn(i, i);
	printf("%d\n", i);	// will print 3
}
```

Local variables are on the stack.

Variable passing depends on bitness and architecture. E.g. 32-bit x86 on the
stack, 64-bit x64 ABI puts first 6 arguments to registers, the rest on the
stack.

Functions can be recursive.  :eyes: [recursive-fn.c](/src/recursive-fn.c)

```C
/*
 * Print 0..9 recursively using a function call fn(9).
 */
#include <stdio.h>

void
myfn(int depth)
{
	if (depth != 0)
		myfn(depth - 1);
	printf("%d\n", depth);
}

int
main(void)
{
	myfn(9);
}

```

:wrench: Print 9..0 using a recursive function.

:eye: solution [recursive-fn2.c](/src/recursive-fn2.c)