# How to use Tesseract OCR APIs in QT on Microsoft Windows
Recently I am building a image recognition program with Tesseract OCR by QT. Howerver I found there is no Tesseract SDK for windows available. I did some research and this is how to make it work.

## Using the official release of Tesseract 
This is a simple method that I recommend. By following https://github.com/tesseract-ocr/tesseract/wiki, Tesseract will be installed with the shared libraries such as libtesseract-4.dll. These libraies are actuall what you need to build your own application.
However, you still need header files which you can download here. Let's create a new folder named Tesseract_SDK and we place the tesserace header files under "C:\Tesseract_SDK\include\tesseract".

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
INCLUDEPATH += "C:\Program Files\Tesseract-OCR\include\tesseract"
```

We also need to copy the shared libraries from the installed directory of Tesseract to our SDK folder.
According to my expericence, the following files are necessary.

liblept-5.dll
libtesseract-4.dll
libgif-7.dll
libjpeg-8.dll
libopenjp2.dll
zlib1.dll
libtiff-5.dll
libjbig-2.dll
liblzma-5.dll
libwebp-7.dll
libgcc_s_sjlj-1.dll

Copy these files from Tesseract folder (it is C:\Program Files\Tesseract-OCR on my computer) to C:\Tesseract_SDK\lib.
Then you have to do the following
1. rename libopenjp2.dll libopenjp2-7.dll. This is because liblept-5.dll actually links to libopenjp2-7.dll but libopenjp2.dll. I don't know why.
2. copy zlib1.dll to your project working directory. This is because there are many different zlib1.exe in windows system. You might link to the incorrect one. 

Without Tesseract lib, you will get link error. So we have to add tesseract to our .pro.
```
LIBS += "C:\Tesseract_SDK\lib\libtesseract-4.dll"
```

Now QT Creator shall be able to compile and link successfully. 
But if you execute it in QT creator, it will crash and the application output will show "The program has unexpectedly finished". This is because the library path is not set. 
In QT Creator, enter projects Build&Run setting, add **C:\Tesseract_SDK\lib** to the end of **PATH**. If you want to execute your program outside QT creator, add to windows system environment variable **PATH**.

Now it's done!
