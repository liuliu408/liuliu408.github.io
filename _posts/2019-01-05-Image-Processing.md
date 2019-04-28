---
layout: post
title:  "Image-Processing"
date:   2019-01-05 09:50:58 +0800
categories: jekyll update
---
Image-Processing

# 归一化RGB
   RGB颜色模型在图像处理中是比较常用的格式，但其有个缺点就是容易受到光照变化或阴影的影响。因此，在进行图像处理过程中，通常会对RGB进行归一化，以便消除其对部分光照的影响。
## 为什么归一化RGB能够消除部分光照变化的影响？
举个例子：
```shell
归一化前：
T1时刻的像素A的像素值为：RGB(30, 60, 90)
T2时刻的像素A的像素值为：RGB(60, 120, 180)  （受光照影响，R/G/B三个颜色通道的value产生了变化）

归一化公式：
r = R / (R+G+B) ;
g = B / (R+G+B) ;
b = 1 - r - g ;

归一化后：
T1时刻的像素A的像素值为：rgb(1/6, 1/3, 2/3)
T2时刻的像素A的像素值为：rgb(1/6, 1/3, 2/3)
T1和T2时刻的归一化RGB的值没有发生变化。
可以看到，归一化RGB的好处在于，当某个像素受光照或阴影的影响而产生颜色通道R、G、B上的scale变化的话，则通过归一化操作，可以消除这样的影响。

说明：
1）归一化RGB对于灰色（R=G=B）或黑色像素存在问题；
   原因：当某个像素的R=G=B时，如果由于光照变化影响其R/G/B三个通道值分别发生了变化，但是变化后值仍然为：R' = G' = B'，那么对它们的归一化是不起作用的 （由归一化公式可知）。
   
2）归一化RGB并不能去除所有类型的光照或阴影产生的影响。
   原因：由归一化RGB的公式可知，其只对R/G/B三个通道值发生scale变化（即 scale = R'/R = G'/G = B'/B）的情况时具有光照不变性。
```
---

```c++
//代码来源：https://blog.csdn.net/icvpr/article/details/8502929
//图像旋转与缩放 
#include <iostream>
#include <vector>
#include <opencv2/opencv.hpp>
 
int main(int argc, char** argv)
{ 
	cv::Mat image = cv::imread("../test.jpg");
	if (image.empty())
	{
		std::cout<<"read image failure"<<std::endl;
		return -1;
	} 
	cv::Point2f center = cv::Point2f(image.cols / 2, image.rows / 2);  // 旋转中心
	double angle = 30;  // 旋转角度
	double scale = 0.5; // 缩放尺度
 
	cv::Mat rotateMat; 
	rotateMat = cv::getRotationMatrix2D(center, angle, scale);
 
	cv::Mat rotateImg;
	cv::warpAffine(image, rotateImg, rotateMat, image.size());
 
	cv::imwrite("../rotate.jpg", rotateImg); 
	return 0;
}
```
## 知识点
 [图像卷积与滤波的一些知识点](https://blog.csdn.net/zouxy09/article/details/49080029)    
   
   
   
