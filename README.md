GoPlus - The Go+ language for data science
========

[![LICENSE](https://img.shields.io/github/license/qiniu/goplus.svg)](https://github.com/qiniu/goplus/blob/master/LICENSE)
[![Build Status](https://travis-ci.org/qiniu/goplus.png?branch=master)](https://travis-ci.org/qiniu/goplus)
[![Go Report Card](https://goreportcard.com/badge/github.com/qiniu/goplus)](https://goreportcard.com/report/github.com/qiniu/goplus)
[![GitHub release](https://img.shields.io/github/v/tag/qiniu/goplus.svg?label=release)](https://github.com/qiniu/goplus/releases)
[![Coverage Status](https://codecov.io/gh/qiniu/goplus/branch/master/graph/badge.svg)](https://codecov.io/gh/qiniu/goplus)
[![GoDoc](https://img.shields.io/badge/Godoc-reference-blue.svg)](https://godoc.org/github.com/qiniu/goplus)

[![Qiniu Logo](http://open.qiniudn.com/logo.png)](http://www.qiniu.com/)

## Summary about Go+

What are mainly impressions about Go+?

- A static typed language.
- Fully compatible with [the Go language](https://github.com/golang/go).
- Script-like style, and more readable code for data science than Go.

For example, the following is a legal Go+ source code:

```go
a := [1, 2, 3.4]
println(a)
```

How do we do this in the Go language?

```go
package main

func main() {
    a := []float64{1, 2, 3.4}
    println(a)
}
```

Of course, we don't only do less-typing things. For example, we  support `list comprehension`, which make data processing easier.

```go
a := [1, 3, 5, 7, 11]
b := [x*x for x <- a, x > 3]
println(b) // output: [25 49 121]

mapData := {"Hi": 1, "Hello": 2, "Go+": 3}
reversedMap := {v: k for k, v <- mapData}
println(reversedMap) // output: map[1:Hi 2:Hello 3:Go+]
```

We will keep Go+ simple. This is why we call it Go+, not Go++.

Less is exponentially more.

It's for Go, and it's also for Go+.


## Compatibility with Go

All Go features (not including `cgo`) will be supported.

* See [supported the Go language features](https://github.com/qiniu/goplus/wiki/Supported-Go-features).

All Go packages (even these packages use `cgo`) can be imported by Go+.

```go
import (
    "fmt"
    "strings"
)

x := strings.NewReplacer("?", "!").Replace("hello, world???")
fmt.Println("x:", x)
```

Be interested in how it works? See [Dive into Go+](https://github.com/qiniu/goplus/wiki/Dive-into-Goplus).

**Also, all Go+ packages can be converted into Go packages, and then be imported by Go.**

First, let's make a directory named `tutorial/14-Using-goplus-in-Go`.

Then write a Go+ package named `foo` in it:

```go
package foo

func ReverseMap(m map[string]int) map[int]string {
    return {v: k for k, v <- m}
}
```

Then use it in a Go package:

```go
package main

import (
	"fmt"

	"github.com/qiniu/goplus/tutorial/14-Using-goplus-in-Go/foo"
)

func main() {
	rmap := foo.ReverseMap(map[string]int{"Hi": 1, "Hello": 2})
	fmt.Println(rmap)
}
```

How to compile this exmaple?

```bash
gop go tutorial/ # Convert all Go+ packages in tutorial/ into Go packages
go install ./...
```

Or:

```bash
gop install ./... # Convert Go+ packages and go install ./...
```

Go [tutorial/14-Using-goplus-in-Go](https://github.com/qiniu/goplus/tree/v6.x/tutorial/14-Using-goplus-in-Go) to get the source code.


## Go+ features

### Map literal

```go
x := {"Hello": 1, "xsw": 3.4} // map[string]float64
y := {"Hello": 1, "xsw": "qlang"} // map[string]interface{}
z := {"Hello": 1, "xsw": 3} // map[string]int
empty := {} // map[string]interface{}
```

### Slice literal

```go
x := [1, 3.4] // []float64
y := [1] // []int
z := [1+2i, "xsw"] // []interface{}
a := [1, 3.4, 3+4i] // []complex128
b := [5+6i] // []complex128
c := ["xsw", 3] // []interface{}
empty := [] // []interface{}
```

### List/Map comprehension

```go
a := [x * x for x <- [1, 3, 5, 7, 11]]
b := [x * x for x <- [1, 3, 5, 7, 11], x > 3]
c := [i + v for i, v <- [1, 3, 5, 7, 11], i%2 == 1]
d := [k + "," + s for k, s <- {"Hello": "xsw", "Hi": "qlang"}]

arr := [1, 2, 3, 4, 5, 6]
e := [[a, b] for a <- arr, a < b for b <- arr, b > 2]

x := {x: i for i, x <- [1, 3, 5, 7, 11]}
y := {x: i for i, x <- [1, 3, 5, 7, 11], i%2 == 1}
z := {v: k for k, v <- {1: "Hello", 3: "Hi", 5: "xsw", 7: "qlang"}, k > 3}
```

### For loop

```go
sum := 0
for x <- [1, 3, 5, 7, 11, 13, 17], x > 3 {
    sum += x
}
```

### Go features

All Go features (not including `cgo`) will be supported.

* See [supported the Go language features](https://github.com/qiniu/goplus/wiki/Supported-Go-features).


## Tutorials

* https://github.com/qiniu/goplus/tree/v6.x/tutorial
