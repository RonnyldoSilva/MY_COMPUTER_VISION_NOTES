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
