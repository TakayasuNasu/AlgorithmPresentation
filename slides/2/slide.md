---
marp: true
paginate: true
theme: gaia-ex
---


Algorithm Presentation: Slide 2.
---

---

#### Agenda


---

### Find the substring pattern of length <br> M in a text of length N.

<br><br>

## Boyer-Moore algorithm

---

### What is Boyer-Moore algorithm?

The Boyer-Moore algorithm is considered as the most efficient string-matching algorithm in usual applications. A simplified version of it or the entire algorithm is often implemented in text editors for the «search» and «substitute» commands.

[Reference: https://www-igm.univ-mlv.fr/~lecroq/string/node14.html#SECTION00140](https://www-igm.univ-mlv.fr/~lecroq/string/node14.html#SECTION00140)

---

### Make Skip Diagram

Search pattern：abca

|pattern|a|b|c|a|
|:-:|:-:|:-:|:-:|:-:|
|skip|3|2|1|0|

If there are duplicate strings, the smaller one will be given priority.

|pattern|b|c|a|default|
|:-:|:-:|:-:|:-:|:-:|
|skip|3|2|1|0|

---

### Ecample

#### 1st

|target|a|b|c|d|e|f|g|i|g|h|i|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|pattern|i|g|h|i||||||||

The search starts at the beginning and compares the search target with the end of the pattern.
Since the string to be searched and the end of the pattern to be searched are "d" and "i", we will skip the default of four.

---

### Conclusion