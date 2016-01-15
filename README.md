# bitmap
Yes! It's Go/Golang's bitmap(bitset) function!

This function is achieved by the official package: "math/big" (big.Int).

It's high efficiency and easy to use.


## Install
	go get github.com/plhwin/bitmap



## Example
	package main

	import (
		"fmt"
		"github.com/plhwin/bitmap"
	)

	func main() {
		x := bitmap.New().Set(0).Set(1).Set(3).Set(5).Set(7).Set(9)
		y := bitmap.New().Set(2).Set(4).Set(5).Set(6).Set(8)
		z := bitmap.New() // z is a new empty bitmap

		// remove offset 5 from x, now x's offset is [0 1 3 7 9], bitmap is 1010001011
		x.Clear(5)

		// set new offset to y agin, now y's offset is [2 4 5 6 8 9], bitmap is 1101110100
		y.Set(9)

		//find out the same bitset from x and y
		and := x.And(y)
		// remove the same bitset from x and y
		or := x.Or(y)
		// find out the different bitset from (x and y)+(y and x)
		xor := x.Xor(y)
		// find out the different bitset from (x and y)
		andnot := x.AndNot(y)

		fmt.Println("x:", x, x.GetAllSetBits(true)) // if false, it will be return a random slice
		fmt.Println("y:", y, y.GetAllSetBits(true))
		fmt.Println("z:", z, z.GetAllSetBits(true), z.BitLen(), z.IsEmpty()) // this bitmap's len is 0, and z.IsEmpty() is true
		fmt.Println("and:", and, and.GetAllSetBits(true))
		fmt.Println("or:", or, or.GetAllSetBits(true))
		fmt.Println("xor:", xor, xor.GetAllSetBits(true))
		fmt.Println("andnot:", andnot, andnot.GetAllSetBits(true))
		
		// here if you want to check a offset in x, you can do it like this:
		fmt.Println("if offset 0 in x?", x.Test(0)) //true
		fmt.Println("if offset 5 in x?", x.Test(5)) //false, Because we run the code 'x.Clear(5)' above.
	}

all the output is:

	x: 1010001011 [0 1 3 7 9]
	y: 1101110100 [2 4 5 6 8 9]
	z: 0 [] 0 true
	and: 1000000000 [9]
	or: 1111111111 [0 1 2 3 4 5 6 7 8 9]
	xor: 111111111 [0 1 2 3 4 5 6 7 8]
	andnot: 10001011 [0 1 3 7]
	if offset 0 in x? true
	if offset 5 in x? false


##Tips:

let's see a new bitmap:

	a := bitmap.New().Set(0).Set(1).Set(3).Set(5).Set(7).Set(9)
	fmt.Println("a:", a, a.GetAllSetBits(true))


it will be out put:

	1000100000 [5 9]

please from right to left to count the offset in the bitmap,so the offset slice is [5 9]
please from right to left, so you can find the corresponding relation between the offset slice `[5 9]` and bitmap `1000100000`,but this is not important, I told you to avoid your strange.


##Others
based on official package "math/big" (big.Int) implementation,the length of the bitmap that can be stored depends on the size of your memory.

when the bitmap's length exceeds 10 billion, the performance is beginning to decline,so it can be applied to more than 99% of the business scene
