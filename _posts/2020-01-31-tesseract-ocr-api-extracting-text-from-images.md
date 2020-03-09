---
layout: post
title: Tesseract OCR C++ API, extracting text from images
tags: [Linux, Google, C++, Data-Science, OOP]
---

In a [previous post](/blog/compiling-tesseract-ocr-development-version-with-lstm-engine/), we compiled all the necessary libraries to setup the `Tesseract OCR C++ API`. In this post we aim to experiment with the basic capabilities of the API. We use the following code snippet:

{% highlight cpp linenos %}

#include <tesseract/baseapi.h>
#include <leptonica/allheaders.h>

int main(int argc, char **argv)
{
    char *text;
    Pix *image;
    
    if (argc != 2){
        fprintf(stderr, "Expected the image path as parameter.\n");
        exit(1);
    }

    // get an instance of tesseract API
    tesseract::TessBaseAPI *api = new tesseract::TessBaseAPI();
    if (api -> Init(NULL, "eng")){
        printf("Error Init API.\n");
        exit(1);
    }

    // uses leptonica to open the image in path argv[1]
    image = pixRead(argv[1]);
    if (!image){
        fprintf(stderr, "Error reading image.\n");
        exit(1);
    }
    // set image and extract text
    api -> SetImage(image);
    text = api -> GetUTF8Text();
    printf("Extracted Text:\n\n%s", text);
    
    // free memory
    api -> End();
    delete api;
    delete [] text;
    pixDestroy(&image);
    
    return 0;
}

{% endhighlight %}


To compile the code above in `Linux` we use:

```
$ g++ -o extract_text extract_text.cpp -llept -ltesseract -std=gnu++11
```

If the libraries were installed in a non-default path, the compiler options `-I` and `-L` can be used, see `$ man g++`. In this example we have to specify the `C++` standard required by the Tesseract API, we use option `-std=gnu++11`.

To execute the program we need to specify the image to scan as a parameter. We first try with one of the images provided in the [tests repository](https://github.com/tesseract-ocr/test).

```
$ ./extract_text test/testing/phototest.tif
Extracted Text:

This is a lot of 12 point text to test the
ocr code and see if it works on all types
of file format.

The quick brown dog jumped over the
lazy fox. The quick brown dog jumped
over the lazy fox. The quick brown dog
jumped over the lazy fox. The quick
brown dog jumped over the lazy fox.
```

Then we use our own image with the following handwriting text:

<img src="/img/handwriting-test-ocr.png" alt="ocrownimg" width="400" />

```
$ ./extract_text handwriting-test-ocr.png
Extracted Text:

Flblo LUrtd |
Hello my N4nC *© Baldo
4e llo (Werld 1
```

We can observe from the experiment above that the trained model struggles to recognize handwriting, especially cursive style. Furthermore, the quality of the results will depend on how clean the input image is.


References:

- [1][tessdoc](https://tesseract-ocr.github.io/tessdoc/Home.html)



 

 






