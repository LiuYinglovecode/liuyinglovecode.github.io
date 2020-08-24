---
title: regexp
date: 2019-12-11 02:31:48
tags:
- rege
- go
- verification
categories:
- go
---

## Go inputs verification

### 1. Numbers
```
getint, err := strconv.Atoi(r.Form.Get("num"))
if err != nil {
  // error occurs when convert to number, it may not a Number
}

// check range of number
if getint > 100 {
  // too big
}

or

if m, _ := regexp.MatchString("^[0-9]+$", r.Form.Get("num")); !m {
  return flase
}
```

### 2. Chinese

```
if m, _ := rangexp.MatchString("^[\\x{4e00}-\\x{9fa5}]+$", r.Form.Get("Chinesecharactor")); !m {
  return false
}
```
### 3. Letters
```
if m, _ := rangexp.MatchString("^[a-zA-Z]+$", r.Form.Get("englishletter")); !m {
  return flase
}
```
### 4. E-mail
```
if m, _ := regexp.MatchString(`^([\w\.\_]{2,10})`,r.Form.Get("mail")); !m {
  fmt.Println("no")
}else {
  fmt.Println("yes")
}
```
### 5. Drop down List
```
<select name="fruit">
<option value="apple">apple</option>
<option value="pear">pear</option>
<option value="banana">banana</option>
</select>
```
and use the following strategy to sanitize our input:

```
slice := []string{"apple", "pear", "banana"}

for _, v := range slice {
  if v == r.Form.Get("fruit") {
    return true
  }
}
return false
```

### 6. Radio buttons
```
<input type = "radio" name="gender" value="1">Male
<input type = "radio" name="gender" value="2">Female
```
use the following code to validate the input:

```
slice := []int{1,2}

for _, v := range slice {
  if v == r.Form.Get("gender") {
    return true
  }
}
return false
```
### 7. Check boxes

```
<input type="checkbox" name = "interest" value = "football">football
<input type="checkbox" name="interest" value="basketball">Basketball
<input type="checkbox" name="interest" value="tennis">Tennis
```
In this case, the sanitization is a little bit different to validating the button and check box inputs since here we get a slice from the check boxes.

```
slice :=[]string{"football","basketball","tennis"}
a := Slice_diff(r.Form["interest"],slice)
if a == nil {
  return true
}

return false
```

### 8. Date and time

Suppose you want users to input valid dates or times. Go has the time package for converting year, month and day to their corresponding times. After that, it's easy to check it.

``` bash
t := time.Date(2019, time.December, 10, 23, 0, 0, 0, time.UTC)
fmt.Printf("Go launched at %s\n", t.Local())
```
After you have the time, you can use the time package for more operations, depending on needs.
