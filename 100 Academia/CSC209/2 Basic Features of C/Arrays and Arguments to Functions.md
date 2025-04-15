---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 2
Module:
  - "[[2 - Basic Features of C]]"
Lecture:
  - "4"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/UrgkAdwnMP3NoLQt8QOWfb
Date: 2025-01-16
Date created: Thu., Jan. 16, 2025, 3:17:43 pm
Date modified: Mon., Feb. 3, 2025, 4:56:23 pm
---

# Arrays and Arguments to Functions

## Function Calls and Pointers

Consider the following (incorrect) implementation for a function that increases age by one:

```c
#include <stdio.h>

void lie(int age) {
    printf("You are %d years old\n", age);
    age += 1;
    printf("You are %d years old\n", age);
}

int main() {
    int age = 18;
    lie(age);
    printf("But your age is still %d\n", age);
    return 0;
}
```
