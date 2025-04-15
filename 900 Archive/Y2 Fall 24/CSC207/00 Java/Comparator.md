---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 3
Module:
  - '[[Java]]'
Date:
Chapter: 2.9
Date created: Sat., Oct. 5, 2024, 9:00:47 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

> [!question] What if we want to be able to choose between ordering reviews according to their rating, the length of their text, or the number of likes they have?

> [!question] What if we want to make instances of `MonthDay` comparable according to just the month, so February 14th and February 20th would be considered tied?

- Can only be one `compareTo` in the class
- Didn’t write this class → can’t change what its `compareTo` does

- Define a **comparator** class for each kind of comparison we want.
    - **Comparator** class implements the `Comparator` interface, which requires this method:

```
int compare(T o1, T o2)
    Compares its two arguments for order.
    Returns a negative integer, zero, or a positive integer
    as the first argument is less than, equal to, or greater
    than the second.
```

Consider an example of a `Comparator` that orders `Reviews` according to likes.

```java
import java.util.Comparator;

class LikesComparator implements Comparator<Review> {
     /**
      * Compares its two arguments for order.
      *
      * Returns a negative integer, zero, or a positive integer
      * as r1 is less than, equal to, or greater than r2 in terms
      * of number of likes.
      *
      * @param r1 the first Review to compare
      * @param r2 the second Review to compare
      * @return a negative integer, zero, or a positive integer
      *      as r1 is less than, equal to, or greater than r2
      */
    @Override
    public int compare (Review r1, Review r2) {
        return r1.getLikes() - r2.getLikes();
    }
}
```

- `compare` method needs to know the number of likes a review has received
    - Stored in a private instance variable → Added a getter method to provide access to it

> [!abstract]+ Now, we can use a version of `sort` that accepts a `Comparator` as a second argument
> - Uses it to determine how things are sorted

```java
public static void main(String[] args) {
    Review[] freshVeg = {r1, r2, r3, r4, r5};
    // Let's add some likes so the sorting will be interesting.
    r1.like();
    r1.like();
    r1.like();
    r4.like();
    r3.like();
    Arrays.sort(freshVeg, new LikesComparator());
    for (int i = 0; i < freshVeg.length; i++) {
        System.out.println(freshVeg[i]);
    }
}
```

```
Dunkirk (95): Gifted ensemble cast and masterful direction; likes = 0
Despicable Me3 (60): Zany but scattershot humour; likes = 0
Spider Man: Homecoming (95): A fun adventure; likes = 1
My Neighbour Totoro (99): A work of art; likes = 1
Emoji Movie (10): Cinematic malware; likes = 3
```

# When to Use Comparable vs. Comparator?

- Not the author of the class $\implies$ cannot make it `Comparable`
    - *Must* define one or more `Comparator`s
- Author of class $\implies$ Both options available