# CSE 15l Lab Report 2 - Kiko Pan 

## Part 1 
Here is the code for my StringServer.
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
After running ```javac Server.java StringServer.java``` and ```java StringServer (port number)``` in the terminal, you should get a link to the local host port server where you can start adding strings. 

On

## Part 2 

## Part 3
