# The mother of all coding languages

- A C program consists of the following parts:
1. Preprocessor Stuff (aka Macros)
```C
#include <stdio.h> // Preprocessor Directive - stdio is part of the standard C lib
```


## Ritual First Program

```C
#include <stdio.h>
int main() {
	printf("Hello World\n");
}
```
**NOTE**:
- `include` is needed for importing code from other files into our program. In this case we are including the **C standard IO lib**. the `.h` ending is for *header files* which are files used for declarations usually. This allows us to fetch the `printf()`  function.

## Data Types

| Data Type | Bytes   | Largest Number |
| --------- | ------- | -------------- |
| char      | 1 byte  | 255            |
| short     | 2 bytes | 65535          |
| int       | 4 bytes | over 4 billion |
| long      | 4 bytes | over 4 billion |
| long long | 8 bytes | 2^64 - 1       |
These are can be both signed (range is halved) and unsigned. **WARN** this is **USUALLY** some micro-controllers, int might be 2 bytes so you need to know the specifics of the platform you are working on.
So it might be better to use fixed width integers defined int he the `stdint.h` lib which declares them like:
```C
#include <stdint.h>
int main() {
	// No matter what compiler you would use these are always take the same amount of space
	int8_t // integer which is 8 bits (1 byte) long
	uint64_t // unsigned int which which is 64 bits (8 bytes) long
}
```

Now to represent decimals, there are `floats` and `doubles` (double the precision than float hence the name). But these are not always super accurate (obviously)
So you should not use them to handle things like money. Always use ints to present them (eg: if you handling INR, always keep money stored as the unit value int *paisa* instead of presenting them as int *rupees* cause there can be 10.5 rupees but not paisa)

