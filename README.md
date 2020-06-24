# MY COMPUTER VISION NOTES

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

## Resizing images

```python
# resize the image to 200x200px, ignoring aspect ratio
resized = cv2.resize(image, (200, 200))
cv2.imshow("Fixed Resizing", resized)
cv2.waitKey(0)
```

Fixing aspect ratio with opencv:
```
# fixed resizing and distort aspect ratio so let's resize the width
# to be 300px but compute the new height based on the aspect ratio
r = 300.0 / w
dim = (300, int(h * r))
resized = cv2.resize(image, dim)
cv2.imshow("Aspect Ratio Resize", resized)
cv2.waitKey(0)
```

Fixing aspect ratio with imutils:
```python
# manually computing the aspect ratio can be a pain so let's use the
# imutils library instead
resized = imutils.resize(image, width=300)
cv2.imshow("Imutils Resize", resized)
cv2.waitKey(0)
```

## Rotating an image

OpenCV:
```python
# let's rotate an image 45 degrees clockwise using OpenCV by first
# computing the image center, then constructing the rotation matrix,
# and then finally applying the affine warp
center = (w // 2, h // 2)
M = cv2.getRotationMatrix2D(center, -45, 1.0)
rotated = cv2.warpAffine(image, M, (w, h))
cv2.imshow("OpenCV Rotation", rotated)
cv2.waitKey(0)
```

Imutils:
```python
# rotation can also be easily accomplished via imutils with less code
rotated = imutils.rotate(image, -45)
cv2.imshow("Imutils Rotation", rotated)
cv2.waitKey(0)

# OpenCV doesn't "care" if our rotated image is clipped after rotation
# so we can instead use another imutils convenience function to help
# us out
rotated = imutils.rotate_bound(image, 45)
cv2.imshow("Imutils Bound Rotation", rotated)
cv2.waitKey(0)
```

## Smoothing Images
```python
# apply a Gaussian blur with a 11x11 kernel to the image to smooth it,
# useful when reducing high frequency noise
blurred = cv2.GaussianBlur(image, (11, 11), 0)
cv2.imshow("Blurred", blurred)
cv2.waitKey(0)
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
```python
# draw a 2px thick red rectangle surrounding the face
output = image.copy()
cv2.rectangle(output, (320, 60), (420, 160), (0, 0, 255), 2)
cv2.imshow("Rectangle", output)
cv2.waitKey(0)
```

