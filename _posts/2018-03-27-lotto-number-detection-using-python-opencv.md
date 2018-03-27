---
layout: post
published: true
date: '2018-03-27 18:04 +0900'
modified: '2018-03-27 18:04 +0900'
category: base
tags:
  - default
imagefeature: sitelogo.png
show_meta: true
comments: true
mathjax: false
gistembed: false
noindex: false
hide_printmsg: false
sitemap: true
summaryfeed: false
title: Lotto Number Detection using Python & OpenCV
description: ''
---
## Base

OpenCV version: 3.4.1
Python version: 3


```
import numpy as np
import cv2
```

## Get image

```
img = cv2.imread('path-to-imgae')
```
 
## Show image in window
```
cv2.namedWindow('image', cv.WINDOW_NORMAL)
cv2.imshow('image', img)
```

## Close window
```
cv2.waitKey(0) # Wait for key input
cv2.destroyAllWindows() # Close all windows
```


## Gaussian Smoothing
A technique for reducing noise in an image by averaging each pixel with its surrounding pixels.









