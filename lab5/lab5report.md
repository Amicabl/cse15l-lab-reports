# Lab 5 Report

This lab report will detail a debugging scenario I have created from a subset of the files provided during the Week 4 Lab on Testing. To add more of my own creativity, I have modified the provided `LinkedListExample.java` file by first fixing the original bug and then creating a `main` method. My strategy for this lab report was to code a `bash` file that would perform a function related to Linked Lists and keep working until I encountered an interesting bug. 

## Setup Information

### File and Directory Structure

This file and directory structure was generated using the `tree` command installed on my local computer. The working directory is a folder I created called `Lab 5`.

```
amicable@Alexas-MBP Lab 5 % tree
.
├── LinkedListExample.java
└── linkedlist.sh

1 directory, 2 files
```

### File Contents

**LinkedListExample.java**

``` java
import java.util.NoSuchElementException;

class Node {
    int value;
    Node next;
    public Node(int value, Node next) {
        this.value = value;
        this.next = next;
    }
}
class LinkedList {
    Node root;
    public LinkedList() {
        this.root = null;
    }
    /**
     * Adds the value to the _beginning_ of the list
     * @param value
     */
    public void prepend(int value) {
        // Just add at the beginning
        this.root = new Node(value, this.root);
    }
    /**
     * Adds the value to the _end_ of the list
     * @param value
     */
    public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
        }
        n.next = new Node(value, null);
    }

    /**
     * @return the value of the first element in the list
     */
    public int first() {
        return this.root.value;
    }
    /**
     * @return the value of the last element in the list
     */
    public int last() {
        Node n = this.root;
        // If no such element, throw an exception
        if(n == null) { throw new NoSuchElementException(); }
        // If it's just one element, return its value
        if(n.next == null) { return n.value; }
        // Otherwise, search for the end of the list and return the last value
        while(n.next != null) {
            n = n.next;
        }
        return n.value;
    }
    /**
     * @return a string representation of the list
     */
    public String toString() {
        Node n = this.root;
        String s = "";
        while(n != null) {
            s += n.value + " ";
            n = n.next;
        }
        return s;
    }
    /**
     * @return the number of elements in the list
     */
    public int length() {
        Node n = this.root;
        int i = 0;
        while(n != null) {
            i += 1;
            n = n.next;
        }
        return i;
    }

    /**
     * This main method takes in a list of arguments and uses them
     * to populate a LinkedList.
     * 
     * @param args The integers to populate the LinkedList with. 
     */
    public static void main (String[] args) {
        // Creates a new LinkedList
        LinkedList myLinkedList = new LinkedList();

        // Creates the first node from the first argument and sets root
        Node rootNode = new Node(Integer.valueOf(args[0]),null);
        myLinkedList.root = rootNode;

        // Uses the append method to add the subsequent arguments
        for (int i = 1; i < args.length; i++) {
            myLinkedList.append(Integer.valueOf(args[i]));
        }

        // Prints out the toString representation of the list
        System.out.println(myLinkedList.toString());
    }
}
```

**linkedlist.sh**
``` bash
# Takes in the filename as an argument
javafile=$1

# Extracts the class name from the filename
classname=$(echo ${javafile%.*})

# Compiles the file
javac $javafile

# Echoes whether the compilation was successful based on exit code
if [[ $? -eq 0 ]]
then
    echo "Compilation successful"
else
    echo "Compilation not successful"
    exit
fi

# Saves the rest of the arguments after the file name to a variable
arguments="${@:2}"
# Saves the output of passing in those arguments to the LinkedList class
output=$(java LinkedList ${@:2})

# Included for debugging purposes
echo $arguments
echo $output

# If the outputted toString() of the LinkedList is the same as the arguments,
# code was successful
if [[ "$output" == "$arguments" ]]
then
    echo "Linked list creation successful"
else
    echo "Expected linked list was not the same as observed"
    echo "Expected: $arguments"
    echo "Observed: $output"
    exit
fi
```

