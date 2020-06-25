# MY COMPUTER VISION

## OCR Tesseract-4 with Python 3:

Install the requeriments to run on Ubuntu 18.04:
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py

sudo pip install opencv-contrib-python
sudo apt install tesseract-ocr

pip3 install pillow
pip3 install pytesseract
pip3 install imutils
```

Try to import `cv2, pytesseract, imutils`:
```python
$ python
Python 3.6.5 (default, Apr  1 2018, 05:46:30) 
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> import pytesseract
>>> import imutils
>>>
```

Test:
```
python3 ocr.py img_phone/6.jpg
```

## Show Images:
```python
cv2.imshow("Fixed Resizing", resized)
cv2.waitKey(0)
```

## Resizing images
```python
# resize the image to 200x200px, ignoring aspect ratio
resized = cv2.resize(image, (200, 200))
```

Fixing aspect ratio with opencv:
```
# fixed resizing and distort aspect ratio so let's resize the width
# to be 300px but compute the new height based on the aspect ratio
r = 300.0 / w
dim = (300, int(h * r))
resized = cv2.resize(image, dim)
```

Fixing aspect ratio with imutils:
```python
# manually computing the aspect ratio can be a pain so let's use the
# imutils library instead
resized = imutils.resize(image, width=300)
```

## Rotating an image

OpenCV:
```python
# let's rotate an image 45 degrees clockwise using OpenCV by first
# computing the image center, then constructing the rotation matrix,
# and then finally applying the affine warp
center = (w // 2, h // 2)
M = cv2.getRotationMatrix2D(center, -45, 1.0)
rotated = cv2.warpAffine(image, M, (w, h)))
```

Imutils:
```python
# rotation can also be easily accomplished via imutils with less code
rotated = imutils.rotate(image, -45)

# OpenCV doesn't "care" if our rotated image is clipped after rotation
# so we can instead use another imutils convenience function to help
# us out
rotated = imutils.rotate_bound(image, 45)
```

## Smoothing Images
```python
# apply a Gaussian blur with a 11x11 kernel to the image to smooth it,
# useful when reducing high frequency noise
blurred = cv2.GaussianBlur(image, (11, 11), 0)
```

## Drawing on an image

    img
     : The destination image to draw upon. We’re drawing on output
     .
    pt1
     : Our starting pixel coordinate which is the top-left. In our case, the top-left is (320, 60)
     .
    pt2
     : The ending pixel — bottom-right. The bottom-right pixel is located at (420, 160)
     .
    color
     : BGR tuple. To represent red, I’ve supplied (0 , 0, 255)
     .
    thickness
     : Line thickness (a negative value will make a solid rectangle). I’ve supplied a thickness of 2
     .

Rectangle:
```python
# draw a 2px thick red rectangle surrounding the face
output = image.copy()
cv2.rectangle(output, (320, 60), (420, 160), (0, 0, 255), 2)
```

Line:
```python
# draw a 5px thick red line from x=60,y=20 to x=400,y=200
output = image.copy()
cv2.line(output, (60, 20), (400, 200), (0, 0, 255), 5)
```

Text:
```python
# draw green text on the image
output = image.copy()
cv2.putText(output, "OpenCV + Jurassic Park!!!", (10, 25), 
	cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 0), 2)
```

## Arguments Parser
```python
# import the necessary packages
import argparse
import imutils
import cv2

# construct the argument parser and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--image", required=True,
	help="path to input image")
args = vars(ap.parse_args())
```

## Converting an image to grayscale
```python
image = cv2.imread(args["image"])
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

## Edge detection

    img
     : The gray
      image.
    minVal
     : A minimum threshold, in our case 30
     .
    maxVal
     : The maximum threshold which is 150
      in our example.
    aperture_size
     : The Sobel kernel size. By default this value is 3
      and hence is not shown on Line 25.

```python
edged = cv2.Canny(gray, 30, 150)
```

## Thresholding
```python
thresh = cv2.threshold(gray, 225, 255, cv2.THRESH_BINARY_INV)[1]
```

## Detecting and drawing contours
```python
# find contours (i.e., outlines) of the foreground objects in the
# thresholded image
cnts = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,
	cv2.CHAIN_APPROX_SIMPLE)
cnts = imutils.grab_contours(cnts)
output = image.copy()

# loop over the contours
for c in cnts:
	# draw each contour on the output image with a 3px thick purple
	# outline, then display the output contours one at a time
	cv2.drawContours(output, [c], -1, (240, 0, 159), 3)
	cv2.imshow("Contours", output)
	cv2.waitKey(0)
	
# draw the total number of contours found in purple
text = "I found {} objects!".format(len(cnts))
cv2.putText(output, text, (10, 25),  cv2.FONT_HERSHEY_SIMPLEX, 0.7,
	(240, 0, 159), 2)
cv2.imshow("Contours", output)
cv2.waitKey(0)
```
