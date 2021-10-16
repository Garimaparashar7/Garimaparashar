# InstagramStyleFilter 
import numpy as np
import cv2
import matplotlib.pyplot as plt
from scipy.interpolate import UnivariateSpline
import matplotlib.image as mpimg
%matplotlib inline
path='rbow.jpg'
img=mpimg.imread(path)
plt.imshow(img)
# create maping function
def mapping_function(x,y):
    spl=UnivariateSpline(x,y)
    return spl(range(256))
def apply_warm(image):
    increase=mapping_function([0,64,128,192,256],[0,70,140,210,256])
    decrease=mapping_function([0,64,128,192,256],[0,40,90,150,256])
    red,green,blue=cv2.split(image)
    red=cv2.LUT(red,increase).astype(np.uint8)
    blue=cv2.LUT(blue,decrease).astype(np.uint8)
    image=cv2.merge((red,green,blue))
    return image
def apply_cool(image):
    increase=mapping_function([0,64,128,192,256],[0,70,140,210,256])
    decrease=mapping_function([0,64,128,192,256],[0,40,90,150,256])
    red,green,blue=cv2.split(image)
    red=cv2.LUT(red,decrease).astype(np.uint8)
    blue=cv2.LUT(blue,increase).astype(np.uint8)
    image=cv2.merge((red,green,blue))
    return image
new_image=apply_warm(img)
plt.imshow(new_image)
new_image=apply_cool(img)
plt.imshow(new_image)
from ipywidgets import interact,interactive,fixed
import ipywidgets as widgets
def choice(x,img):
    if x=='warm':
        return plt.imshow(apply_warm(img))
    if x=='cool':
        return plt.imshow(apply_cool(img))
    if x=='No Filter':
        return plt.imshow(img)
interact(choice,x = widgets.Dropdown(Options=['No Filter','warm','cool'],description="Filter"),img=fixed(img)); 
