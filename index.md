```C++
#include <iostream>
#include <windows.h>
#include <opencv2/opencv.hpp>

int main()
{
    typedef void*(*Create)(const char *);
    typedef void(*Correct)(void *, unsigned char *, unsigned char *);
    HINSTANCE hInstLibrary = LoadLibrary(L"boardEye.dll");

    Create create = (Create)GetProcAddress(hInstLibrary, "createCorrector");
    Correct correct = (Correct)GetProcAddress(hInstLibrary, "correct");

    cv::Mat inputImg = cv::imread("../../var/test.jpg");
    cv::Mat outputImg(800, 1600, CV_8UC3);

    void* corrector = create("../../var/param.yml");
    correct(corrector, inputImg.data, outputImg.data);
    cv::imwrite("../../var/result.jpg", outputImg);

    FreeLibrary(hInstLibrary);

    return 0;
}
```
