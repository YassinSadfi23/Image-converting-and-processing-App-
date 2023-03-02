
//CODE_Yassin_Sadfi

# Libraries
   
    from PyQt5 import QtWidgets, uic
    from PyQt5.QtGui import QPixmap
    from PIL import Image
    from PyQt5.QtCore import Qt
    import cv2
    from matplotlib import pyplot as plt
    import numpy as np

# importing Images

def importing():
    global path
    
    image = QtWidgets.QFileDialog.getOpenFileName()
    
    path = image[0]
    
    c=str(path)
    
    w.txt.setText(str(path))
    
    if c[-3:len(c)].upper()!="JPG" and c[-3:len(c)].upper()!="PNG" :
    
        w.txt.setText("EROOOOOOR le fichier doit etre une IMAGE AAYCHEK !!")



# Histogram_function

def histo():
    
    img = cv2.imread(path)
    Image_Height = img.shape[0]
    Image_Width = img.shape[1]
    Image_Channels = img.shape[2]

    Histogram = np.zeros([256, Image_Channels], np.int32)

    for x in range(0, Image_Height):
        for y in range(0, Image_Width):
            for c in range(0, Image_Channels):
                Histogram[img[x, y, c], c] += 1

    plt.title("Color Image Histogram")
    plt.xlabel("Intensity Level")
    plt.xlim([0, 256])
    plt.plot(Histogram[:, 0], 'b')
    plt.plot(Histogram[:, 1], 'g')
    plt.plot(Histogram[:, 2], 'r')
    plt.savefig("figure.jpg")
    
    q = QPixmap("figure.jpg")
    # resize
    size = w.histo.size()
    scaled = q.scaled(size, Qt.KeepAspectRatio)
    w.histo.setPixmap(scaled)


# slider_Function

def slide_contour():
    img = Image.open(path)
    img=img.convert("L")
    #threshold
    thresh= w.slider.value()
    img=img.point(lambda p:255 if p > thresh else 0)
    # to mono
    img=img.convert("1")
    # save img
    img.save("ee.jpg")
    q=QPixmap("ee.jpg")
    # resize
    size=w.image2.size()
    scaled = q.scaled(size, Qt.KeepAspectRatio)
    w.image2.setPixmap(scaled)

# to grey
def grey():

    img=Image.open(path)
    greyy=img.convert("L")
    greyy.save("eee.jpg")
    q=QPixmap("eee.jpg")
    # resize
    size = w.image1.size()
    scaled = q.scaled(size, Qt.KeepAspectRatio)
    w.image1.setPixmap(scaled)


# Real_Image
def adiya():
    
    Image.open(path)
    q = QPixmap(path)
    # resize
    size = w.image.size()
    scaled = q.scaled(size, Qt.KeepAspectRatio)
    w.image.setPixmap(scaled)

#   proram principale$
    app =QtWidgets.QApplication([])
    w = uic.loadUi("Interface.ui")
    w.show()
    w.btn.clicked.connect(importing)  #btn is the name of the button I created in framework 
    w.btn3.clicked.connect(slide_contour)
    w.btn2.clicked.connect(grey)
    w.btn1.clicked.connect(adiya)
    w.btnH.clicked.connect(histo)
    w.slider.valueChanged.connect(slide_contour)
    app.exec()
    exit()



