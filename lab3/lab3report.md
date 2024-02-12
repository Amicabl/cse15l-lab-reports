# Lab Report 3

## Part 1

For this part, I have chosen the bug with the `reverseinPlace()` method in `ArrayExamples.java`.

### A failure inducing input for the buggy program

```java
@Test 
public void testReverseInPlace2() {
    int[] input1 = {1, 2, 3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{3, 2, 1}, input1);
}
```

### An input that doesn't produce a failure

```java
@Test 
public void testReverseInPlace1() {
    int[] input1 = {3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
}
```

### The symptom

This screenshot shows the failure within the `ArrayTests.java` file.
![Image](lab3_tests.png)

This screenshot shows the "Test Results" output.
![Image](lab3_failure.png)
