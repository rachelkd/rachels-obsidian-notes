---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 4
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "7"
Chapter:
Slides/Notes:
  - "[[07-Java Swing.pdf]]"
Date: 2025-01-28
Date created: Tue., Jan. 28, 2025, 4:06:31 am
Date modified: Tue., Feb. 4, 2025, 4:52:01 am
---

# Strings

## Introduction

> [!def]+ C String Definition
>
> - A ==character array== ending with ==null terminator `\0`==
>     - Not a new data type — just a `char` array with special termination
> - Null character marks ==end of meaningful content==

> [!info]+ Strings solve two problems with raw char arrays:
>
> 1. No need for manual character-by-character processing
> 2. Prevents printing garbage values from uninitialized array elements

```c
#include <stdio.h>

int main(void) {
    char text[20];
    text[0] = 'h';
    text[1] = 'e';
    text[2] = 'l';
    text[3] = 'l';
    text[4] = 'o';
    text[5] = '\0';

    // We can print each character in the array one at a time.
    // But we don't know what junk is in the array after the null character.
    int i;
    for (i = 0; i < 20; i++) {
        printf("%c", text[i]);
    }
    printf("\n");

    // we can use %s as a format specifier inside printf to print strings
    printf("%s\n", text);
    return 0;
}
```

## Initializing Strings and String Literals

```c
#include <stdio.h>

int main() {
    char text[20] = {'h', 'e', 'l', 'l', 'o', '\0'};

    // This does the same thing
    char text[20] = "hello";

    printf("%s\n", text);

    // But text1 is a pointer to a string literal
    char *text1 = "hello";
    printf("%s\n", text1);


    // We can change a character in text but not text1
    text[0] = 'j';
    printf("%s\n", text);
    // See what happens if we uncomment the next line and compile
    // text1[3] = 'X';

    return 0;
}
```

> [!info]+ Three Initialization Approaches
>
> 1. **Explicit array initialization**
>
>    ```c
>    char text[20] = {'h','e','l','l','o','\0'};
>    // Remaining elements automatically set to \0
>    ```
>
> 2. **String literal shorthand**
>
>    ```c
>    char text[20] = "hello"; // Compiler adds null terminator to remaining 15 elements
>    ```
>
> 3. **Omitted array size**
>
>    ```c
>    char text[] = "hello"; // Compiler allocates 6 elements (5 chars + \0)
>    ```

- Omitting the array size does not mean that the array size can change

> [!warning]+ It is illegal for the initializer to be longer than the size of the `char` array, but it *is* legal for the array and initializer to be the same size.
>
> - Initializer length must be `≤ array_size - 1` (leave space for `\0`)
> - Identical array/initializer sizes create ==unterminated strings==
>
>    ```c
>    char bad[5] = "hello"; // No room for \0 → Not a valid string
>    ```

> [!def]+ String literal
>
> - String constant that cannot be changed

> [!danger]+ Key Distinction Between String Variables and Literals
>
> ```c
> char text[] = "hello";  // Modifiable string variable
> char *text1 = "hello";  // Pointer to immutable string literal
> ```
>
> - Can modify array elements:
> `text[0] = 'j'; // Valid`
> - Attempting literal modification:
> `text1[0] = 'j'; // Undefined behavior`

- `text1` points to the first letter of the string literal
- ? What happens if we run `text1[0] = 'j';`?
    - Undefined behaviour at runtime
    - Perfectly okay to change a string variable
    - Not a string constant!

### Lecture

```c
int main() {
    char str1[] = {'h', 'e', 'l', 'l', 'o', '\0'};
    char str2[] = "hello";

    str1[0] = 'j';  // Legal
    str2[0] = 'j';  // Also legal

    // Where is memory allocated?

    char greet1[10] = "hello";

    char *greet2 = malloc(sizeof(char) * 10);
    strncpy(greet2, greet1, 10);

    const char *greet3 = "hello";  // greet3 points to "hello" in read-only memory
}
```

- Initialization for `str1` and `str2` are exactly the same
- String literals are in ==read-only== memory
- `const` tells the compiler to never modify `*greet3`

| Variable | Variable Location | Data Location    | Modifiable?            | Notes                                                                                                                                                           |
| -------- | ----------------- | ---------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `str1`   | Stack             | Stack            | Yes (entire array)     | Explicitly initialized as a stack array.                                                                                                                        |
| `str2`   | Stack             | Stack            | Yes (entire array)     | Initialized with a string literal *copied* to the stack.                                                                                                        |
| `greet1` | Stack             | Stack            | Yes (entire array)     | Fixed-size stack array (`char[10]`). Unused elements are zero-initialized.                                                                                      |
| `greet2` | Stack             | Heap             | Yes (heap data)        | Pointer (`char*`) on the stack, points to heap memory allocated via `malloc`. Must be `free`d to avoid leaks.                                                   |
| `greet3` | Stack             | Read-only memory | No (data is immutable) | Pointer (`const char*`) on the stack, points to a string literal in read-only memory. Modifying the data causes undefined behaviour (e.g., segmentation fault). |

## Size and Length

```c
#include <stdio.h>
#include <string.h>

int main() {
    char weekday[10] = "Monday";
    printf("Size of string: %lu\n", sizeof(weekday));
    printf("Length of string: %lu\n", strlen(weekday));
    return 0;
}
```

- Size of `weekday` is 10
- & `sizeof` gives the number of bytes occupied by array
    - Compile-time operation
    - Does not look at contents of array
    - Not a way to determine the number of characters in a string

```c
char s1[10] = "Mon";
char s2[10] = "day";

char s3[20];
s3 = s1 + s2
```

- `s1 + s2`:
    - Adds two pointers together
    - Has nothing to do with string concatenation

<!-- break -->
- C standard library contains many functions for working with strings
    - e.g., Determine length of string or concatenate two strings
    - & Include the string header file in our program
        - `#include <string.h>`
        - Header file contains function prototypes of all of the C string functions

> [!thm]+ `size_t strlen(const char *s)`
>
> - Returns number of characters in string `s`
>     - Not including null termination character
> - `size_t` is an *unsigned* integer type
>     - Can treat like integer

## Copying Strings

```c
#include <stdio.h>
#include <string.h>

int main() {
    char s1[5];
    char s2[32] = "University of";

    // This is unsafe because s1 may not have enough space
    // to hold all the characters copied from s2.

    //strcpy(s1, s2);

    // This doesn't necessarily null-terminate s1 if there isn't space.
    strncpy(s1, s2, sizeof(s1));
    // So we explicitly terminate s1 by setting a null-terminator.
    s1[4] = '\0';

    printf("%s\n", s1);
    printf("%s\n", s2);
    return 0;
}
```

- **Copying** a string
    - Overwrite what was previously there
- **Concatenation**
    - Add one string to the end of what was previously there by the other

> [!thm]+ `char *strcpy(char *s1, const char *s2)`
>
> - Copies the characters from string `s2` into the beginning of array `s1`
> - Overwrites what was at the start of `s1`
>     - → `s1` is not required to be a string when `strcpy` is called
> - `s2` is required to be a string when called
>     - i.e., `s2` is a string literal or char array that includes a null terminator

- ! `strcpy` is known as an unsafe function
    - `s2` is a string of 14 characters + null terminator
    - Does not fit in `s1` of size 5
    - Case is undefined
        - i.e., Different compilers handle this differently
- Safe versions of unsafe functions often add an `n` somewhere in there name
    - For new parameter `n`
    - Control how much activity the function is allowed to do before stopping

`strcpy` has a safe counterpart: `strncpy`

> [!thm]+ `char *strncpy(char *s1, const char *s2, int n);`
>
> - `n`
>     - Indicates the maximum number of characters that `s1` can hold
>     - $\therefore$ Max number of characters *including* null terminator that can be copied from `s2` into `s1`

- `strncpy` is also ==unsafe==
    - Not guaranteed to add a null terminator
    - Unless it finds one in the first `n` characters in `s2`

> [!tip]+ How do we safely use `strncpy`?
>
> - Ensure string ends with null character
>     - Explicitly add null character

### Lecture

```c
CAPACITY = 7;

int main() {
    // How do we do this safely?
    strcpy(dest, source);

    // Option 1: Source could be longer than dest
    strncpy(dest, source, strlen(source));

    // Option 2: dest might not have a null termination character in it
    strncpy(dest, source, strlen(dest));

    // Option 3: source might have more space allocated for it than dest
    strncpy(dest, source, sizeof(source));

    // Option 4: doesn't leave space for the null terminator; dest might not be null terminated, but we will not overwrite the end
    strncpy(dest, source, sizeof(dest));

    // Option 5: Same as 4
    strncpy(dest, source, CAPACITY);

    // Option 6: Leaves space for a null termination character, but it does not put it there
    strncpy(dest, source, CAPACITY - 1);

    // Option 7: Correct
    strncpy(dest, source, CAPACITY - 1);
    dest[CAPACITY - 1] = '\0';

    // Option 8: CAPACITY is not an index in the array
    strncpy(dest, source, CAPACITY - 1);
    dest[CAPACITY] = '\0';
}
```

- `strlen` counts until the null termination character
    - If it does not see the null termination character, it will keep going until it finds one
        - Always going to find one
    - Option 2:
        - `strlen(dest)` can be bigger than what we allocate for `dest`

## String Concatenation

- Also known as **appending** to a string

> [!thm]+ `char *strcat(char *s1, const char *s2)`
>
> - Adds characters from `s2` to end of string `s1`
> - Both `s1`, `s2` must be strings prior to call to `strcat`

> [!info]+ `strcat` vs. `strncat`
> **`strcat(s1, s2)`**
>
> - ==Appends entire `s2` to `s1`==
> - Dangerous: No buffer overflow checks
>
> **`strncat(s1, s2, n)`**
>
> - Appends ==max `n` characters== from `s2`
> - Always adds ==null terminator==
> - Safer but requires manual size calculation

```c
// Example Safe Concatenation
char s1[30];
strcpy(s1, "University of");
strncat(s1, " C Programming", sizeof(s1) - strlen(s1) - 1); // Calculate remaining space
```

## Searching with Strings

### Searching for Single Character

> [!thm]+ `char *strchr(const char *s, int c)`
>
> - `s` is the string to search
> - `c` character to search for in the string
> - Searches from left to right
> - Returns a ==pointer== to the character that was found
>     - Null, if character was not found

```c
#include <stdio.h>
#include <string.h>

int main() {
    char s1[30] = "University of C Programming";
    char *p;

    // find the index of the first 'v'
    p = strchr(s1, 'v');
    if (p == NULL) {
        printf("Character not found\n");
    } else {
        printf("Character found at index %ld\n", p - s1);
    }

    // find the first token (up to the first space)
    p = strchr(s1, ' ');
    if (p != NULL) {
        *p = '\0';
    }
    printf("%s\n", s1);

    return 0;
}
```

- Use **pointer arithmetic** to determine index of “v” in `s1`
    - `s1` and `p` are pointers to the same array
    - Pointer subtraction is well defined

### Searching for Substring

> [!thm]+ `char *strstr(const char *s1, const char *s2)`
>
> - Searches left to right in `s1` for first occurrence of substring `s2`
> - Returns pointer to the character of `s1` that *begins* the match to `s2`
>     - Null if substring `s2` not in `s1`

```c
#include <stdio.h>
#include <string.h>

int main() {
    char s1[30] = "University of C Programming";
    char *p;

    p = strstr(s1, "sity");

    if (p == NULL) {
        printf("Character not found\n");
    } else {
        printf("Character found at index %ld\n", p - s1);
    }

    return 0;
}
```
