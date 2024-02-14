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

## Part 2

For this part, I am choosing to research the `find` command.

### Option 1: `find -size`

&NewLine;
**Source 1:** Entering `man find` into the terminal.\
**Source 2:** [Computer Hope](https://www.computerhope.com/unix/ufind.htm#Operators)

According to `man`, the `find -size` traverses the file hierarchies in lexographical order. From "Computer Hope," I learned that the `-size` command is considered a test because it returns true if the file matches the given condition. 
&NewLine;

**Example 1: `find . -size +100k`**
<blockquote> 
    
**Working Directory:** `terminal/`

The code below searches the current directory `terminal/` using `find .`. The test `-size +100k` indicates that the `find` command will check all files in `.` and determine whether or not each file takes up more than 100k of space. The output is all files recursively found in `terminal/` with a size greater than 100 kilobytes. 

```
amicable@Alexas-MacBook-Pro technical % find . -size +100k
./government/About_LSC/commission_report.txt
./government/About_LSC/State_Planning_Report.txt
./government/Env_Prot_Agen/multi102902.txt
./government/Env_Prot_Agen/ctm4-10.txt
./government/Env_Prot_Agen/bill.txt
./government/Env_Prot_Agen/tech_adden.txt
./government/Gen_Account_Office/d0269g.txt
./government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
./government/Gen_Account_Office/Sept27-2002_d02966.txt
./government/Gen_Account_Office/d01376g.txt
./government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
./government/Gen_Account_Office/pe1019.txt
./government/Gen_Account_Office/gg96118.txt
./government/Gen_Account_Office/d01591sp.txt
./government/Gen_Account_Office/im814.txt
./government/Gen_Account_Office/ai9868.txt
./government/Gen_Account_Office/May1998_ai98068.txt
./government/Gen_Account_Office/d02701.txt
./biomed/1471-2105-3-2.txt
./911report/chapter-13.4.txt
./911report/chapter-13.5.txt
./911report/chapter-13.2.txt
./911report/chapter-13.3.txt
./911report/chapter-3.txt
./911report/chapter-1.txt
./911report/chapter-6.txt
./911report/chapter-7.txt
./911report/chapter-9.txt
./911report/chapter-12.txt
```
</blockquote>

**Example 2: `find government/ -size -2k -type d`**
<blockquote>
    
**Working Directory:** `terminal/`

To explore whether or not the `-size` test checks for directories, I modified the command from above to only search for directories using `-type d`. Looking inside the `government/` directory using `find government/`, I recursively checked for all directories with a size less than 2 kilobytes. 

```
amicable@Alexas-MacBook-Pro technical % find government/ -size -2k -type d 
government/
government//About_LSC
government//Env_Prot_Agen
government//Alcohol_Problems
government//Post_Rate_Comm
```

I then compared this list with the full list of directories in `government/`.

```
amicable@Alexas-MacBook-Pro technical % find government/ -type d
government/
government//About_LSC
government//Env_Prot_Agen
government//Alcohol_Problems
government//Gen_Account_Office
government//Post_Rate_Comm
government//Media
```

Since this list is longer than the one searching for directories less than 2 kilobytes, I asserted that `government//Gen_Account_Office` and `government//Media` must have sizes greater than 2 kilobytes. 

</blockquote>

### Option 2: `find -user`


**Source 1:** [Computer Hope](https://www.computerhope.com/unix/ufind.htm#Operators)
**Source 2:** [nixCraft](https://www.cyberciti.biz/faq/how-do-i-find-all-the-files-owned-by-a-particular-user-or-group/)

According to nixCraft, `find -user` finds all files that are owned by the specified user.


**Example 1: `find . -user amicable`** 
<blockquote>

**Working Directory:** `terminal/`

Because I forked and cloned the repo from Github, I was curious to see whether or not I would be the user that owns all of the files. To test this, I decided to check the `terminal/` directory with my own username. 

A very long list of files printed out, so I decided to pipe the results into `head` to only display the first 10 lines of the output.

```
amicable@Alexas-MacBook-Pro technical % find . -user amicable | head -n 10
.
./government
./government/About_LSC
./government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
./government/About_LSC/Progress_report.txt
./government/About_LSC/Strategic_report.txt
./government/About_LSC/Comments_on_semiannual.txt
./government/About_LSC/Special_report_to_congress.txt
./government/About_LSC/CONFIG_STANDARDS.txt
./government/About_LSC/commission_report.txt
```
</blockquote>

**Example 2: `find biomed/ -user bob`**

**Working Directory:** `terminal/`

I then wanted to see what would happen if I put in a user that did not exist. I thought either nothing would print out, or there would be an error message. When I put in the user `bob`, I got an error saying that the user could not be found. This result was not surprising. 

```
amicable@Alexas-MacBook-Pro technical % find biomed/ -user bob
find: -user: bob: no such user
```

### Option 3: `find -
**Source 1:** [Computer Hope](https://www.computerhope.com/unix/ufind.htm#Operators)

**Example 1**
**Example 2**

### Option 4: Here we go

**Example 1**
**Example 2**
