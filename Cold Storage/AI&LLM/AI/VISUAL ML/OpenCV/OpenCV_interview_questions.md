# OpenCV Interview Questions

### 1. What is OpenCV?
**Answer:** OpenCV (Open Source Computer Vision Library) is a library of programming functions mainly aimed at real-time computer vision. It is used for a wide range of applications, including image and video analysis, object detection, and face recognition.

### 2. How do you read and display an image in OpenCV?
**Answer:** You can read an image using `cv2.imread()` and display it using `cv2.imshow()`. You also need to use `cv2.waitKey()` to pause the execution and `cv2.destroyAllWindows()` to close the window.

'''python
import cv2
img = cv2.imread('image.jpg')
cv2.imshow('Image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
'''

### 3. What is the difference between BGR and RGB color spaces?
**Answer:** OpenCV reads images in BGR (Blue, Green, Red) format by default, while many other image processing libraries use RGB (Red, Green, Blue). It's important to be aware of this when working with other libraries, as you may need to convert between the two. You can convert a BGR image to RGB using `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`.

### 4. What is image thresholding?
**Answer:** Image thresholding is a simple form of image segmentation. It's a way of partitioning an image into a foreground and background. The most common type is binary thresholding, where each pixel is set to a maximum value (e.g., 255) if it's above a certain threshold, and to a minimum value (e.g., 0) if it's below.

### 5. What is the Canny edge detector?
**Answer:** The Canny edge detector is a popular edge detection algorithm. It involves several steps:
1.  **Noise reduction:** Using a Gaussian filter.
2.  **Finding intensity gradients:** Using a Sobel filter.
3.  **Non-maximum suppression:** To thin out the edges.
4.  **Hysteresis thresholding:** To connect the edges.

### 6. What are morphological operations?
**Answer:** Morphological operations are a set of operations that process images based on shapes. The most common ones are:
*   **Erosion:** It erodes away the boundaries of the foreground object. It's used to diminish the features of an image.
*   **Dilation:** It increases the object area. It's used to accentuate features.
*   **Opening:** Erosion followed by dilation. It's useful for removing noise.
*   **Closing:** Dilation followed by erosion. It's useful for closing small holes inside the foreground objects.

### 7. How can you detect faces in an image using OpenCV?
**Answer:** OpenCV provides pre-trained Haar cascade classifiers for face detection. You can load the classifier using `cv2.CascadeClassifier()` and then use the `detectMultiScale()` method to detect faces in an image.

### 8. What are contours?
**Answer:** Contours can be explained simply as a curve joining all the continuous points (along the boundary), having the same color or intensity. The contours are a useful tool for shape analysis and object detection and recognition.

### 9. How do you capture video from a camera?
**Answer:** You can use the `cv2.VideoCapture()` object. Passing `0` as an argument will use the default camera. You can then read frames from the camera in a loop using the `read()` method.

### 10. What is the difference between `cv2.resize()` and `cv2.warpAffine()`?
**Answer:**
*   `cv2.resize()` is used for simple scaling of an image.
*   `cv2.warpAffine()` is used for more complex transformations, such as translation, rotation, and shearing. It takes a 2x3 transformation matrix as input.
