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





























