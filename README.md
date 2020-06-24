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
