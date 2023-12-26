# Section1

## Named return values structured like so:
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

Note: only use this for short functions because it can cause readability issues with longer ones

## Short variable declarations

usually variables need to be declared like so: var i, j int = 1, 2 (with or without the type when an initializer is present)

however, inside of functions, you can use a short assignment := with an initializer (implied type)

func main() {
	var i, j = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}

## Go's basic types:

- bool
- string
- int, int8, int16, int32, int64
- uint, unint8, uint32, uint64 uintptr

- byte (alias for uint8)
- rune (alias for int32 represents a unicode code point)
- float32 float64
-complex64 complex128

## Zero values
Variables declared without an explicit initial value are given their zero value:
- 0 for numeric types
- false for boolean types
- "" (an empty string) for string types

## Type conversion

type conversions must be explicit
The expression T(v) converts the value v to type T:
func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z uint = uint(f)
	fmt.Println(x, y, z)
}

## const

- constants are declared like variables bu with the const key word
- cannot use the := shorthand
- numberic constants are high-precesion value
	- an untyped ocnstant takes the type needed by its context:

const (
// Create a huge number by shifting a 1 bit left 100 places.
// In other words, the binary number that is 1 followed by 100 zeroes.
Big = 1 << 100
// Shift it right again 99 places, so we end up with 1<<1, or 2.
Small = Big >> 99
)

func needInt(x int) int { return x\*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}

# Logic and loops

## Fo

Go only has one looping construct, the for loop which has three components separated by a ";":
1. the init statement: exercuted before the first iteration
	- variables defined here are only in the scope of the for statement
2. the condition expression: evaluated before every iteration
3. the post statement: executed at the end of every iteration

The loop will stop iterating once the boolean evaluates to false

example:
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

## for is the new while

for acts as a while loop because the init and post statements are optional:
func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}

when doing the above, you can drop the semicolons:
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

## infinit lops for[ever]

drop all the components of the for loop to obtain an infinit loop:

func main(){
	for{
	}
}


## If statements

###The conditional for if statements does not need a parenthesies, but the task needs a {}

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

### Some if statemenst hagve short variable declarations

These variables, like "v" below, exist only in the scope of the statment
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}

Those variables are available within the following else statements as the scope is extended

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

## Switch statements

A shorter way to write if - else statements
Go's switch case only runs the selected case, and does not continue through all of the following cases. the "break statement is automatic in go

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}
}

- a switch without a condition is the same as `switch true`
func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}

## Defer
- defers the execution of a function until the surronding functions returns
- sequential defers are pushed onto the call stack and executed in LIFO order

# Section 3

Go has pointers. A pointer holds the memory address of a value.

The type *T is a pointer to a T value. Its zero value is nil.

var p *int
The & operator generates a pointer to its operand.

i := 42
p = &i
The * operator denotes the pointer's underlying value.

fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
This is known as "dereferencing" or "indirecting".

Unlike C, Go has no pointer arithmetic.
func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}

## Structs

- a collection of fields
- use dots to access the fields
- can be accessed through struct pointer
	- do not need to writed (\*p).X
	- can just writte p.X for ease of use
type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}


## Struct Literals

- use the `Name:` syntax to denote newly allocated struct value

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}

##Arrays

The type [n]T is an array of n values of type T.

The expression

var a [10]int
declares a variable a as an array of ten integers.

An array's length is part of its type, so arrays cannot be resized.

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}

## Slices

An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. In practice, slices are much more common than arrays.

The type []T is a slice with elements of type T.

A slice is formed by specifying two indices, a low and high bound, separated by a colon:

a[low : high]
This selects a half-open range which includes the first element, but excludes the last one.

The following expression creates a slice which includes elements 1 through 3 of a:

a[1:4]

A slice does not store any data, it just describes a section of an underlying array.

Changing the elements of a slice modifies the corresponding elements of its underlying array.

Other slices that share the same underlying array will see those changes.

## Slice literals
A slice literal is like an array literal without the length.

This is an array literal:

[3]bool{true, true, false}
And this creates the same array as above, then builds a slice that references it:

[]bool{true, true, false}
`
func main() {
	q := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(q)

	r := []bool{true, false, true, true, false, true}
	fmt.Println(r)

	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(s)
}
`

## Slice length and capacity
A slice has both a length and a capacity.

The length of a slice is the number of elements it contains.

The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.

The length and capacity of a slice s can be obtained using the expressions len(s) and cap(s).

You can extend a slice's length by re-slicing it, provided it has sufficient capacity.


# 13/27 creating a slice with make

Slices can be created with the built-in make function; this is how you create dynamically-sized arrays.

The make function allocates a zeroed array and returns a slice that refers to that array:

a := make([]int, 5)  // len(a)=5
To specify a capacity, pass a third argument to make:

b := make([]int, 0, 5) // len(b)=0, cap(b)=5

#Appending to a slice

It is common to append new elements to a slice, and so Go provides a built-in append function. The documentation of the built-in package describes append.

`func append(s []T, vs ...T) []T`

The first parameter s of append is a slice of type T, and the rest are T values to append to the slice.

The resulting value of append is a slice containing all the elements of the original slice plus the provided values.

If the backing array of s is too small to fit all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

# Range

The range form of the for loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a *copy* of the element at that index.


You can skip the index or value by assigning to \_.

for i, _ := range pow
for \_, value := range pow
If you only want the index, you can omit the second variable.

for i := range pow

























