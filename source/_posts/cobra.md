---
title: create cli by cobra
date: 2019-12-30 01:37:20
tags:
- cobra
- cli
- go
categories:
- Golang
cover: ../../images/articles/cobra.png
---

![Alt text](../../images/cobra.png)


### install
cd to ../golang.org/x
``` bash
git clone https://github.com/golang/text
git clone https://github.com/golang/sys
```
download github.com/spf13/cobra/cobra
cd to its dir
then open cmd & type 
``` bash
go install github.com/spf13/cobra/cobra
```
then use it:
``` bash 
cobra init mycalc
```
Cobra-based application structure:

``` bash
▾ appName/
    ▾ cmd/
        root.go
    main.go
``` 
cobra main.go structure:
``` bash
package main

import (
  "mycalc/cmd"
)

func main() {
  cmd.Execute()
}
```
when you run it, it will reports error:
``` bash
G:\go-work\bin\mycalc>go run main.go
main.go:18:8: cannot find package "go-work/bin/my-calc/cmd" in any of:
        c:\go\src\go-work\bin\my-calc\cmd (from $GOROOT)
        G:\go-work\src\go-work\bin\my-calc\cmd (from $GOPATH)
```
⚠️so you copy the whole package to $GOPATH, then 
``` bash 
G:\go-work\bin\mycalc>go install mycalc
G:\go-work\bin\mycalc>go run main.go
Hello CLI

G:\go-work\bin>mycalc
Hello CLI
```
the command in root.go is called rootCmd.

### the cli flow
1. edit root.go
``` bash
func init() {
     fmt.Println("inside init")
     cobra.OnInitialize(initConfig)
...
```
2. edit main.go
``` bash
func main() {
     fmt.Println("inside main")
     cmd.Execute()
}
```
and it will shows this:
``` bash
G:\go-work\bin>mycalc
inside init
inside main
inside initConfig
inside initConfig
Hello CLI
```
Should notice two types of flags: Persistent flags & Local flags

### add cli:
1. add num
create a command named add, navigate to the project 
``` bash 
G:\go-work\src\mycalc>cobra add add
add created at G:\go-work\src\mycalc
```
rebuild the binary using go install mycalc and run mycalc add.
``` bash
G:\go-work\src\mycalc>go install mycalc
```

``` bash
G:\go-work\bin>mycalc add
add called
```
2. modify it to add a series of numbers
to add numbers, fisrt have to convert the string into int then returan the result.
``` bash
import (
    ...
    "strconv"
)
```
3. inside the add.go, create a new addInt function.
``` bash
func addInt(args []string) {
    var sum int
    // the first return value is index of args, we can omit it using _ 
    for _, ival := range args {
    // strconv is the library used for type conversion, for string
    // to int conversion Atio method is udes.
        itemp, err := strconv.Atoi(ival)
        if err != nil {
            fmt.Println(err)
        }
        sum = sum + itemp
    }
    fmt.Printf("Addition of numbers %s is %d, args, sum)
}
```
4. update RUN func
``` bash
...
addInt(args)
...
```
5. rebuild
``` bash
G:\go-work\bin>mycalc add a bc 8
strconv.Atoi: parsing "a": invalid syntax
strconv.Atoi: parsing "bc": invalid syntax
Addition of numbers [a bc 8] is 8
```
### achieve float calc by using flag
1. create a local flag in init

``` bash
//add.go
func init() {
    rootCmd.AddCommand(addCmd)
    addCmd.Flags().BoolP("float", "f",false,"Add Floating Numbers")
}
```
2. create a addFloat func in add.go
//add.go
func addFloat(args []string) {
    var sum float64
    for _, fval := range args {
        ftemp, err := strconv.ParseFloat(fval,64)
        if err != nil {
            fmt.Println(err)
        }
        sum = sum + ftemp
    }
    fmt.Printf("sum of floating nums %s is %f", args,sum)
}
```
3. create a addFloat func in addCmd RUN func
``` bash
// add.go
// addCmd
Run: func(cmd *cobra.Command, args []string) {
    // get the flag value, its default value is false
    fstatus, _ := cmd.Flags().GetBool("float")
    if fstatus {
        addFloat(args)
    } else {
        addInt(args)
    }
},
```
### select even numbers & add them
``` bash
 cobra add even
```
1. init in even.go
``` bash
//even.go
func init() {
    addCmd.AddCommand(evenCmd)
...
}
```
2. update evenCmd struct variable's RUN method
``` bash
//even.go
Run: func(cmd *bobra.Command, args []string) {

    var evenSum int
    for _, ival := range args {
        itemp, _ := strconv.Atoi(ival)
        if itemp%2 == 0 {
            evenSum = evenSum + itemp
        }
    }
    fmt.Printf("the even addition of %s is %d", args, evenSum)
},
```
### select old numbers & add them
``` bash
//odd.go
...
if itemp%2 != 0 
...
```
