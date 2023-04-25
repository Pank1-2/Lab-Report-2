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

A failing test case for ```averageWithoutLowest``` is one where there is two of the same lowest values. An example is shown below.
``` 
@Test 
public void testavg(){
    double[] input1 = {0.0, 2.0, 2.0, 5.0, 0.0};
    assertEquals(3, ArrayExamples.averageWithoutLowest(input1), 0.0); 
}
```
In this test case, it is expected that the output is 3 because the lowest value is 0.0 and since it shows up twice, we don't want to include either of them  in our calculations. So, 2 + 2 + 5 = 9 and since there are three values, we would divide it by 3 giving us 3. However, when running this test, we get 2.25 instead because the code divides by 4 instead of 3. 

An input that doesn't fail is one where there is only one lowest value. An example is shown below.
``` 
@Test 
public void testavg(){
    double[] input1 = {0.0, 2.0, 2.0, 5.0};
    assertEquals(3, ArrayExamples.averageWithoutLowest(input1), 0.0); 
}
```
Since this test case only has one lowest value, the code runs as expected. The ```averageWithoutLowest``` method is written to expect one lowest value which is why this one would work and the previous test case wouldn't. 

The symptom in this case is the 2.25 output when we ran the failing test. The bug is in the return statement. It returns the sum of all the values expect the lowest divided by the length of the array minus 1. It assumes that there is only one instance of the lowest value and doesn't take into account duplicates. 

Code with bug: 
``` 
static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);
  }
```

Code without bugs: 
``` 
static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    double low = 0;
    for(double num: arr){
      if(num == lowest){
        low++;
      }
    }
    return sum / (arr.length - low);
  }
```
In the fixed code, it uses a for loop and loops through the array and counts how many instances the lowest value shows up. Then instead of just subtracting the array length by 1 in the return statement, it subtracts by the number of times the lowest value occurs fixing the code. 

## Part 3
Although I found lab 2 and 3 a bit more challenging that I expected, I learned how to create and run web servers using a handler and how to fork changes straight onto my github repository. I was able to practice writing test cases and debug code. Although I already know how to write test cases, debugging the linked list helped me gain a better understanding of linked lists. 
