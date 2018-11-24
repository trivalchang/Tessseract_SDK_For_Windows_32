# How to use Tesseract OCR APIs in QT on Microsoft Windows
Recently I am building a image recognition program with Tesseract OCR by QT. Howerver I found there is no Tesseract SDK for windows available. I did some research and this is how to make it work.

## Using the official release of Tesseract 
This is a simple method that I recommend. By following https://github.com/tesseract-ocr/tesseract/wiki, Tesseract will be installed with the shared libraries such as libtesseract-4.dll. These libraies are actually what you need to build your own application.
However, you still need header files. 

In this repository, shared libraries are from Tesseract official release and the header files are compiled by myself. Now you don't have to generate these by yourself. Just download it.

Suppose this repository is placed in "C:\Tesseract_SDK". To use the below code segment, 

```
#include <baseapi.h>
    cv::cvtColor(gray, gray, COLOR_RGB2GRAY);
    tesseract::TessBaseAPI tess;
    if (tess.Init("G:\\venus\\release\\tessdata", "eng", tesseract::OEM_DEFAULT) == -1)
    {
        printf("Unable to init\n");
        qInfo() << "Unable to init\n";
    }
    tess.SetImage((uchar*)gray.data, gray.cols, gray.rows, 1, gray.cols);
    char* out = tess.GetUTF8Text();
````
We need to add the include path in .pro file.

```
INCLUDEPATH += "C:\Tesseract_SDK\include\tesseract"
```

You have to copy zlib1.dll to your project working directory. This is because there are many different zlib1.exe in windows system. You might link to the wrong one. 

Then add tesseract lib to our .pro.
```
LIBS += "C:\Tesseract_SDK\lib\libtesseract-4.dll"
```

Now QT Creator shall be able to compile and link successfully. 
But if you execute it in QT creator, it will crash and the application output will show "The program has unexpectedly finished". This is because the library path is not set. 
In QT Creator, enter projects Build&Run setting, add **C:\Tesseract_SDK\lib** to the end of **PATH**. If you want to execute your program outside QT creator, add to windows system environment variable **PATH**.

Now it's done!
