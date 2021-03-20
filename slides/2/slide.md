---
marp: true
paginate: true
theme: gaia-ex
---


Algorithm Presentation: Slide 2.
---

---

#### Agenda

- Find the substring pattern of length <br> M in a text of length N.

---

### Find the substring pattern of length <br> M in a text of length N.

<br><br>

## Boyer-Moore algorithm

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
As a result, Skip "4" targets.Because it ends in a "d" and an "i".

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

## Time complexity

##### Best performance.

$\Omicron\lparen\frac n m\rparen$

<br>

##### Only need to find one match

$\Omicron\lparen n + m\rparen$

<br>

##### Find much string many times case

$\Omicron\lparen nm\rparen$

---

### Conclusion