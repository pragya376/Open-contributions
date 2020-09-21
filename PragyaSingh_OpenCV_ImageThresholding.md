# Thresholding in OpenCV
Image Thresholding is a way by which a colored image is converted into a binary image.The idea of thresholding is to simplify visual data for analysis. This is also useful in extracting foreground and background objects and also for creating sketch like images. 
There are four techniques:
- Simple Thresholding
- Adaptive Thresholding
- Otsu's Thresholding

## Simple Thresholding
In the Simple thresholding operation the pixels whose values are greater or below a specific threshold value are assigned with a standard value.
So, we can perform threshold operation by using a function **threshold()**.
Here is an example code:
```
img = cv2.imread('Simplethreshold.jpg',0)
ret,thresh1 = cv2.threshold(img,127,255,cv2.THRESH_BINARY)
ret,thresh2 = cv2.threshold(img,127,255,cv2.THRESH_BINARY_INV)
ret,thresh3 = cv2.threshold(img,127,255,cv2.THRESH_TRUNC)
ret,thresh4 = cv2.threshold(img,127,255,cv2.THRESH_TOZERO)
ret,thresh5 = cv2.threshold(img,127,255,cv2.THRESH_TOZERO_INV)

titles = ['Original Image','BINARY','BINARY_INV','TRUNC','TOZERO','TOZERO_INV']
images = [img, thresh1, thresh2, thresh3, thresh4, thresh5]

for i in range(6):
    plt.subplot(2,3,i+1),plt.imshow(images[i],'gray')
    plt.title(titles[i])
    plt.xticks([]),plt.yticks([])

plt.show()
```

Now lets analyse the above code.
```
img = cv2.imread(‘SimpleThreshold.jpg’,0)
```
First parameter is picture, And for second parameter we have three options -1,0,1, which are Unchanged, Grayscale, Color respectively.
Now lets look at the next Function.
```
ret,thresh1 = cv2.threshold(img,127,255,cv2.THRESH_BINARY)
```
Here, **ret** is basically **retVal** which is used in Otsu's Binarization. But here its not used so  ret value will remain same as in the code. And, I will explain **retVal** later in this article.
Next is **threshold** function which has the following parameters:
 - **img** which is a variable defined for image.
 - **127** is the **Threshold Value**, so a function will be applied to any value which is above threshold value.
 - **255** is a **Max value** as range of grayscale is form 0 to 255, 0 means Black and 255 means white.
 ![Grayscale](https://user-images.githubusercontent.com/64967140/93779180-50b16480-fc44-11ea-903a-f84c36c11222.png)
  - Last Parameter is the type of thresholding.
1. THRESH_BINARY:   This turn any value above the threshold into minimum possible value and any value below the threshold into maximum possible value.
2. THRESH_BINARY_INV: This is the opposite of THRESH_BINARY.
3. THRESH_TRUNC: Values higher than threshold are set equal to the threshold value i.e 127, And values lower than threshold remain unchanged.
4. THRESH_TOZERO: In this Values higher than threshold remain unchanged and values lower than threshold are set to zero.
5. THRESH_TOZERO_INV: In this pixels less than threshold value are fixed and higher values are set to zero.
 Below is the Output image:
 
![](https://user-images.githubusercontent.com/64967140/93806167-633c9580-fc66-11ea-9b30-d40919f2d0b8.png)

## Adaptive Thresholding
In Adaptive Thresholding, threshold value is caluclated for smaller regions, therefore there are different threshold value for different regions.
Unlike Simple Thresholding where threshold value is constant for whole image. And , it creates problem if image have some lighting on it due to which some areas become lighter than other.
For example:

**Original Image**

<img src="https://user-images.githubusercontent.com/64967140/93786461-fa94ef00-fc4c-11ea-9918-31256b6ab4cc.gif" width="100" height="100" />

**Image with Simple Thresholding**

<img src="https://user-images.githubusercontent.com/64967140/93786460-f963c200-fc4c-11ea-9523-1dab9d613a39.gif" width="100" height="100" />

**Image with Adaptive Thresholding**

<img src="https://user-images.githubusercontent.com/64967140/93786474-fd8fdf80-fc4c-11ea-84b9-69a1084521bb.gif" width="100" height="100" />

As we can see Adaptive Thresholding did better job than Simple Thresholding.
Now, In OpenCV **adaptiveThreshold()** function is used for AdaptiveThresholding. And it has one extra parameter than Simple Thresholding i.e **adaptiveMethod()** Now there are two types of Adaptive method:
1. **ADAPTIVE_THRESH_MEAN_C** In this method threshold value at pixel(x,y) is calculated by calculating the mean pixel value for the blockSize which is then multiplied by the mean pixel value of the neighboring blockSize pixel(x,y) minus some constant C.
2. **ADAPTIVE_THRESH_GAUSSIAN_C** In this method threshold value at pixel(x,y) is the Gaussian weighted sum of the neighborhood values minus some constant C.

**Code**
```
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
img = cv.imread('Adaptive.jpg',0)
img = cv.medianBlur(img,5)
ret,th1 = cv.threshold(img,127,255,cv.THRESH_BINARY)
th2 = cv.adaptiveThreshold(img,255,cv.ADAPTIVE_THRESH_MEAN_C,\
            cv.THRESH_BINARY,11,2)
th3 = cv.adaptiveThreshold(img,255,cv.ADAPTIVE_THRESH_GAUSSIAN_C,\
            cv.THRESH_BINARY,11,2)
titles = ['Original Image', 'Global Thresholding (v = 127)',
            'Adaptive Mean Thresholding', 'Adaptive Gaussian Thresholding']
images = [img, th1, th2, th3]
for i in range(4):
    plt.subplot(2,2,i+1),plt.imshow(images[i],'gray')
    plt.title(titles[i])
    plt.xticks([]),plt.yticks([])
plt.show()
```
**Output**

![](https://user-images.githubusercontent.com/64967140/93807127-d1358c80-fc67-11ea-9644-8675f43a27ce.png)


## Otsu's Thresholding
In this Threshold value is found automatically which will work best for the image. This works best when we have many pictures taken with varying lighting and also manual determination of threshold value is very time consuming.
And **retVal** is used in otsu's thresholding. It replaces the global thresholding value by the determined by Otsu's Binarization.This only works when Ostu’s binarization is used.
**Code**
```
import cv2
import numpy as np
from matplotlib import pyplot as plt
 
img = cv2.imread('leaf.jpg',0)
 
ret, imgf = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY+cv2.THRESH_OTSU)
 
plt.subplot(3,1,1), plt.imshow(img,cmap = 'gray')
plt.title('Original Noisy Image'), plt.xticks([]), plt.yticks([])
plt.subplot(3,1,2), plt.hist(img.ravel(), 256)
plt.axvline(x=ret, color='r', linestyle='dashed', linewidth=2)
plt.title('Histogram'), plt.xticks([]), plt.yticks([])
plt.subplot(3,1,3), plt.imshow(imgf,cmap = 'gray')
plt.title('Otsu thresholding'), plt.xticks([]), plt.yticks([])
plt.show()
```

**Output**

![](https://user-images.githubusercontent.com/64967140/93807372-283b6180-fc68-11ea-9706-9ccea9cca970.png)


You can contact me on pragyatomar1611@gmail.com for any query.
Or ping me on ![Linkedln](https://www.linkedin.com/in/pragya-singh-01122017a/)











  
 
