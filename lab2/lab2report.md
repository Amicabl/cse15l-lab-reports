# Lab 2 Report

## Part 1 

### Code snippet of ChatServer.java

```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // A string holding the chat log that will be added onto with each /add-message request
    String chatLog = "";

    // Intended function: /add-message?s=<string>&user=<string>
    
    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Use /add-message to add a message.");
        } else if (url.getPath().equals("/add-message")) {
            String[] parameters = url.getQuery().split("=");
            String user = parameters[2];
            String temp = parameters[1];
            String[] messageTemp = temp.split("&");
            String message = messageTemp[0];
            chatLog += user + ": " + message + "\n";
            return chatLog;
        } else {
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

### Screenshot 1: `/add-message?s=Hey&user=Amicable`

![Image](lab2_1.png)

**Which methods in your code are called?** 

The `handleRequest` method is called when the request is made.

**What are the relevant arguments to those methods?** 

The URI `https://0-0-0-0-4000-hkvni56f5vad4pf4ukgdo1mcqk.us.edusercontent.com/add-message?s=Hey&user=Amicable` is passed in as an argument.

**What are the values of the relevant fields in the class? How do the values of any relevant fields of the class change from this specific request?** 

`url`: `https://0-0-0-0-4000-hkvni56f5vad4pf4ukgdo1mcqk.us.edusercontent.com/add-message?s=Hey&user=Amicable`\
The full URL gets passed in to the `handleRequest` method when the URL is loaded.

`url.getPath()`: `"/add-message"`\
The path changes from `/` to `/add-message`.

`url.getQuery()`: `"s=Hey&user=Amicable"`\
There was no query before the request was made. 

`parameters`: `["s","Hey&user","Amicable"]`\
The query is broken by `=` into three parts. `parameters` will be different depending on the query.

`user`: `"Amicable"`\
The `user` is extracted from the array `parameters`, which can change for each request.

`temp`: `"Hey&user"`\
The middle part of `parameters` is stored in `temp`.

`messageTemp`: `["Hey","user"]`\
`temp` is then split by `&` into two parts.

`message`: `"Hey"`\
The message is extracted from `messageTemp`.

`chatLog`: `"Amicable: Hey"`\
The `user`, a colon, the `message` and a newline (`\n`) is concatenated to become `chatLog`.  

### Screenshot 2: `/add-message?s=Hi!&user=jpolitz`

![Image](lab2_2.png)

**Which methods in your code are called?** 

The `handleRequest` method is called when the request is made.

**What are the relevant arguments to those methods?** 

The URI `https://0-0-0-0-4000-hkvni56f5vad4pf4ukgdo1mcqk.us.edusercontent.com/add-message?s=Hi!&user=jpolitz` is passed in as an argument.

**What are the values of the relevant fields in the class? How do the values of any relevant fields of the class change from this specific request?** 

`url`: `https://0-0-0-0-4000-hkvni56f5vad4pf4ukgdo1mcqk.us.edusercontent.com/add-message?s=Hi!&user=jpolitz`\
The full URL gets passed in to the `handleRequest` method when the URL is loaded.

`url.getPath()`: `"/add-message"`\
The path before was also `/add-message` so this value does not change.

`url.getQuery()`: `"s=Hi!&user=jpolitz"`\
The value changes because the query is different from the query during the previous request.

`parameters`: `["s","Hi!&user","jpolitz"]`\
The query is broken by `=` into three parts. `parameters` is different from the previous request because the query is different.

`user`: `"jpolitz"`\
The new `user` is extracted from the array. 

`temp`: `"Hi!&user"`\
The middle part of `parameters` is stored in `temp`.

`messageTemp`: `["Hi!","user"]`\
`temp` is then split by `&` into two parts.

`message`: `"Hi!"`\
The message is extracted from `messageTemp`.

`chatLog`: `"jpolitz: Hi!"`\
The `user`, a colon, the `message` and a newline (`\n`) is concatenated to become `chatLog`. Because `user` and `message` have changed, `chatLog` has also changed.

## Part 2

### Screenshot 1: Private SSH key

![Image](lab2_private.png)

Because `ls` successfully prints out the absolute path, this demonstrates that the location is valid. 

### Screenshot 2: Public SSH key

![Image](public_ssh.png)

`authorized_keys` is a file on the `arobertson@ieng6.ucsd.edu` server that contains the public key. The `authorized_keys` file is found in the `.ssh` directory. 

### Screenshot 3: Terminal interaction without password prompt

![Image](lab2_terminal.png)

Because I have an authorized key, the `ssh` command does not prompt me for a password when connecting to `arobertson@ieng6.ucsd.edu`.

## Part 3

I was not aware of the difference between a path and a query prior to Labs 2 and 3. I also did not know that there was a built-in Java class that handles URL requests. I learned how I could manipulate a webpage by inputting different paths and queries using `getPath()` and `getQuery()`.



