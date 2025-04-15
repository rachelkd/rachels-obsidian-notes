---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 3
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "5"
  - "9"
Chapter: 
Slides/Notes: 
Date: 2025-01-20
Date created: Tue., Jan. 21, 2025, 3:00:24 am
Date modified: Mon., Mar. 10, 2025, 2:12:13 am
---

# Files

## Introduction

- Variables in memory are ==temporary== and lost when the program stops
- Files allow data to persist between program executions
- Common uses:
    - Storing program results
    - Reading input data

### Opening Files

- Use `fopen` to open a file and create a ==stream==
- Syntax:

    ```c
    FILE *file_pointer = fopen(const char *filename, const char *mode);
    ```

- Modes:
    - `"r"`: Read from file
        - File must exist
    - `"w"`: Write to file
    - `"a"`: Append to file
    - More modes in [[#^0a97f3|Week 5 Review]]

### File Pointer

- `fopen` returns a ==file pointer== (`FILE *`)
- Used to:
    - Read/write to the file
    - Close the file
- Always check if `fopen` returns `NULL` (indicates failure):

    ```c
    if (file_pointer == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }
    ```

### Closing Files

- Use `fclose` to close a file
- Syntax:

    ```c
    int error = fclose(file_pointer);
    ```

- Returns `0` on success, nonzero on failure
- Always check for errors:

    ```c
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }
    ```

### Example. Opening and Closing a File

```c title:fopen.c
#include <stdio.h>

int main() {
    int error;
    FILE *scores_file;

    scores_file = fopen("top10.txt", "r");
    if (scores_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }

    printf("File opened: we can use it here\n");

    error = fclose(scores_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

```plaintext title:top10.txt
Dan 12000
Michelle 11400
Diane 10700
Anya 9900
Arnold 9200
Jen 8000
Paul 7000
Danny 4500
Karen 4400
Andrew 1337
```

^7a1288

- Files provide ==persistent storage== for data
- Always check for errors when opening/closing files
- Use `fprintf(stderr, …)` for error messages
- Clean up resources by closing files after use

## Reading From Files

### `fgets` Function

- Used to read ==text== or complete lines from a stream

    ```c
    char *fgets(char *s, int n, FILE *stream);
    ```

- Parameters:
    - `s`: Pointer to memory where the read data is stored
        - e.g., `char array`
    - `n`: Maximum number of characters to read
        - Including `\0`
    - `stream`: Source of data (e.g., file pointer or `stdin`)
- Returns:
    - On success: `s`
    - On failure: `NULL`
- Reads at most `n - 1` characters
- Stops reading at the ==end of a line== or end of file
- Adds a **null terminator** `\0` to the end of the string

#### Example. Reading from a File

```c
#include <stdio.h>
#define LINE_LENGTH 80

int main() {
    FILE *scores_file;
    int error;
    char line[LINE_LENGTH + 1];  // +1 for null-terminator

    scores_file = fopen("top10.txt", "r");
    if (scores_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }

    // Read each line until end of file
    while (fgets(line, LINE_LENGTH + 1, scores_file) != NULL) {
        printf("%s", line);
    }

    error = fclose(scores_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

### File Cursor

- A ==cursor== keeps track of the current position in the file
- Starts at the beginning of the file
- Moves forward after each successful `fgets` call
- Reaches the end of the file after reading all lines

```c
#include <stdio.h>
#define LINE_LENGTH 80

int main() {
    char line[LINE_LENGTH + 1];

    while (fgets(line, LINE_LENGTH + 1, stdin) != NULL) {
        printf("You typed: %s", line);
    }

    return 0;
}
```

- `fgets` can also read from `stdin` (keyboard by default)
- `fgets` is ideal for reading ==text files== line by line
- Always account for the ==null terminator== when defining buffer size
- Use `stdin` for reading input from the keyboard
- Check for `NULL` to detect end of file or errors

### `fscanf` Function

- Similar to `scanf`, but reads from any ==stream== (not just `stdin`)

    ```c
    int fscanf(FILE *stream, const char *format, ...);
    ```

- Returns:
    - Number of items successfully read
    - `EOF` if end of file is reached or an error occurs
- First argument is a ==file pointer== (`FILE *`)
- Can read from files, not just standard input
- Same format specifiers as `scanf` (e.g., `%d`, `%s`, `%f`)

#### Example. Reading Structured Data

```c
#include <stdio.h>

int main() {
    FILE *scores_file;
    int error, total;
    char name[81];  // 80 characters + null terminator

    scores_file = fopen("top10.txt", "r");
    if (scores_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }

    // Read name and score from each line
    while (fscanf(scores_file, "%80s %d", name, &total) == 2) {
        printf("Name: %s. Score: %d.\n", name, total);
    }

    error = fclose(scores_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

## Writing to Files

- Recall: Opening files:
    - `output_file = fopen("my_file.txt", "w")`
- Whenever we write to files, we must use modes:
    - `"w"`, or
        - Write (and overwrite, if it exists) a file
    - `"a"`
        - Append to file, if it exists
- Two ways to print:
    - `printf`
    - **`fprintf`**
        - Allows you to ==specify stream== where output should go

### `fprintf`

```c
#include <stdio.h>

int main() {
    FILE *output_file;
    int error;
    int total = 50;
    float small_number = 0.125;
  
    output_file = fopen("myfile.txt", "w");
    if (output_file == NULL) {
        fprintf(stderr, "Error opening file\n");
        return 1;
    }
  
    fprintf(output_file, "This will be the first line in the file\n");
    fprintf(output_file, "The integer is %d\n", total);
    fprintf(output_file, "The small float number is %f\n", small_number);
  
    error = fclose(output_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed\n");
        return 1;
    }

    return 0;
}
```

### Buffering

- When program writes to a *stream*, it writes to a location in memory controlled by the OS
    - Memory is periodically written to file on disk
    - When the write-backs occur depends on how the stream is set up
- ? What happens if computer loses power in the middle of the program?
    - ? Will we see all, some, or none of the `fprint` statements?
        - Behaviour is undefined
        - ! Not recommended to use I/O — file I/O or standard output — for debugging
        - If program crashes or terminates abnormally:
            - `fprintf`, `printf` statements that were executed might not be written to disk (or screen, if `stdin`)

<!-- break -->
- If you need to make sure that key modifications have been made to a stream you are writing:
    - **Flush** the stream
    - `int fflush(FILE *stream)`
        - Requests OS to write any changes in its buffer
    - % We do not typically need to do this: OS will do it for you

> [!summary]+ Risks of using `fprintf` and `printf`
> - Results of statements are undefined in the event of a crash
> - In the case of abnormal termination, executed statements may not be written to the disk or the screen
> - Not reliable for debugging
> - Unknown what the results of the calls will be if computer loses power in the middle of the program

## Reading and Writing a File: Putting It All Together

- Suppose we have our Top 10 Scores file
- Want to create a new file that contains only the names
    - One name per line
    - No scores
- → Use a combination of `fscanf` and `fprintf`

![[#^7a1288]]

```c
#include <stdio.h>

int main() {
    // Add second file pointer to refer to output file
    FILE *scores_file, *output_file;
    int error, total;
    char name[81];
  
    scores_file = fopen("top10.txt", "r");
    if (scores_file == NULL) {
        fprintf(stderr, "Error opening input file\n");
        return 1;
    }
    
    // Open file in write mode
    output_file = fopen("names.txt", "w");
    if (output_file == NULL) {
        fprintf(stderr, "Error opening output file\n");
        return 1;
    }
  
    while (fscanf(scores_file, "%80s %d", name, &total) == 2) {
        printf("Name: %s. Score: %d.\n", name, total);
        
        // Print to output file
        fprintf(output_file, "%s\n", name);
    }
  
    error = fclose(scores_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed on input file\n");
        return 1;
    }

    error = fclose(output_file);
    if (error != 0) {
        fprintf(stderr, "fclose failed on output file\n");
        return 1;
    }

    return 0;
}
```

## Low Level I/O: Binary Files

See [[Low Level IO|Low Level I/O]].

## Week 5 Review

`@ 2025-02-06`

```c
FILE *fopen(const char *pathname, const char *mode)
```

| Mode | When file exists       | When file does not exist | Readable | Writeable |
| ---- | ---------------------- | ------------------------ | -------- | --------- |
| `r`  | Start at beginning     | Error                    | Yes      | No        |
| `w`  | File erased            | Creates empty file       | No       | Yes       |
| `a`  | Start at *end* of file | Creates empty file       | No       | Yes       |
| `r+` | File erased            | Creates empty file       | Yes      | Yes       |
| `a+` | Start at end of file   | Creates empty file       | Yes      | Yes       |

^0a97f3

> [!note] `rb`, `wb` from PCRS aren’t actually needed on Unix/Linux, but they do help with line endings on Windows (non WSL Windows)

### Read Operations

- Formatted read
    - `int fscanf(FILE *stream, const char *format, …);`
    - Formatted info has to be exactly correct to be useful
        - Sample applies for `scanf`
- Read a line of text
    - `char *fgets(char *s, int size, FILE *stream);`
- Read bytes (binary data)
    - `size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);`

### Write Operations

- Formatted write
    - `int fprintf(FILE *stream,  const char *format, …)`
- Write a line of text
    - `int fputs(const char *s, FILE *stream);`
- Write bytes
    - i.e., [[Low Level IO|Binary data]]
    - `size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);`

### Other Operations

See [[Low Level IO#Moving Around in Files]] for more details.

- Change file position (cursor position)
    - `int fseek(FILE *stream, long offset, int whence);`
    - Cursor is moved by `offset` relative to value of `whence`

- Values for `whence`:
    - `SEEK_SET`
        - Relative to beginning of the file
    - `SEEK_CUR`
        - Relative to current position of cursor
    - `SEEK_END`
        - Relative to end of file
        - Offset should be a *negative* number

<!-- break -->
- Move to beginning of file:
    - `void rewind(FILE *stream)`

<!-- break -->
- Close a stream:
    - `int fclose(FILE *stream)`

### Viewing a Binary File

- `od`
    - Octal dump
    - Useful for hex, octal, or decimal output
    - `od -A <address format> -t <output format> <FILE>`
- `xxd`
    - Useful for hex or binary output
    - `xxd [-b] <FILE>`

### Buffering

- High-level library
    - e.g., `fopen`, `fgets`, `fread`, `fprintf`, `printf`, etc.
- `stdin`
    - Line *buffered*
- `stdout`
    - Line *buffered*
    - Block buffered if redirected to a file
- `stderr`
    - Unbuffered

### Key Points

- & Data is just bytes
    - Always!
- What matters is how data is *interpreted*
