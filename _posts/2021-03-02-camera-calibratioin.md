---
layout: post
title: 相机标定
categories: cv_camera
description: some word here
keywords: camera, calibration,相机,标定
---

### 1 相机标定的定义

任何传感器都存在有误差，狭义的相机标定就是校正这部分误差，让传感器尽量准确一点。

之所以相机厂家不自行标定，是因为相机标定的参数与相机实际的光圈、焦距大小有关。在一般的场景下，相机的光圈和焦距都是由用户自行调节的，因此相机标定是必要的。

广义上来说，相机标定包含——传感器误差的校正、传感器相互之间位置关系标定（只存在于多目相机系统）

### 2 理想单目相机成像模型

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIRWtYVYZHDaMuDkmm7WiaCDLstfJsEiaIXt0EwhrXn257ib1KKPcGTwTVA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIxmibcOK5HlrqGygs73ugh3ZliaujHW0HoKmpKKYPl0oBODAuusSvqcyA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

以针孔相机Oc为例，P是物点，P'是物点经过Oc对应的像点。所谓焦距，即是成像平面到光心的距离。

接下来是理想成像模型。在相机成像模型中，主要有四大坐标系：**世界坐标系**、**相机坐标系**、**图像坐标系**、**像素坐标系**

**世界坐标系（3D）：世界坐标系就是以外界某个参考点建立的坐标系，单位为mm，或者m。**

**相机坐标系（3D）：以光心建立相机坐标系，其中轴指向相机的正前方。**

**图像坐标系（2D）：光心在成像平面的投影，也就是点建立另一坐标系，称为图像坐标系，单位为mm。**

**像素坐标系（2D）：像素坐标系是我们最终用户所看到的，也就是坐标系的原点在图像左上角，单位为像素。**

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI05MIpeiaSggxXKiaHw8hskicibh1eKHicMzIwF5EsoBOCgv0Laf3zqR1onQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

有了这些坐标系，我们可以把成像过程简化为**世界-> 相机 -> 图像 -> 像素**的过程。



#### 2.1 世界坐标系->相机坐标系

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIj0sOZpeGamw7qOSc0t0AQAhlPXSBEAa08ok4QhVcvDbT0vdyRmZhiag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIjAwtTibLVEpOLfRnGsq3RpEwfF1XbSu8rFQQXs2STribSvibLhDxF1X6A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 2.2 相机坐标系 -> 图像坐标系

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI7BKU1ppGEFoEoUfZicQEI51icGhNIpRicJnKVagjY35VSPo4ZJzE1EVQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI2Lx75KaxhjeCGEOKkhvI3FYnhsCcsNqY82Waj4f5Bb7qbibGEd6bXnA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI6BNgr9Q5Ro57Piaz1AicV6KOiaQAQJ650oKU98s0wibkAUfRrpp0bFUTVA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIZqzJco80tDc2yEO9xjGAXVAYZF3FF0F8wq2npghoAuMJ9Dh63lciaFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 2.3 图像坐标系->像素坐标系

在**图像坐标系**中，坐标的原点在图像中间，而我们平时处理图像时用的坐标系，称为**像素坐标系**，坐标系的原点在图像的左上角，此外它们的度量单位也不同，前者的单位是mm，而后者的单位是像素。

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPICazRJHoghPSOBvYxFvic9jwKTPMPaD9taFD6Nz95jDoyqbaovT3gibiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI7zCpfRv35m4EfwwH84O1LO3icycaXdoo4nH66BDCVmh791JRkT69vxw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIHvRh7rzLUffPUthl1vibmP7Vzr2zxFAm1hd0nlJNc8ykwuyibFrLBKEw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 2.4 总结

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPI0iaAelQwPcJial9AHlSE3wdJ9jIKGvHhmbIsLPYqMEMBRjMZXIjYs7RA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPITqnicKQDcMgZocYFrfEja6ZiaaBoZD6IlGZtic0DUso2ZoJ83sS2XW0LQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 3 畸变

#### 3.1 径向畸变

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIjk7GAvvibXCRTsIvYibEfCrQZuBBbvs0z4AkEbfEjFicZDLicYmDYC086g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIFmR0m2bsP66I1AL0R8fia1Sz67QN1sCQBRXWMKianuCphdickdFsicrapQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 3.2 切向畸变

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIPkoPjuOFWCeXpDpvpVXLnOJZiaRk3Sru4KqNpaVRZRXicqhvpCdybn8g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 3.3 总结

![图片](https://mmbiz.qpic.cn/mmbiz_png/Q0FNTB1XHicwVKYlKticS5AS3rQHYsUkPIkrnnSDmyl7kO3oRuToqqVLWs0LHzs0QK0IJHicsiaEWN87bVLdjgD2sw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

