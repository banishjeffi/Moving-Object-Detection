# Moving-Object-Detection
---
## 💫 About Developer:

### Banish J

🤖 AI & ML Developer | Data Science & Analytics Expert  
🌐 Web Apps & IoT Skilled | Awesome UI Creator  
💻 AI is my main focus! 👾

## 📞 Contact
##### **☎️**   [9444333914](tel:9444333914)
##### **📧**  [mail@banish.in](mailto:mail@banish.in)
##### **🌐**  [Banish](https://www.banish.in)

## Core Concept

***This code captures video from the webcam, processes each frame to detect motion by comparing the current frame with the initial frame, highlights the areas of detected motion, and displays the results in a window. If motion is detected, it prints "Motion Detected" and draws a green rectangle around the moving object. The video feed is displayed in a window, and the program can be terminated by pressing the 'q' key.***

## Importing Necessary Libraries
```python
import cv2
import time
import imutils
```
- **cv2**: The OpenCV library for computer vision tasks.
- **time**: A module to handle time-related tasks.
- **imutils**: A convenience library for basic image processing functions, making it easier to work with OpenCV.

### Initializing the Webcam and Waiting
```python
cam = cv2.VideoCapture(0)
time.sleep(1)
```
- **cv2.VideoCapture(0)**: Initializes the webcam for video capture. The argument `0` typically refers to the default webcam on the computer.
- **time.sleep(1)**: Introduces a delay of 1 second to allow the webcam to warm up.

### Defining Initial Variables
```python
firstFrame = None
area = 500
```
- **firstFrame**: A variable to store the first frame captured by the webcam, which will be used as a reference frame for detecting motion.
- **area**: The minimum area threshold for a detected contour to be considered significant (to filter out small movements or noise).

### Real-Time Motion Detection Loop
```python
while True:
    _, img = cam.read()
    text = "Normal"

    img = imutils.resize(img, width=500)
    grayImg = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    gaussianImg = cv2.GaussianBlur(grayImg, (21, 21), 0)
```
- **cam.read()**: Captures a frame from the webcam. `_` is used to ignore the first return value (a boolean indicating if the frame was read correctly), and `img` stores the captured image.
- **text = "Normal"**: Initializes a text variable to "Normal" indicating no motion detected.
- **imutils.resize(img, width=500)**: Resizes the frame to a width of 500 pixels while maintaining the aspect ratio.
- **cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)**: Converts the captured color image to grayscale.
- **cv2.GaussianBlur(grayImg, (21, 21), 0)**: Applies a Gaussian blur to the grayscale image to reduce noise and smooth the image.

### Setting Up the First Frame
```python
    if firstFrame is None:
        firstFrame = gaussianImg
        continue
```
- Checks if `firstFrame` is `None`. If it is, assigns the current blurred grayscale image to `firstFrame` and skips to the next iteration of the loop.

### Computing the Difference and Thresholding
```python
    imgDiff = cv2.absdiff(firstFrame, gaussianImg)
    threshImg = cv2.threshold(imgDiff, 25, 255, cv2.THRESH_BINARY)[1]
    threshImg = cv2.dilate(threshImg, None, iterations=2)
```
- **cv2.absdiff(firstFrame, gaussianImg)**: Computes the absolute difference between the first frame and the current frame.
- **cv2.threshold(imgDiff, 25, 255, cv2.THRESH_BINARY)[1]**: Applies a binary threshold to the difference image. Pixels with a value greater than 25 are set to 255 (white), and others are set to 0 (black).
- **cv2.dilate(threshImg, None, iterations=2)**: Dilates the threshold image to fill in small holes and gaps.

### Finding Contours and Detecting Motion
```python
    cnts = cv2.findContours(threshImg.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    cnts = imutils.grab_contours(cnts)
    for c in cnts:
        if cv2.contourArea(c) < area:
            continue
        (x, y, w, h) = cv2.boundingRect(c)
        cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)
        text = "Motion Detected"
```
- **cv2.findContours(threshImg.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)**: Finds contours in the threshold image.
- **imutils.grab_contours(cnts)**: Grabs the correct contours list based on the version of OpenCV.
- Iterates over the detected contours.
  - **cv2.contourArea(c) < area**: Ignores contours with an area smaller than the defined threshold.
  - **cv2.boundingRect(c)**: Computes the bounding box coordinates for the contour.
  - **cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)**: Draws a rectangle around the detected contour.
  - **text = "Motion Detected"**: Updates the text variable to indicate motion detection.

### Displaying the Result
```python
    print(text)
    cv2.putText(img, text, (10, 20), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)
    cv2.imshow("camraFeed", img)
```
- **print(text)**: Prints the motion detection status.
- **cv2.putText(img, text, (10, 20), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)**: Puts the motion detection status text on the frame.
- **cv2.imshow("camraFeed", img)**: Displays the frame with motion detection status.

### Handling Key Press and Releasing Resources
```python
    key = cv2.waitKey(10)
    print(key)
    if key == ord("q"):
        break
cam.release()
cv2.destroyAllWindows()
```
- **cv2.waitKey(10)**: Waits for 10 milliseconds for a key press.
- **if key == ord("q")**: Checks if the 'q' key is pressed to break the loop and stop the program.
- **cam.release()**: Releases the webcam resource.
- **cv2.destroyAllWindows()**: Closes all OpenCV windows.

---
