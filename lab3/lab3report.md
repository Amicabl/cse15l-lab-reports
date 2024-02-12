# Lab Report 3

## Part 1

For this part, I have chosen the bug with the `reverseinPlace()` method in `ArrayExamples.java.`

### A failure inducing input for the buggy program
```
@Test 
	public void testReverseInPlace2() {
    int[] input1 = {1, 2, 3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{3, 2, 1}, input1);
	}
```
