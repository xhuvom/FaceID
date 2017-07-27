![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

#Darknet#
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

For more information see the [Darknet project website](http://pjreddie.com/darknet).

For questions or issues please use the [Google Group](https://groups.google.com/forum/#!forum/darknet).

----------------------YOLO Facial recognition on Darknet Framework-------------------------------------------------

############## Detecting and recognizing face is a three step process with automatic annotation #####################
Fork on github: https://github.com/xhuvom/darknetFaceID
YOLO darknet implementation to detect, recognize and track multiple faces. Yes it can detect and recognize individual faces just by training on different classes. The algorithm automatically learn facial features itself and recognize individual faces. All you need is to train different face images as different classes.
I have tested 3 different faces trained with ~2k individual images per class. After about 60k epochs, the algorithm works pretty well with acceptable accuracy. See a demo video ( https://www.youtube.com/watch?v=UsOi1BfunnU )

Annotating large number of images manually by hand is time-consuming and inefficient for practical prototyping. Thats why I have used the fork https://github.com/quanhua92/darknet/ to detect faces from webcam images and annotate  any number of images automatically.

Basically its a simple three step process::
1. Capture
2. Train
3. Deploy

# Part 1: Capture >>
[i] To detect face from live camera feed and annotate automatically, use the .cfg and .weight files from QuanHua (https://mega.nz/#F!GRV1XKbJ!v8BCsFO8iJVNppiGXY4qMw). 
[ii] Only add those lines on src/image.c file of this fork as described bellow:

(line #223) to save .jpg images and (line #227) to save annotations on separate folders for each class (also change class number on line #229 

[iii] After modifications, run the detector from live webcam or video file which specifically shows only one particular persons face. 
[iv] Repeat the process for every persons you want to recognize and modify training data location and class number accordingly.
About ~2k face images per person is enough to recognize individual faces but to improve accuracy, more data could be added.


## Part 2: Train>>
After capturing each persons face images and annotations on separate training folders, some data preprocessing is required for training. 
Image conversion: Convert jpg images to JPEG for Darknet framework using command [ $ mogrify -format JPEG *jpg ] according to your image data directory.

Label conversion: Convert annotations to VOC data format with scripts/convert.py script provided on scripts folder. This operation generates training image list file on the same folder for different classes. Add all those training list files into one file and point the file on cfg/face.data 

After preprocessing, modify class numbers accordingly, create data/face.names and cfg/face.data files with your desired labels and directories.

Configure src/yolo.c file and yolo_kernels, comment the lines on src/image.c file.
Now OPENCV=0 and start training with cfg file(modify #224 with filters and class numbers according to the equation > filters = (class+coord+1)*num
Now start training on GPU. 

### Part 3 >> Deploy
After about 120k training epochs, the training weight files now should successfully detect and recognize individual faces with acceptable accuracy.

The same process could be used to recognize facial expressions (demo https://www.youtube.com/watch?v=GMy0Zs8LX-o). The only thing I have added here is the automatic annotation of face images, which is quite cumbersome if done by hand.
Let me know your successful training stories or mail me for further clarification at abushuvom@gmail.com
