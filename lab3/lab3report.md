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
![Image](lab3_failure.png)

This screenshot shows the "Test Results" output.
![Image](lab3_test.png)

### The bug

**Before**
```java
// Changes the input array to be in reversed order
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
}
```

**After**
```java
// Changes the input array to be in reversed order
static void reverseInPlace(int[] arr) {
    int[] temp = new int[arr.length];
    for (int i = 0; i < arr.length; i += 1) {
      temp[i] = arr[i];
    }
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = temp[arr.length - i - 1];
    }
}
```
