---
marp: true
paginate: true
theme: gaia-ex
---

<!-- _class: top -->

Algorithm Presentation
===

- Chihori Suzuki
- Takayasu Nasu

---

#### Agenda

- Find the smallest/largest M elements in a stream of N items.
- Find the substring pattern of length <br> M in a text of length N.

---

<!-- _class: headline -->

## Find the smallest/largest M elements in a stream of N items.

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
    i:3     [6, 10, 61]       -> 61     <- 24
    i:4     [6, 10, 24]       -> 24     <- 9
    i:5     [6, 9, 10]                  <- 1625   -- ignore
    i:6     [6, 9, 10]        -> 10     <- 2
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

 [1,        3,        5,        7,        9]     <- 6   // input < middle 
                                ^         ^         
                         low(middle+1)   high  
                         new middle   

 [1,        3,        5,        7,        9]     <- 6  
                      ^         ^         
              high(middle-1)  low,middle     

return middle <--- position to insert

```

---

### Conclusion

1. Loop number of items  -----> $\Omicron\lparen N \rparen$

2. Find the position  -----> $\Omicron\lparen logM \rparen$

    Total Time Complexity ====>  $\Omicron\lparen NlogM \rparen$


---

Next

---

<!-- _class: headline -->

## Find the substring pattern of length M in a text of length N.

<br><br>

### Boyer-Moore algorithm

---

### What is Boyer-Moore algorithm?

The Boyer-Moore algorithm is considered as the most efficient string-matching algorithm in usual applications. A simplified version of it or the entire algorithm is often implemented in text editors for the «search» and «substitute» commands.

[Reference: https://www-igm.univ-mlv.fr/~lecroq/string/node14.html#SECTION00140](https://www-igm.univ-mlv.fr/~lecroq/string/node14.html#SECTION00140)

---

### Make Skip Table

Search pattern：abca

|pattern|a|b|c|a|
|:-:|:-:|:-:|:-:|:-:|
|skip|3|2|1|0|

If there are duplicate strings, the smaller one will be given priority.

|pattern|b|c|a|default|
|:-:|:-:|:-:|:-:|:-:|
|skip|3|2|1|4|

---

### Sample Code to make Skip Table

```swift
func createTable(_ pattern: String) -> Dictionary<Character, Int> {

  var table = Dictionary<Character, Int>()

  // If there is a duplicate string, it prioritizes the smaller ones.
  for i in 0..<pattern.count {
    table[Character(pattern[i])] = pattern.count - i - 1
  }

  // e.g. pattern => Today
  // table => ["T": 4, "o": 3, "d": 2, "a": 1, "y": 0]
  return table
}
```

---

### Example

#### 1st

|target|a|b|c|d|e|f|g|i|g|h|i|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|pattern|i|g|h|i||||||||

This algorithm starts the search from the beginning, then it compares the target edge and pattern edge.
As a result, Skip "4" targets.Because it ends in "d" and "i".

---

#### 2nd

|target|a|b|c|d|e|i|g|i|g|h|i|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|pattern|||||i|g|h|i|||

The algorithm compares the before letters, because the target and pattern are both "i".
Then target letter is "g", pattern letter is "h". When target is "g", that skip "1" according to Skip Table.
Skip "1".

---

#### 3rd

|target|a|b|c|d|e|i|g|i|g|h|i|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|pattern||||||i|g|h|i||

When target is "g", the pattern is "i", then skip "2" according to Skip Table.

---

#### 4th

|target|a|b|c|d|e|i|g|i|g|h|i|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|pattern||||||||i|g|h|i|

The algorithm compares the before letters, because the target and pattern are both "i".
It compares the before letters again and again.

Every target letter and pattern letters the same. That's why the algorithm finish!

---

#### Pseudocode

```swift
func originalSubstring(_ text: String, _ pattern: String) {

  let table = createTable(pattern)

  var i = pattern.count - 1
  var j = pattern.count - 1

  while i < text.count {
    if text[i] != pattern[j] {
      if let number = table[Character(text[i])] {
        i += number
      } else {
        i += pattern.count - 1
      }
      j = pattern.count - 1
    } else {
      if j == 1 {
        print(text[i - 1, i + pattern.count])
        break
      }
      i -= 1
      j -= 1
    }
  }
}
```

---

### Conclusion

1. Best performance  -----> $\Omicron\lparen\frac n m\rparen$

2. Only need to find one match  -----> $\Omicron\lparen n + m\rparen$

3. Find much string many times case -----> $\Omicron\lparen nm\rparen$