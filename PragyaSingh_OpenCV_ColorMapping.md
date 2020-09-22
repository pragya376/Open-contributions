# Color Mapping
Human eyes are not good enough to observe changes in a GRAYSCALE Images speacially small changes in GreyScale intensities. And we are way better at perceiving changes in colors.
Data is better visualized by pseudo-coloring. For Example Temperata Data, Height Data, Pressure Data etc.

**OpenCV defines 12 colormaps that can be applied to a grayscale image.**
So OpenCV use **applyColorMap()** function to produce pesudocolored image.
This Figure shows the visual representation of colormaps. Lower grayscale values are replaced by colors to the left of the scale while higher grayscale values are on the right of the scale.

<img src="https://user-images.githubusercontent.com/64967140/93757182-eee10280-fc23-11ea-9619-3d72f0d1172b.jpg" width="350" height="350" />

*Demo Picture*

<img src="https://user-images.githubusercontent.com/64967140/93757345-36678e80-fc24-11ea-9f4a-63b14410b9aa.jpg" width="200" height="350" />

**Python Code**
```
import cv2
im_gray= cv2.imread("picture.jpg" , cv2.IMREAD_GRAYSCALE)
im_color= cv2.applyColorMap(im_gray, cv2.COLORMAP_HOT)

```

**Ouput** After applying colorMapping.

<img src="https://user-images.githubusercontent.com/64967140/93757655-c86f9700-fc24-11ea-9bf9-bdc341706bd7.jpg" width="200" height="350" />

In the same way we can apply any colormap accordingly.

This Contribution has been made by Pragya Singh.
You can contact me at pragyatomar1611@gmail.com or Ping me at [Linkedln](https://www.linkedin.com/in/pragya-singh-01122017a/)
