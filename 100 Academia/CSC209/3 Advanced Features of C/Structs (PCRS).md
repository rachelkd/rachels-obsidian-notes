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
  - "8"
Chapter: 
Slides/Notes: 
Date: 2025-01-28
Date created: Tue., Jan. 28, 2025, 4:06:31 am
Date modified: Mon., Feb. 3, 2025, 4:56:46 pm
---

# Structs

- **Structs**
    - Used to *aggregate* data when values are not necessarily same type

|                     | Array                                           | Structure                              |
| ------------------- | ----------------------------------------------- | -------------------------------------- |
| Data of same type   | Yes                                             | Not required                           |
| Declaration details | Type, number of elements<br>Array `[]` notation | Types of *members*<br>`struct` keyword |
| Access via          | Index notation                                  | Dot notation                           |

- Use **member** to refer to item in structure
    - vs. *element* for array

```c title:structs.c
#include <stdio.h>
#include <string.h>

int main() {
    // Note that "struct" is required whenever we declare
    // a variable of a structure type:
    struct student {
        // Members of struct student:
        char first_name[20];
        char last_name[20];
        int year;
        float gpa;
    };

    // good_student is of type "struct student"
    struct student good_student;
  
    // Initialize the members of the struct using dot notation:
    strcpy(good_student.first_name, "Jo");
    strcpy(good_student.last_name, "Smith");
    good_student.year = 2;
    good_student.gpa = 3.2;
  
    // Print the values of good_student's members:
    printf("Name: %s %s\n",
           good_student.first_name, good_student.last_name);
    printf("Year: %d. GPA: %.2f\n", good_student.year, good_student.gpa);
  
    return 0;
}
```

## Using Struct in Functions

Recall:

- Cannot pass an array to a function
- Pass a pointer to the first element of the array instead
    - Function can make changes to the array

```c
#include <stdio.h>

/* Changes the value of the 0th element of int array numbers
 * to 80. 
 */
void change(int numbers[]) {
    numbers[0] = 80;
}

int main() {
    int my_array[5];

    my_array[0] = 40;
    change(my_array);

    // The following prints 80 because "change" modifies the
    // *original* array, since the array was *not* copied
    // when passed as an argument:
    printf("Element at index 0: %d\n", my_array[0]);
  
    return 0;
}
```

- & Structs do not work this way

```c
#include <stdio.h>
#include <string.h>

struct student {
    char first_name[20];
    char last_name[20];
    int year;
    float gpa;
};

/* This version of change() operates on a *copy* of struct s,
   not the original. This means that the print statement in main
   will print:
       first name if "Jo"
       GPA is 2.1
   ..since the original good_student is not modified by change().

====

void change(struct student s) {
    s.gpa = 4.0;
    strcpy(s.first_name, "Adam");
}
*/

/* Changes the values of a struct student's members. 
   This function operates on the *original* struct, rather
   than on a copy of the struct. */
void change(struct student *s) {
    strcpy((*s).first_name, "Adam");
    (*s).gpa = 4.0;
}

int main() {
    struct student good_student;
 
    strcpy(good_student.first_name, "Jo"); 
    good_student.gpa = 2.1;

    // Passing address of good_student:
    change(&good_student);
    
    printf("first name is %s\n",
                good_student.first_name);
    printf("GPA is %f\n", good_student.gpa);
  
    return 0;
}
```

- $ Structs are not passed to functions in the same way as arrays
    - Function *does* get a copy of the struct
    - Any change that function makes is only a change to the copy, not original struct
    - Array:
        - Gets passed as a pointer to its first element
        - Function not operating on copy of array

## Pointers to Structs

- Pointers also apply to structs
    - Declare pointers to point to structs
    - Change structs through their pointers
    - Pass pointers (to structs) to functions
        - Functions can modify structs

```c
#include <stdio.h>
#include <string.h>

int main() {
    struct student {
        char first_name[20];
        char last_name[20];
        int year;
        float gpa;
    };
  
    struct student s;

    // Cannot be dereferenced until it points to
    // allocated space:
    struct student *p;
  
    // Initialize the members of s:
    strcpy(s.first_name, "Jo");
    strcpy(s.last_name, "Smith");
    s.year = 2;
    s.gpa = 3.2;
  
    // p now points to the address of struct student s:
    p = &s;

    // Dereference p to get the struct, then use the
    // dot operator to access the "gpa" member:
    (*p).gpa = 3.8;

    p->year = 1;    // this is the same as (*p).year = 1;
    strcpy(p->first_name, "Henrick");
  
    printf("Name: %s %s\n",
           s.first_name, s.last_name);
    printf("Year: %d. GPA: %.2f\n", s.year, s.gpa);
  
    return 0;
}
```

- `(*p).gpa`
    - `*p` is in parentheses
    - Dot has a higher precedence than star
    - `*p.gpa` would try to access a member of a pointer
        - Pointers do not have members
- `p->year = 1`
    - Same as `(*p).year = 1`
    - Cleaner syntax
