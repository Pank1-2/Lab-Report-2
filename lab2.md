# CSE 15l Lab Report 2 - Kiko Pan 

## Part 1 
Here is the code for my ```StringServer``` class.
```import java.io.IOException;
import java.net.URI;

public class StringServer{
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}

class Handler implements URLHandler {
    String x = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return x;
        } 
        else if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                x += parameters[1] + '\n';
                return x;
            }
        }
        return "String Not Found!";
        
    }
} 
```
After running ```javac Server.java StringServer.java``` and ```java StringServer (port number)``` in the terminal, you should get a link to the local host port server where you can start adding strings. These two commands calls the ```StringServer``` class and creates a local server. 

Once you open the local host link it should look blank like the image below! <img src = "Screen Shot 2023-04-24 at 2.22.55 PM.png" width = "500" height ="250">

When you add a message using ```/add-message?s=<string>``` it should appear on the screen like the example below. When you're adding a message, it calls the ```Handler``` class which implements ```URLhandler```. It then runs through the if statements in the ```handleRequest``` method. In this case, it satisfies the else if statement since the input contained ```/add``` and it then splits the input at the '=' sign. Once that is done, it updates the empty string variable that was created at start of the ```Handler``` class and adds a new line. Lastly, it will return the string variable with the string the user inputted, in this case it's 'Hello'.

<img src = "Screen Shot 2023-04-24 at 2.23.15 PM.png" width = "500" height = "200">

If you wanted to add another message after the first one, it should save the previous messages and add the new message on the line below. Similarly, when adding a new message, it goes through the same process as the one above where it runs through the if statements in the ```handleRequest``` class. The message here is being added onto the previous one so it updates the existing string by adding the current inputted string onto the previous string variable. 

<img src = "Screen Shot 2023-04-24 at 2.23.24 PM.png" width = "500" height = "190">
In both cases, nothing about the fields of the class changed. The empty string that was first created when the class was called is being updated with strings based on the user's input. If the input has the format of ```/add-message?s=<string>``` it should consider whatever is after the = sign as a string despite its value because the value is converted into a string.  

## Part 2 
The buggy program I will be looking at is the  ```averageWithoutLowest``` method. In this method returns the averages of all the elements in the array besides the lowest value and if there are no elements or just one element in the array, it should return 0. 

A failing test case for ```averageWithoutLowest``` is one where there is two of the same lowest values. 

## Part 3
Although I found lab 2 and 3 a bit more challenging that I expected, I learned how to create and run web servers using a handler and how to fork changes straight onto my github repository. 
# Also
