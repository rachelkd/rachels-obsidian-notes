---
dg-publish: true
tags: ["#cs", "#java", "#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 3
Module:
  - "[[Java]]"
Date: 
Chapter: 2.8
Date created: Sat., Oct. 5, 2024, 8:33:03 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

# Being Comparable Enables Sorting

- Java provides a `sort` method capable of sorting an array of `int`s or of any other primited type
- `sort` is **[[Classes|overloaded]]**
    - Series of `sort` methods
    - Each one capable of handling one of the primitive types
    - Defined as static methods in the `Arrays` class

```java
int[] ages = {10, 24, 3, 45, 83, 9};
Arrays.sort(ages);
```

- What if we want to sort objects of some other types?
    - e.g., Java provides class `MonthDay`
        - keep track of the month and day parts of a date
        - What if we wanted to sort an array of `MonthDay` objects?
        - What if we wanted to sort an array of `File` objects and an array of `Double` objects?
    - & Designers of Java made a gneeral sort method that accepts an array of `Object`
- For `sort` to do its job, it must be able to *compare* the elements of the array to decide on their order
    - Cannot simply use operators like `<` to compare two instances of `MonthDay`
    - Operators will not accept instances of `MonthDay` as operands
- Instead, `sort` requires all elements in array must implement the `Comparable` interface
    - Interface requires any class that implements it to define `compareTo` method

```
int compareTo(T o)
    Compares this object with the specified object for order.
    Returns a negative integer, zero, or a positive integer
    as this object is less than, equal to, or greater than
    the specified object.
```

- Similar to Python’s `__lt__` method (less than)
    - Could compare and sort out objects
- Java: Return negative integer, zero, or positive integer instead of `True` or `False` in Python

## Example. `MonthDay`

- Authors of `MonthDay` class define this method and declare that the class *[[Interfaces|implements]]* the `Comparable` interface
- If we pass an array of `MonthDay` objects to sort, it is *guaranteed* to be able to use `compareTo` to put them in order

```java
public static void main(String[] args) {
    // Create some MonthDay objects and put them in an array.
    // This class does not provide any constructors. Instead, we
    // call static method "of" to make an instance.
    MonthDay md1 = MonthDay.of(1, 5);
    MonthDay md2 = MonthDay.of(7, 24);
    MonthDay md3 = MonthDay.of(7, 24);
    MonthDay md4 = MonthDay.of(1, 28);
    MonthDay md5 = MonthDay.of(2, 14);
    MonthDay[] dates = {md1, md2, md3, md4, md5};

    // Because MonthDay implements Comparable, we can call sort,
    // which depends on that:
    Arrays.sort(dates);
}
```

- Sort method has another requirement:
    - All elements in the array must be **mutually comparable**
    - Prevents us from trying to sort an array with a mixture of `DateTime` and `File` objects, for instance
    - Objects are comparable *within* each class
    - Not *across* classes
        - Unless classes which the objects are instances of share a parent class implementing `Comparable`

# Being Comparable Enables Comparisons

- Can use `compareTo` to compare two objects of a class

```java
System.out.println(md1.compareTo(md2)); // -6
System.out.println(md2.compareTo(md1)); //  6
System.out.println(md2.compareTo(md3)); //  0
```

# Making Our Own Classes Comparable

Consider this class `Review`:

```java
class Review {
    /**
     * A review, for example of a book or movie.
     */

    // === Class Variables ===

    // The name of the item that this Review is about.
    String item;
    // The numeric rating, between 0 and 100, associated with this Review.
    private int rating;
    // The written component of this review.
    private String text;
    // The number of likes that this review has received.
    private int likes;

    public Review(String item, int rating, String text) {
        this.item = item;
        this.rating = rating;
        this.text = text;
        this.likes = 0;
    }

    public String toString() {
        return this.item + " (" + this.rating + "): " +
            this.text + "; likes = " + this.likes;
    }

    /**
     * Records a like for this Review.
     */
    public void like() {
        this.likes += 1;
    }
}
```

- To make instances of this class *comparable*, we need to do two things:
    - Must implement the `compareTo` method
    - Must change class declaration to indicate that we have fulfilled the requirements of implementing the `Comparable` interface

```java
class Review implements Comparable<Review> {
    
    // Implementation omitted
    
    /**
     * Compares this object with the specified object for order.
     *
     * Returns a negative integer, zero, or a positive integer as this
     * object is less than, equal to, or greater than the specified object.
     *
     * @param other the object to be compared.
     * @return a negative integer, zero, or a positive integer as this
     * object is less than, equal to, or greater than the specified object.
     */
    @Override
    public int compareTo(Review other) {
        if (this.rating < other.rating) {
            return -1;
        } else if (this.rating > other.rating) {
            return 1;
        } else {
            return 0;
        }
    }
}
```

- `Comparable<Review>` vs. just `Comparable`
    - We *promise* that an instance of this class can be compared to any other instance of `Review`
    - To be consistent, parameter of `compareTo` is specifically type `Review`
    - Just `Comparable` $\implies$ `compareTo` would have to accept and deal with *any* `Object`

```java
public static void main(String[] args) {
    Review r1 = new Review("Emoji Movie", 10,
        "Cinematic malware");
    Review r2 = new Review("Dunkirk", 95,
        "Gifted ensemble cast and masterful direction");
    Review r3 = new Review("Spider Man: Homecoming", 95,
        "A fun adventure");
    Review r4 = new Review("My Neighbour Totoro", 99,
        "A work of art");
    Review r5 = new Review("Despicable Me3", 60,
        "Zany but scattershot humour");

    System.out.println(r1.compareTo(r2)); // -1
    System.out.println(r2.compareTo(r1)); // 1
    System.out.println(r2.compareTo(r3)); // 0

    Review[] badFruit = {r1, r2, r3, r4, r5};
    Arrays.sort(badFruit);
    for (int i = 0; i < badFruit.length; i++) {
        System.out.println(badFruit[i]);
    }

}
```

```
Emoji Movie (10): Cinematic malware; likes = 0
Despicable Me3 (60): Zany but scattershot humour; likes = 0
Dunkirk (95): Gifted ensemble cast and masterful direction; likes = 0
Spider Man: Homecoming (95): A fun adventure; likes = 0
My Neighbour Totoro (99): A work of art; likes = 0 
```

# Being Comparable Enables More

- Class that implements `Comparable` can be used in certain *Collections* that care about order
    - e.g., `SortedSet`
