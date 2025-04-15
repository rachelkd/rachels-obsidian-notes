---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 11
Module:
  - "[[3 - Professional and Miscellaneous Topics]]"
Lecture:
  - "21"
Chapter:
Slides/Notes:
  - "[[21-regex.pdf]]"
Date: 2024-11-19
Date created: Thu., Nov. 21, 2024, 9:19:22 pm
Date modified: Sat., Dec. 7, 2024, 10:29:13 pm
aliases: [Regex]
---

Explain what these are:

```regex
^[a-z][a-zA-Z0-9]*$
[abc]C[a-e][24680]*[A-Z]
\(\d{3}\) \d{3}-\d{4}
^(([de])f)\2\1$
[a-t&&[r-z]]
```

# Regular Expression

> [!def]+ Regular Expression
>
> - A sequence of characters that specifies a match pattern in text

- A given regex will match a set of strings

> [!question]+ What can we do with regex?
>
> - & Extract **substrings** which match a pattern
> - Does a given string match a pattern?
> - Find data in documents
>     - e.g., Phone numbers (in a file, on a webpage)
>     - Email addresses
>     - CSC course codes
>     - Find (and replace) in IntelliJ
> - Check for validity
>     - Is a password strong enough with the right set of characters?
>     - Does a variable conform to the Java style guidelines

## Describing a Set of Strings

- IDEs e.g., IntelliJ might describe the Java naming conventions for variables this way:
    - `^[a-z][a-zA-Z0-9]*$`
    - Start with a lowercase, then 0+ letters or numbers
    - Does the empty string match this guideline?
        - No; missing lowercase first letter
        - `[a-z]` has nothing to match

### `^[a-z][a-zA-Z0-9]*$`

- Describes a *pattern* that appears in a set of strings
- We say that any such string ==*matches* or *satisfies* the regular expression==
- A string which matches this regex is consistent with the Java naming conventions for variables
    - Can see lots of similar examples in `mystyle.xml` config file for Checkstyle

<!-- break -->

- `^` (carat) character:
    - The pattern must start at the beginning of the string
    - Called an **anchor**
- Square brackets `[]`:
    - Choose ==one== of the characters listed inside
    - Leftmost set of brackets `[a-z]`:
        - Given all lowercase English letters to choose from
    - Second character comes from second set of square brackets:
        - Any lowercase, uppercase letter, or digit
- `*`:
    - Zero or more of whatever immediately precedes it
    - Example of a **quantifier**
        - `+` means “one or more”
- `$`
    - Signifies the end of the string
    - Another **anchor**

> [!example] Examples that satisfy the regex
>
> - `x`
> - `numStudents`
> - `obj1`

> [!example]+ Examples that do *not* satisfy
>
> - `Alphabet`
>     - Starts with capital
> - `2ab`
>     - Starts with digit
> - `next_value`
>     - Underscore

> [!question]+ What if there are no anchors?
> Given `[abc]C[a-e][24680]*[A-Z]`,
>
> - When applied to a string, it will find the **substrings** that match
> - e.g.,
>     - `cCaA` matches
>     - `ABCcCcCc1A23` contains substrings that match
>         - `cCcC` matches, but not `cCa1A`

## Special Symbols

- A period `.`
    - Matches *any* character
- White space characters
    - Backslash is the **escape** character
    - `\s` : Any white space character
    - `\t`: Tab character
    - `\n`: New line character
- `^` inside a square bracket has another meaning
    - Matches any character *except* the contents of the square brackets
    - e.g., `[^aeiouAEIOU]` matches anything that is *not* a vowel

## Character Classes

> [!tip] You can make your own character classes by using square brackets

- e.g., `[q-z]`, `[AEIOU]`, `[^1-3a-c]`, or
- Use a ==pre-defined class==

![](https://i.imgur.com/huBfc97.png)

- Note that `\w` includes `_`

## Quantifiers

| Character | Meaning                |
| --------- | ---------------------- |
| `*`       | 0 or more              |
| `+`       | 1 or more              |
| `?`       | 0 or 1, i.e., optional |

- Append `{2}` to a pattern for exactly ==two copies of the same pattern==
- `{2, }` for ==two or more copies of the same pattern==
- `{2,4}` for two, three, or four copies of the same pattern

![](https://i.imgur.com/STrUS6R.png)

## Escaping a Symbol

> [!hint]+ Sometimes we want symbols to show up in the string that otherwise have meaning in regular expressions
>
> - *Escape* the meaning of the symbol by writing `\` in front of it

- A period `.` means any character
- To have a period show up in the string:
    - We write `\.`

> [!example]+ `abc123` matches the regex `[a-e][a-e].+`

> [!example] `1.4` matches the regex `[0-9]\.[0-9]`

## Pattern Repetition vs. Character Repetition in Regex

> [!example]+ Phone Number Pattern
> Pattern: `\(\d{3}\) \d{3}-\d{4}`
>
> - Matches: (123) 456-7890
> - Breaks down to:
>     - `\(` : Literal opening parenthesis
>     - `\d{3}` : Any three digits
>     - `\)` : Literal closing parenthesis
>     - Space
>     - `\d{3}` : Any three digits
>     - `-` : Literal hyphen
>     - `\d{4}` : Any four digits

- Two ways to specify pattern repetition:
  1. Manual repetition: `\d\d\d`
  2. Quantifier notation: `\d{3}`

> [!question] What if we wanted to repeat the *exact* same digit three times instead?

### Character Repetition

> [!warning]+ `\d{3}` will match ANY three digits (e.g., 123, 456, 789)
> It does NOT ensure the digits are the same (like 111, 222, 333)

> [!def]+ Backreferences `()`
>
> - Use `()` to create **capture groups**
> - Reference captured groups with `\n` where n is group number
> - Groups ==numbered by position of opening bracket== from left

> [!example]+ Examples
>
> 1. `(\d\d\d)\1a\1` matches “124124a124”
>
>    - Group 1: `(\d\d\d)` captures “124”
>    - `\1` repeats “124”
>    - `a` matches literal “a”
>    - `\1` repeats “124” again
>
> 2. `^(([de])f)\2\1$` matches “dfddf”, “efeef”
>    - Group 1: `[de]f` captures “df” or “ef”
>    - Group 2: `[de]` captures “d” or “e”
>    - `\2` repeats Group 2 (single letter)
>    - `\1` repeats Group 1 (letter + f)

## Logical Operators in Regex

> [!info]+ Key Operators
>
> - `|` : OR operator
> - `&&` : Intersection operator
>     - Intersection of the range *before* the ampersands and the range that appears *after*

> [!example]+ Examples
>
> - `[a-t&&[r-z]]` matches `r`, `s`, `t`
>     - `a-t`: letters a through t
>     - `r-z`: letters r through z
>     - Intersection: only letters in both ranges

> [!warning] Different regex implementations may support different operators

![](https://i.imgur.com/neVPo3Q.png)

## Anchors

![](https://i.imgur.com/VeXErjh.png)

# Regex in Java

> [!info]+ Classes
>
> 1. `String` Methods:
>
>    - `split()`
>    - `matches()`
>    - `replaceAll()`
>    - `replaceFirst()`
>
> 2. `Pattern` Class
>
>    - For regex compilation and matching
>    - [Java 8 Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)
>
> 3. `Matcher` Class
>    - For performing match operations
>    - [Java 8 Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html)

> [!warning]+ If we want a  `\` in regex when using Java…
>
> - We need two backslashes in Regex: `\\`
> - `\` is also the escape character in Java
> - $\implies$ We need four backslashes in a Java String for one backslash in Regex: `\\\\`

```java title:RegexMatcher.java
import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/*
 * Come up with a string that satisfies [abc]* but not a*b*c*
 * cb matches the entire regex [abc]*, but not a*b*c*
 */
/**
 * A demo of Java regular expressions.
 * @author campbell
 * @author lshorser
 * @author pgries
 */
public class RegexMatcher {
    /**
     * Prompts the user to enter a regular expression and a string,
     * and reports whether that regular expression matches the string.
     * The user types quit to exit.
     */
    public static void doMatching() {
        Scanner sc = new Scanner(System.in);
        String re, line;
        System.out.print("Regular expression: ");
        re = sc.nextLine();
        while (!re.equals("quit")) {
            System.out.print("String: ");
            line = sc.nextLine();
            System.out.println(Pattern.matches(re, line));
            System.out.print("Regular expression: ");
            re = sc.nextLine();
        }
        sc.close();
    }

    public static void main(String[] args) {
        // You can do an individual match in one easy line:
        System.out.println(Pattern.matches("a*b", "aaaaab"));
        // Notice that it automatically anchors
        // That is, it is equivalent to ^a*b$
        System.out.println(Pattern.matches("a*b", "baaaaab"));
        System.out.println();

        // If you never reuse the same pattern, this is fine.
        // As in this method:
        doMatching();

        // But if you plan to reuse a pattern, it's more efficient
        // to let Java build the matching infrastructure once and
        // reuse it for each match against that pattern.
        Pattern p = Pattern.compile("CSC[0-9][0-9][0-9]H1(F|S)");
        Matcher m = p.matcher("CSC207H1S");
        System.out.println("Does CSC207H1S match " + p + " ?");
        System.out.println(m.matches());

        // Here we reuse that (under the hood) infrastructure.
        System.out.println("Does CSC199H1Y match " + p + " ?");
        System.out.println(p.matcher("CSC199H1Y").matches() + "\n");

        // The matcher has other methods that let you find out
        // which substrings matched with which "capturing group"
        // of the pattern. Each capturing groups begins with a
        // left bracket. The capturing groups are numbered from 0,
        // and group 0 is the whole pattern.
        // Here I add more brackets to the pattern.
        // This will allow us to capture the group of characters
        // that is the course number.
        // (Exercise: rewrite the pattern to be more concise)
        p = Pattern.compile("CSC([0-9][0-9][0-9])H1(F|S)");
        m = p.matcher("CSC207H1S");
        System.out.println(m.matches());
        System.out.println(m.group(0)); // the entire string
        System.out.println(m.group(1)); // the first group: 207
        System.out.println(m.group(2)); // the second group: S

        // Using a backreference.
        p = Pattern.compile("(\\d\\d\\d)ABC\\1");
        m = p.matcher("123ABC123");
        System.out.println(m.matches());
        m = p.matcher("123ABC456");
        System.out.println(m.matches());
        //String s = "hello world";
        //System.out.println(s.matches("h[a-z]*\sw[a-z]*");
    }
}
```

```java title:RegexChecker.java
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class RegexChecker {
    public static void main(String[] args) {
        // Match one String against one regex
        System.out.println(Pattern.matches("([.w]){2}f", "..f"));
        System.out.println(Pattern.matches("[k*]a*b", "kaaaaabx"));

        // Using a Matcher object to find matching substrings
        Pattern pattern = Pattern.compile("[a-c]\\+{5}\""); // \\+{5} becomes +{5} in the regex
        Matcher matcher = pattern.matcher("a1bb\\\\c2a4\\\\b+++++\"6c89\\\\c+++++\"");
        while (matcher.find()) {
            System.out.println(matcher.group());
        }

        // Using the String method "matches" to check if a String matches a regex
        String str = "Hello_World11";
        boolean isSame = str.matches("(Hello)\\w.*(1)\\2");
        System.out.println(isSame);
    }
}
```
