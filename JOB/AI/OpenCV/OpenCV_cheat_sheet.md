# OpenCV Cheat Sheet

## Core Operations

*   **Read an image:**
    '''python
    import cv2
    img = cv2.imread('image.jpg')
    '''
*   **Display an image:**
    '''python
    cv2.imshow('Image', img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
    '''
*   **Save an image:**
    '''python
    cv2.imwrite('new_image.jpg', img)
    '''
*   **Get image properties:**
    '''python
    height, width, channels = img.shape
    '''

## Basic Image Processing

*   **Convert color space:**
    '''python
    gray_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    '''
*   **Resize an image:**
    '''python
    resized_img = cv2.resize(img, (new_width, new_height))
    '''
*   **Apply thresholding:**
    '''python
    ret, binary_img = cv2.threshold(gray_img, 127, 255, cv2.THRESH_BINARY)
    '''
*   **Blurring/Smoothing:**
    '''python
    gaussian_blur = cv2.GaussianBlur(img, (5, 5), 0)
    median_blur = cv2.medianBlur(img, 5)
    '''
*   **Edge Detection (Canny):**
    '''python
    edges = cv2.Canny(gray_img, 100, 200)
    '''
*   **Morphological Operations:**
    '''python
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5,5))
    eroded_img = cv2.erode(img, kernel, iterations=1)
    dilated_img = cv2.dilate(img, kernel, iterations=1)
    '''

## Drawing and Annotations

*   **Draw a line:**
    '''python
    cv2.line(img, (x1, y1), (x2, y2), (0, 255, 0), 2) # Green line, thickness 2
    '''
*   **Draw a rectangle:**
    '''python
    cv2.rectangle(img, (x1, y1), (x2, y2), (255, 0, 0), -1) # Blue filled rectangle
    '''
*   **Draw a circle:**
    '''python
    cv2.circle(img, (center_x, center_y), radius, (0, 0, 255), 3) # Red circle, thickness 3
    '''
*   **Put text on an image:**
    '''python
    cv2.putText(img, 'Hello OpenCV', (50, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 255), 2)
    '''

## Video Processing

*   **Capture video from camera/file:**
    '''python
    cap = cv2.VideoCapture(0) # 0 for default camera, or 'video.mp4'
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        cv2.imshow('Frame', frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    cap.release()
    cv2.destroyAllWindows()
    '''

## Contours and Object Detection

*   **Find contours:**
    '''python
    contours, hierarchy = cv2.findContours(binary_img, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    '''
*   **Draw contours:**
    '''python
    cv2.drawContours(img, contours, -1, (0, 255, 0), 3)
    '''
*   **Face Detection (Haar Cascades):**
    '''python
    face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
    faces = face_cascade.detectMultiScale(gray_img, 1.1, 4)
    for (x, y, w, h) in faces:
        cv2.rectangle(img, (x, y), (x+w, y+h), (255, 0, 0), 2)
    '''
