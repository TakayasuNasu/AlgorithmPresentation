---
marp: true
paginate: true
theme: gaia-ex
---


Algorithm Presentation: Slide 1.
---


---
#### Agenda

- Find the smallest/largest M elements in a stream of N items.



---
### Example:  find 3 elements in 8 items
input:

    [6, 61, 10, 24, 9, 1625, 2, 30]
output (smallest)

    [2, 6, 9]
output (largest)

    [1625, 61, 30]

---
###  find the SMALLEST elements

    // Loop i: 0 to 8 (nomber of items)

            output-array                input
    i:0     []                          <- 6
    i:1     [6]                         <- 61
    i:2     [6, 61]                     <- 10
<<<<<<< HEAD
    i:3     [6, 10, 61]       -> 61     <- 24
    i:4     [6, 10, 24]       -> 24     <- 9
    i:5     [6, 9, 10]                  <- 1625   -- ignore
    i:6     [2, 6, 10]         -> 10     <- 2
=======
    i:3     [6, 10, 61]       -> 61     <- 24
    i:4     [6, 10, 24]       -> 24     <- 9
    i:5     [6, 9, 10]                  <- 1625   -- ignore
    i:6     [2, 6, 9]         -> 10     <- 2
>>>>>>> main
    i:7     [2, 6, 9]                   <- 30     -- ignore

How do I do that??

---
### Create the output items
```swift
Loop (i => 0 to 8 )

    if output-array is Empty
        outpust array [] <- 6

    if output-array.count is '3' && array[last index] < input
        Nothings to do! continue
        ex) array: [6, 9, 10]                  <- 1625
            // array.count = 3  &&  10 < 1625

    if output-array.count is '3'
        remove last element from output-array  // [6, 10, 24]  -> 24   <- input:9
        insert the input for output-array  at RIGHT INDEX // [6, 9, 10]
```

---
### Find the position to insert
```swift
while low <= high
 [1,        3,        5,        7,        9]     <- 6   // middle < input
  ^                   ^                   ^         ^
 low               middle                high     input

 [1,        3,        5,        7,        9]     <- 6   // input > middle
                                ^         ^
                         low(middle+1)   high
                         new middle

 [1,        3,        5,        7,        9]     <- 6  // input < middle
                      ^         ^
                    high    low, middle
** if middle = hight
        if middle < input  ----> return middle + 1
        else if input < middle ---> return middle  (index: 3)
```
---
### Conclusion
1. Loop number of items : $\Omicron\lparen N \rparen$



2. Find the position  :  $\Omicron\lparen logM \rparen$

    Total Time Complexity ====>  $\Omicron\lparen NlogM \rparen$

