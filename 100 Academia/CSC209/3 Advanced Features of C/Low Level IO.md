---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [Low Level I/O]
Week: 5
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "9"
Chapter:
Slides/Notes:
Date: 2025-02-06
Date created: Wed., Feb. 5, 2025, 9:22:32 pm
Date modified: Thu., Feb. 6, 2025, 5:56:16 pm
---

# Low Level I/O

## Introduction to Binary Files

- Have been using text files when performing file input and output
- Not all files contain text
    - Ultimately: All files contain binary data
        - Bytes — 8 bits — that are either 1 or 0
- In text files:
    - Each byte holds a pattern that can be interpreted as a human-readable text character

<!-- break -->
- **Binary files**
    - e.g., Compiled C program
    - It is not human-readable
        - Will be displayed as junk; data in file is not intended to be read
- ? Why would we want to store files in binary vs. human-readable text?
    - Often no convenient way of storing something as text
        - e.g., Image files `.jpg`, `.gif` or music files `.wav`, `.mp3` have no obvious text in them
        - Hold numeric representations of pictures or music
        - No reason to invest some way to display these files as text
    - May choose to store data in a binary format if intended consumer is a *computer*
    - Storing data in binary format typically leads to smaller files than storing it in a text format
- ? If binary files are smaller and more versatile, why not use binary files for everything?
    - Binary files are not directly viewable or editable
    - In applications where you want humans to read or edit content, a text format is appropriate

> [!important] We handle binary formats in much the same way as we deal with text files

- Open file to use it in C program
    - & Opening binary file requires a different mode: `output_file = fopen("testing.dat", "rb")`
    - Include letter `b` in the mode that we pass to `fopen`
    - Name of file lacks `.txt` extension
        - `txt` stands for text, so it is misleading to name a binary file with that extension
- Binary file extensions:
    - No extension at all, `jpg`, `mp3`, etc.
    - Extension tells us how to interpret its contents

### Writing a Binary File

- Functions learned so far are not useful for binary files
    - Binary files have no notion of line
    - Data in binary file is not broken into lines unlike text files
- Functions introduced so far read and produce text
    - Not binary data!

#### `fwrite`

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)
```

- `ptr`
    - Pointer to data we want to write to the file
    - Typically starting address of array
    - Can also be a pointer to an individual variable, if we want to just write one value
- `size`
    - Size of each element that we are writing to the file
- `nmemb`
    - Number of elements we are writing to the file
    - For individual variable: `nmemb = 1`
    - For array: `nmemb = number of elements in the array`
- `stream`
    - File pointer to which we will write
    - Must refer to a stream open in *binary mode*
- % Ordering of elements is different from ordering we saw when working with text files
    - Recall: `int fprintf(FILE *stream, const char *format, …)`
        - From [[Files#Writing to Files]]

> [!question]+ What does `fwrite` return?
>
> - Number of elements successfully written to file
> - 0, if error occurred

- Typically add error checking code after calls
    - Just like calls that manipulate text

##### Example. Writing Array Data to a Binary File

```c
#include <stdio.h>

int main(void) {
    FILE *data_file;
    int error;
    int numbers[] = {400, 800, 1200, 1600, 2000};

    data_file = fopen("array_data", "wb");
    if (data_file == NULL) {
        fprintf(stderr, "Error: could not open file\n");
        return 1;
    }

    error = fwrite(numbers, sizeof(int), 5, data_file);

    // Check if all numbers were written to file
    if (error != 5) {
        fprintf(stderr, "Error: array not fully written to file\n");
        return 1;
    }

    error = fclose(data_file);
    if (error != 0) {
        fprintf(stderr, "Error: fclose failed\n");
        return 1;
    }

    return 0;
}
```

- Use `sizeof` to compute the size of each element added to binary file
    - Makes code *portable*

### Reading Binary Files

#### `fread`

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
```

- `ptr`
    - Pointer to memory where data from the file will be stored
- `size`
    - Size of one element
- `nmemb`
    - Number of elements to read
- `stream`
    - Stream to read from

<!-- break -->
- $ One difference between `fwrite` and `fread`
    - `fread` takes a *non-constant* pointer as first argument
        - `fwrite` takes a *constant* pointer
            - `fwrite` writes data from memory to file
                - Memory does not need to be modified
        - `fread` is writing data from the file to memory
            - ! Parameter cannot be `const`

<!-- break -->
- ? What does `fread` return?
    - Number of elements successfully read from the file
    - If `fread` returns 0, no elements were successfully read
        - Signals that error has occurred; or
        - End of file has been reached

##### Example. Reading a Binary File of Array Data

```c
#include <stdio.h>

#define NUM_ELEMENTS 5

int main() {
    FILE *data_file;
    int error;
    int numbers[NUM_ELEMENTS];
    int i;

    data_file = fopen("array_data", "rb");
    if (data_file == NULL) {
        fprintf(stderr, "Error: could not open file\n");
        return 1;
    }

    fread(numbers, sizeof(int), NUM_ELEMENTS, data_file);

    for (i = 0; i < NUM_ELEMENTS; i++) {
        printf("%d ", numbers[i]);
    }

    printf("\n");

    error = fclose(data_file);
    if (error != 0) {
        fprintf(stderr, "Error: fclose failed\n");
        return 1;
    }

    return 0;
}
```

## Reading and Writing .wav Files

- **wav** file
    - Binary file format
    - Stores sound data

![](https://i.imgur.com/9kLghKT.png)

- wav file structure:
    - Two parts:
        - **Header**
            - 44 bytes
            - Contains information about wav file including parameters required to properly play file in music program
        - One or more two-byte values called a **sample**
            - When wav file is created, audio signal is sampled many thousands of times per second
            - Each sample is stored as two-byte integers
- **Short integers**
    - Two bytes
    - Will use `short int` to read samples in wav file
- `od`
    - Linux utility to read binary data

```bash title:"Reading a short.wav wav file"
od -A d -j 44 -t d2 short.wav
```

- `-A d`
    - Translates `od` output from default base 8 to more-convenient base 10
- `-j 44`
    - Tells `od` to skip the first 44 bytes of the file
        - Header occupies the 44 bytes
        - No reason to look at or modify header
- `-t d2`
    - Tells `od` file consists of 2-byte values
- `short.wav`
    - The file to read

```bash title:Output
0000044        2        2        2        2        2        8        8        8
0000060        8        8        16       16       16       16       16       4
00000076       4        4        4        4
0000084
```

- First line:
    - Leftmost column says `44`
        - Means we are starting at byte 44 of the file
    - Rest of line are the two-byte integers found at `44`, `46`, `48`, etc.
- Next line:
    - Starting at byte `60`
- Last line:
    - End at byte `84`
    - → Entire file is `84` bytes

### Increasing the Sound in a .wav File

```c title:increase.c
#include <stdio.h>

#define HEADER_SIZE 44

/* Increases the volume of a .wav file (specified by argv[1]),
   and saves the altered version as a file whose name is
   specified by argv[2]. */
int main(int argc, char *argv[]) {
    char *input_name, *output_name;
    FILE *input_wav, *output_wav;
    short sample;
    short header[HEADER_SIZE];
    int error;

    if (argc != 3) {
        fprintf(stderr, "Usage: %s inputfile outputfile\n", argv[0]);
        return 1;
    }

    input_name = argv[1];
    output_name = argv[2];

    input_wav = fopen(input_name, "rb");
    if (input_wav == NULL) {
        fprintf(stderr, "Error: could not open input file\n");
        return 1;
    }

    output_wav = fopen(output_name, "wb");
    if (output_wav == NULL) {
        fprintf(stderr, "Error: could not open output file\n");
        return 1;
    }

    fread(header, HEADER_SIZE, 1, input_wav);
    error = fwrite(header, HEADER_SIZE, 1, output_wav);
    if (error != 1) {
        fprintf(stderr, "Error: could not write a full audio header\n");
        return 1;
    }

    while (fread(&sample, sizeof(short), 1, input_wav) == 1) {
        sample = sample * 4;
        error = fwrite(&sample, sizeof(short), 1, output_wav);
        if (error != 1) {
            fprintf(stderr, "Error: could not write a sample\n");
            return 1;
        }
    }

    error = fclose(input_wav);
    if (error != 0) {
        fprintf(stderr, "Error: fclose failed on input file\n");
        return 1;
    }

    error = fclose(output_wav);
    if (error != 0) {
        fprintf(stderr, "Error: fclose failed on output file\n");
        return 1;
    }

    return 0;
}
```

## Reading and Writing Structs

```c title:"Student Struct"
struct student {
    char first_name[20];
    char last_name[20];
    int year;
    float gpa;
}
```

- We could separately write the first name, last name, year, and GPA
    - This is tedious!

```c
#include <stdio.h>
#include <string.h>

int main() {
    struct student {
        char first_name[14];
        char last_name[20];
        int year;
        float gpa;
    };

    FILE *student_file;
    int error;
    struct student s;

    student_file = fopen("student_data", "wb");
    if (student_file == NULL) {
        fprintf(stderr, "Error: could not open file\n");
        return 1;
    }

    strcpy(s.first_name, "Betty");
    strcpy(s.last_name, "Holberton");
    s.year = 4;
    s.gpa = 3.8;

    fwrite(&s, sizeof(struct student), 1, student_file);

    strcpy(s.first_name, "Grace");
    strcpy(s.last_name, "Hopper");
    s.year = 6;
    s.gpa = 3.9;

    fwrite(&s, sizeof(struct student), 1, student_file);

    error = fclose(student_file);
    if (error != 0) {
        fprintf(stderr, "Error: fclose failed\n");
        return 1;
    }

    return 0;
}
```

- Use `sizeof` to get size of `struct`
    - C is allowed to insert space/padding between **members** of a struct
    - Eliminate need to guess the size of variables

<!-- break -->
- First name is allowed to be 13 characters, and then a null terminator
- Name does not always take up the full space
    - e.g., Betty is 5 characters, plus a null character

> [!question]+ What happens when we write the entire struct to a file?
> Compile and run the program
>
> ```bash
> od -A d -t u1 student_data
> ```
>
> ![](https://i.imgur.com/HFLyrlj.png)
>
> - Characters are one byte → Tell `od` to present data as one-byte unsigned decimal values
> - Characters should be encoded as ASCII
>     - `72` is the letter “H”
>     - “Hoberton” starts at position 15
> - $ `fwrite` places ==all== 14 bytes of the first name field into the file

## Moving Around in Files

- We have been writing files in order
    - From start → finish
- Recall:
    - `fgets`
        - Reads text files from beginning to end in [[Files]]
    - `fread`
        - Reads binary files from beginning to end

> [!question] Sometimes, we need to jump around the file. How do we read the same part of a file more than once, or skip parts of a file?

### `fseeks`

- Recall: Every open file maintains its current position
    - Position determines where next call of `fread` will read from, or
    - Where the next call of `fwrite` will write to
- % Each read or write call moves the file position

- `fseek`
    - Changes file position

```c
int fseek(FILE *stream, long int offset, int whence)
```

- `stream`
    - File pointer whose position we would like to change
- `offset`
    - Byte count
    - Indicates how much the file position should change
- `whence`
    - Determines how the second parameter — `offset` — is interpreted

| Constant   | Meaning                        |
| ---------- | ------------------------------ |
| `SEEK_SET` | From the beginning of the file |
| `SEEK_CUR` | From the current file position |
| `SEEK_END` | From the end of line           |

#### Examples

- `fseek(my_file, 50, SEEK_SET)`
    - Moves the file position *to* byte 50 of the file
- `fseek(my_file, -50, SEEK_END)`
    - Moves to the 50th byte *before* the end of the file
- `fseek(my_file, 10, SEEK_CUR)`
    - Advances file position 10 bytes *forward* from the current position
- `fseek(my_file, -10, SEEK_CUR)`
    - Moves 10 bytes *backwards* from current position

> [!question]+ If we try to set the file position to 50, and the file has less than 50 bytes, what will happen?
>
> - `fseek` call will succeed
> - A later read or write call will fail
>
> > [!danger] Check for errors in your read or write operations after using `fseek`!

> [!question]+ How can `fseek` fail?
>
> - If we pass an invalid file pointer
> - Can be avoided as long as you check your call to `fopen`

- `fseek` failure cases are better caught before or after the `fseek` call

```c title:create_student_file.c
#include <stdio.h>

#define NUM_STUDENTS 5

int main() {
    struct student {
      char first_name[20];
      char last_name[20];
      int year;
      float gpa;
    };

    FILE *student_file;
    int error;
    struct student s[5] = {
      {"Betty", "Holberton", 4, 3.8},
      {"Michelle", "Craig", 4, 3.5},
      {"Andrew", "Petersen", 3, 0.0},
      {"Daniel", "Zingaro", 1, 2.2},
      {"Grace", "Hopper", 6, 3.9}
    };

    student_file = fopen("five_students", "wb");
    if (student_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }

    fwrite(s, sizeof(struct student), NUM_STUDENTS, student_file);
    error = fclose(student_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

```c title:fseek.c
#include <stdio.h>

int main() {
    struct student {
        char first_name[20];
        char last_name[20];
        int year;
        float gpa;
    };

    FILE *student_file;
    int error, student_num;
    struct student s;

    student_file = fopen("five_students", "rb");
    if (student_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }

    printf("Type -1 to exit.\n");
    printf("Enter the index of the next student to view: ");

    scanf("%d", &student_num);
    while (student_num >= 0) {
        fseek(student_file, student_num * sizeof(struct student), SEEK_SET);

        error = fread(&s, sizeof(struct student), 1, student_file);
        if (error == 1) {
            printf("Name: %s %s\n", s.first_name, s.last_name);
            printf("Year: %d. GPA: %.2f\n", s.year, s.gpa);
        }
        else {
            fprintf(stderr, "Error: student could not be read.\n");
        }
        printf("Enter the index of the next student to view: ");
        scanf("%d", &student_num);
    }

    error = fclose(student_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

### Moving to the Beginning of a File

- Two ways:
    1. `fseek(my_file, 0, SEEK_SET)`
    2. & `void rewind(FILE *stream)`
        - Easier to use
