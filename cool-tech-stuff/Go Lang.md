# Go
1. [Go Lang Tutorials for getting started](https://go.dev/learn/#guided-learning-journeys)
2. [Go Lang philosophy](https://go.dev/doc/effective_go)
3. Books & Good Resources:
	1. Let's Go Futher (Go based backend APIs and web dev)
	2. [Learn GO With Tests](https://quii.gitbook.io/learn-go-with-tests)
4. [100 Go Mistakes and How to avoid them](https://100go.co/)

## Go: Hello World

```go
package main 

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```
Here is the breakdown of those three blocks:

**1. `package main`**
- **The Concept:** In Go, every file belongs to a package.
- **The Rule:** If you want to build an **executable** (a program you can run), the package _must_ be named `main`.
- **The Contrast:** If you named this `package utils`, Go would treat it as a library to be imported, not a program to be run.

**2. `import "fmt"`**
- **The Concept:** This is the Standard Library (batteries included).
- **The Detail:** `fmt` stands for "Format." It handles input and output (I/O). It is Go's equivalent to Node's global `console` object, but much more powerful.

**3. `func main() { ... }`**
- **The Concept:** The Entry Point.
- **The "Low Level" Detail:** When your OS runs the binary, the Go runtime looks specifically for this function to start execution. When `func main` finishes, the program dies immediately. There is no implicit event loop keeping it alive (unless you create one).

## Running the Code

Two Ways:

### Method A: Temp Run
The "Dev" Way, the `run` command would compule the code in a temp folder, runs it and then deletes the binary. Perfect for development.
```bash
go run main.go  # should show output in terminal
```

### Method B: Compile to Binary
The "Production" way. This compiles your code into a standalone machine code binary.
```bash
go build main.go  # build the binary machine code file/executable
./main  # execute that bin
```

## 6 Main Points about GO
### 1. Statically types Language

```go
var msg string = "hello world"
var intger = 14  // GO can infer types also. But you must specify the value of the variable.
```

### 2. Strongly Typed Language

```go
var a = 1
var b = "two"
var c = a + b  // will throw an error
```

### 3. Go is Compiled
- Compiled languages are always *faster* than interpreted

### 4. Go is Compiled FAST AF
- compile times itself it pretty fast. Great for devs like us cause we can add features, compile and test them out pretty quickly for even minor changes.

### 5. Built in Concurrency
- Don't need special packages and third party libs to write concurrent working codes. 
- It is something baked into the language using **GoRoutines**

### 6. Simplicity 
- Do a lot with less code
- Automatic garbage collection for cleaning up unused memory

## Modules and Packages

### Packages
- Package is just a dir which contains a collections of go files

### Modules
- A collection of packages is called a module.
- When we init a new project, we actually init a new module
```bash
go mod init <name>
```
- The naming standard of the go project is usually the name of the git repository location of it. But it can be the project name also
- This will create a `go.mod` file which would contain the modules you using (only the mod name you just gave for a new project. It would also contain name of external mods if we use them) and the go version we are using

## Constants and Variables

- variables are declared with the `var` keyword
```GO
var intNum int
```
- constants are declares with the `const` keyword
```GO
const bestFriend = "Soma"
```

## Data Types 

### Integers
- There are a lot of int types:
```GO
int  // will default to int32 or int64 depending on computers architecture
int8
int16
int32
int64

uint // unsigned ints (only positive) and have same bit sizes are normal ints => can store numbes twice as large in the same amount of memory
```
- Is is so that we think about the ranges and kind of value we would deal with.
- Eg: For storing rgb colors, we only need a int8 

| **Type** | **Size**       | **Range (Approx)** | **When to use?**                                                                                                                          |
| -------- | -------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `int`    | **32/64 bits** | Depends on CPU     | **The Default.** Use this for loops, counts, and basic math. It matches your machine's architecture.                                      |
| `int64`  | 8 bytes        | ±9 Quintillion     | Timestamps (Unix Epoch), Database IDs (BigInt), file sizes.                                                                               |
| `int32`  | 4 bytes        | ±2 Billion         | Handling data from systems that are strictly 32-bit. (Also, a `rune` is an alias for this).                                               |
| `int16`  | 2 bytes        | ±32,000            | Parsing audio files or legacy network protocols.                                                                                          |
| `int8`   | 1 byte         | -128 to 127        | working with raw binary streams.                                                                                                          |
| `uint`   | **32/64 bits** | 0 to Max           | **Unsigned** (Positive only). Use only if you need bitwise operations or specifically need to prevent negative numbers at the type level. |
### Floats
- There are no default float, there is only:
```GO
float32
float64
```

The general rule in Go is: **"Use the defaults unless you have a very specific reason not to."** (int and float64)
### Maths
- We cannot math 2 different types (even if they are ints and floats)
```GO
var flaotNum32 float32 = 10.1
var flaotNum64 float64  = 10.1
fmt.println(floatNum32 + floatNum64)  // Error: invalid operation: floatNum32 + floatNum64 (mismatched types float32 and float64) compiler (MismatchedTypes) [20, 10]
```
- If we need to do maths shit on two different datatypes, we would need to convert one of them into a common datatype:
```GO
var intNum32 int32 = 10
var floatNum32 float32 = 10
fmt.println(float32(intNum32) + floatNum32)
```

- By default `/` does division and `%` gives back the remainder
```GO
// Integer Divison
fmt.Println(3 / 2)   // Output: 1

// Float Division
fmt.Println(3.0 / 2) // Output: 1.5
```
- Same rule here for different types:
```GO
func main() {
    var a int = 3
    var b int = 2

    // ❌ WRONG: Go will still do integer division first!
    // var result float64 = float64(a / b) 
    // Result would be 1.0 (it calculates 3/2 = 1 first, THEN converts to float)

    // ✅ CORRECT: Convert the operands individually
    var result float64 = float64(a) / float64(b)
    
    fmt.Printf("The result is: %.2f\n", result) // Output: 1.50
}
```



### String (are weird)

| **Syntax** | **Name**           | **What is it really?**                                                           | **usage**                               |
| ---------- | ------------------ | -------------------------------------------------------------------------------- | --------------------------------------- |
| `""`       | **String Literal** | An immutable slice of bytes. Interprets escape characters (like `\n` or `\t`).   | Normal text.                            |
| `` ` ` ``  | **Raw String**     | An immutable slice of bytes. Ignores escape characters. Can span multiple lines. | SQL queries, Regex, JSON blobs.         |
| `''`       | **Rune Literal**   | **NOT A STRING.** It is an Integer (`int32`).                                    | Represents a single Unicode Code Point. |
**Crucial Concept:** In Go, single quotes `''` do not create strings. They create **Runes**. A `rune` is just an alias for `int32`. When you write `'A'`, Go sees the number `65`. When you write `'Γ'`, Go sees `915`.

***IMPORTANT***
- **`len()` counts bytes**, not characters.
- Go strings are encoded in **UTF-8**:
	- **Standard ASCII (0-127):** Takes **1 byte**. (e.g., 'a', 'B', '1', '$').
	- **The "Fancy" special chars:** Takes **2, 3, or 4 bytes**.
- In UTF-8, only the first **127** (ASCII) are 1 byte. Even characters in the "Extended ASCII" range (128-255), like the formatted quotes or the copyright symbol `©`, take **2 bytes** in UTF-8.

*Question for you*: Why does Go do this? Why doesn't `len()` just count characters like in Java or Python? 
*Ans for you*: **Performance.**

In UTF-8, characters are variable width. To count the characters in a 1GB text file, the computer has to scan _every single byte_ from start to finish to see where the characters begin and end. That is an **O(N)** operation (slow).

Counting bytes is an **O(1)** operation (instant) because the string header knows its own byte size immediately. Go prefers the fast operation by default and forces you to be explicit if you want the slow operation.

#### Why Such stupidity ?

- Go uses UTF-8 encoding to represent strings on our computer.
- Remember strings are represented in binary numbers for the computer. 
- One such early way of doing this was to use the ASCII encoding (7bits, 128 chars). But, Like mentioned before, As characters started to increase, we needed more bytes to represent them.
- If you were to use a fixed length encoding like UTF-32 There would be a lot of wasted space for these smaller characters like 'A'. 
- UTF-8 On the other hand uses variable length encoding as in use just as much bytes as that character would require. Which is great for performance because it saves up on a lot of memory.
- This also means that when we make an array, it's actually an array of all the byte representations of each of those characters. So if we were to iterate over all of those characters in the string, we would see that the special characters (eg which take 2 bytes) We'll have the next index missing from the loop 'Cause their binary representation is taking up two bytes. This is also why the `len(str)` Gives back the l number of bytes in the string, And not the number of characters.
- Eg: String "résumé":
![[../assets/resume binary string.png]]

#### 5. How to iterate properly (The `range` magic)

If you iterate a string with a standard `for` loop, you get bytes (which breaks non-English text). If you use the `range` keyword, Go automatically decodes the UTF-8 for you on the fly!
```GO
str := "HiΓ"

// The "Byte" way (Naive)
// Prints bytes: 72, 105, 206, 147 (Garbage at the end)
for i := 0; i < len(str); i++ {
    fmt.Printf("%x ", str[i]) 
}

// The "Rune" way (Smart)
// Prints chars: H, i, Γ (Perfect!)
for index, char := range str {
    fmt.Printf("%c ", char) 
}
```

### Runes

- Instead of handling the underlying binary string on our own, we can cast the string array into a rune array:
```GO
myString := []rune("resume")
```
- Rune represent the chars as unicode numbers so the "résumé" string would look something like this:
![[../assets/resume in rune array.png]]
- Rune are just a alias for int32 ngl.
### Boolean

- super simple, like all others langs:
```GO
var myBool bool = true
fmt.println(myBool)  // true
```

### Arrays

You already know these:
- Fixed length
- Same Type
- Indexable
- Contiguous in Memory
```GO
var intArr [3]int32 // declaration and later assignment
intArr[0] = 1
intArr[1] = 2
intArr[2] = 3
	
fmt.Println(intArr[0])
fmt.Println(intArr[0:2])
fmt.Println(intArr[1:3])
	
intArr2 := [3]int16{1, 2, 3}  // declaration and assignment
fmt.Println(intArr2)
```

### Slice

Slices wrap arrays to give a general, powerful and convenient interface to sequence of data.
If we omit the length value we now have a slice.

```GO
intSlice := []int32{4,5,6}
fmt.Println(intSlice)
intSlice = append(intSlice, 7)
fmt.Println(intSlice)
```

We can change the length of a slice by using the `cap()` (capacity) func. **NOTE** that the length and the capacity of your arrays are 2 different things.

```GO
intSlice := []int32{4, 5, 6}
fmt.Printf("intSlice Legnth: %v | Capacity: %v \n", len(intSlice), cap(intSlice))

intSlice = append(intSlice, 7) // new array alloc
fmt.Printf("intSlice Legnth: %v | Capacity: %v \n", len(intSlice), cap(intSlice))

// For better performance and avoiding frequent reallocation, use the make() func
// int32 slice with len 0 and cap 10
intMakeSlice := make([]int32, 0, 10) 
```

A speed test would relieve that the creation and reallocation of array is a significant performance hit.

### Maps (Dict)

Similar to other programming languages, maps helps store the collections of `key:value` pairs 
Make a map using the follow syntax:
```GO
myMap := make(map[keyType]valueType)
myMap := map[keyType]valueType{key: val, key, val}

// Declare, then assign values later
myMap1 := make(map[string]int)
myMap1["Varun"] = 8
myMap1["Soma"] = 8

// Declare and assign values at same time
myMap2 := map[string]int{"Varun": 8, "Soma": 8, "Tarun": 10}
```

By default, if the key doesn't exist, the map will return the null default of that value type (if values are ints, it will return 0).
Hence, the map can also return 2 variables value and a boolean to know if the key was not found.
Eg:
```GO
rating1, ok := myMap2["Varun"]
if ok {
	println("Varun's rating is:", rating1)
} else {
	println("Varun's age not found in map")
}
```

Delete an map entry by using the Delete() func
```GO
delete(myMap2, key)
```


## Looping

The basic for loops looks like this in Go:
```GO
for i:=0; i<10; i++ {
	fmt.Println(i)
}
```

If we want to iterate over a `array`, `slice`, or a `map`, we can use the `range` keyword.
- Maps:
```GO
for key, value := range myMap2 {
	fmt.Printf("key: %v | value: %v \n", key, value)
}
```
- Arrays / Slices:
```GO
for i, v := range intArr {
	fmt.Printf("Index: %v, value: %v \n", i, v)
}
```

Go **does not have while loop** instead there are a few work around with the for loop itself, to achieve similar functionality:
```GO
i := 0
for i<10 {
	// ... logic
	i += 1
}

i = 0
for {
	if i >= 10 { break }
	i = i + 1
}
```

---
## Printing to console

### fmt Verbs

The universal lazy verb: `%v`. It is smart enough to figure out how to print almost anything.

General & Debugging:

| **Verb** | **Output**                            | **Use Case**                                                                                                  |
| -------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `%v`     | The value in default format           | "Just show me the data."                                                                                      |
| `%+v`    | The value + field names (for Structs) | Debugging objects/structs. Shows `{Name: "John"}` instead of just `{John}`.                                   |
| `%#v`    | **Go Syntax representation**          | **The Ultimate Debugger.** It prints the code required to recreate the data (e.g., `main.User{Name:"John"}`). |
| `%T`     | The Type                              | Checking if you have an `int` or `int64`.                                                                     |
| `%p`     | The Pointer address (`0xc000...`)     | checking if two variables point to the same memory.                                                           |

Type Specific Formatting

| **Verb** | **Output**                | **Example Input -> Output**                                |
| -------- | ------------------------- | ---------------------------------------------------------- |
| `%d`     | Base-10 Integer           | `15` -> `15`                                               |
| `%b`     | Binary (Base-2)           | `15` -> `1111`                                             |
| `%x`     | Hexadecimal (Base-16)     | `255` -> `ff` (Great for looking at raw bytes/hashes)      |
| `%f`     | Float (Default precision) | `10.5` -> `10.500000`                                      |
| `%.2f`   | Float (2 decimal places)  | `10.5` -> `10.50`                                          |
| `%s`     | String                    | `"hello"` -> `hello`                                       |
| `%q`     | Quoted String             | `"hello"` -> `"hello"` (Safe print: escapes special chars) |
| `%c`     | Character (Rune)          | `65` -> `A`                                                |


---

## Testing

we can write the test files in the follow way (naming convention - always end the file name with `_test.go`):
```bash
touch funcNameToCheck_test.go
```
Run the tests for a module using the following cmd:
```bash
go test     # overall PASS/Fails with test name
go test -v  # verbose 
```
