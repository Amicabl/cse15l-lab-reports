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
The issue with the "Before" code is that it tries to replace each element without temporarily saving the original value. When the loop reaches the middle of `arr`, the values in the first part of the array have already been overwritten with the values from the second half. When it tries to set the values in the second half of the array, it does not take from the original first half of the array as intended.

The fix in the "After" code creates a new temporary array called `temp` to store all of the original values in the `arr` array. To make sure that `arr` itself is not modified, `temp` is initialized using `arr.length` and the values from `arr` are copied over one by one using a `for` loop. `arr` is then modified by using values from the `temp` array which avoids the problem with the values being overwritten while still modifying the original array to fit the criteria of reversing in place.
