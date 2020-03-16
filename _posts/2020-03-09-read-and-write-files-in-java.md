---
layout: post
title: BufferedReader, FileReader and FileWriter Java Classes
tags: [Linux, Java, OOP]
---

In this post, I will blog how to read and write files in `Java` using the classes `BufferedReader`, `FileReader` and `FileWriter`. This toy exercise was inspired by the problem of a bigger project that I coded a long time ago. We have a comma-separated file (`CSV`) with the information of points in a two-dimensional plane, such as in each line we can find the group of points that form a polygon. One row has the following form: ```3150,1486,3155,1486,3157,1489, ...```, where each pair of digits represents a point.

For this exercise, we simply read the `CSV` file origin. Then translate each point 5 pixels on both `x` and `y` axis. Finally, we write the result in a new file. By default, the `FileWriter` object is created without append mode. If we want to open the file in append mode we can use `new FileWriter(csvOutFile, true)`. According to the documentation, it is recommended to wrap `BufferedReader` around `FileReader` or other readers to alleviate potential costs associated with reading operations. The following code fragment shows the implementation of this toy exercise.

{% highlight java linenos %}

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class ReadWriteData
{
    public static void main(String args[])
    {
        String csvFile = "data-file.csv";
        String csvOutFile = "data-file-out.csv";
        String line;
        
        try(BufferedReader br = new BufferedReader(new FileReader(csvFile)))
        {
            // write the data to a new file
            FileWriter writer = new FileWriter(csvOutFile);
            
            while((line = br.readLine()) != null){
                String[] lineValues = line.split(",");
             
                for(int i = 0; i < lineValues.length; i++){
                    int value = Integer.parseInt(lineValues[i]);
                    value += 5;
                    try{
                        writer.append(String.valueOf(value));
                        
                        if (i != lineValues.length - 1)
                            writer.append(",");
                    }
                    catch(Exception e){
                        System.out.println("Error CsvFileWriter.");
                        e.printStackTrace();
                    }
                }
                
                writer.append("\n");
            }
            
            writer.flush();
            writer.close();
        }
        catch(IOException ioe){
            ioe.printStackTrace();
        }
    }
}

{% endhighlight %}


To compile this code we use:

```
$ javac ReadWriteData.java
```

We execute the compiled program as follows:

```
$ java ReadWriteData
```

The `java` [3] version used is as follows:

```
$ java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (Zulu 8.44.0.11-CA-linux64) (build 1.8.0_242-b20)
OpenJDK 64-Bit Server VM (Zulu 8.44.0.11-CA-linux64) (build 25.242-b20, mixed mode)
```


#### References

1. [javase](https://docs.oracle.com/javase)
2. [BufferedReader](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)
3. [zulu-community](https://www.azul.com/downloads/zulu-community/)

<br />






