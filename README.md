# YoloV4_colab_tutorial
This is tutorial for training and testing YOLOv4 on Colab. 
## Create new Colab notebook
* Open Google Colab https://colab.research.google.com/ and create new note book, name it whatever you want.
* Mount drive: It's optional step, but you should do it to save your project to Google Drive for using later.
* In this tutorial, I use my Goolge Drive to store the project.
## Prepair YOLOv4
Excute below command to come to project's directory
```bash
%cd /content/drive/My Drive/Colab Notebooks/YOLOv4
```
Clone YOLOv4
```bash
!git clone https://github.com/AlexeyAB/darknet
```
After cloning, you will have a new sub directory named `darknet` in `/content/drive/My Drive/Colab Notebooks/YOLOv4` directory. Then come to the sub directory:
```bash
%cd darknet
```
Change makefile to have GPU and OPENCV enabled
```bash
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile
!sed -i 's/CUDNN_HALF=0/CUDNN_HALF=1/' Makefile
```
Then build darknet by run make command
```bash
!make
```
## Prepair dataset
I use [KAIST dataset](https://soonminhwang.github.io/rgbt-ped-detection/data/) for this tutorial. The dataset consist of 4 classes (person, people, cyclist, person?).
By convenience, I download and save the dataset to `darknet/build/x64/data/obj`.
To quick download the dataset, use `wget` command:
```bash
```
Then, download
```bash
%cd /content/drive/My Drive/Colab Notebooks/YOLOv4/darknet/build/x64/data/obj
!wget https://link_to_each_sub_set_of_KAIST_dataset
```
Download annotation zip file:
```bash
!wget https://link_to_annotation_zip_file
```
After downloading, the `obj` directory should contain 11 zip files:
```bash
---obj
------annotations.zip
------set00.zip
------set01.zip
------set02.zip
------set03.zip
------set04.zip
------set05.zip
...
------set11.zip
```
Extract zip files
```bash
!unzip *.zip
```
After extracting, remove zip file to save disk.
```bash
!rm -r *.zip
```

Finally, the `obj` directory would be following:
```bash
---obj
------set00
------set01
------set02
------set03
------set04
------set05
...
------set11
```
If you want to train the model on color images only, remove thermal images to save your disk.
```bash
!find . -type d -name 'lwir' -prune -exec rm -rf {} +
```
## Prepair annotations
YOLO requires a `.txt` file for each images with the same name and directory. Ex, the image at `set00/V000/visible/I00001.jpg` need an annotation file located at `set00/V000/visible/I00001.txt` (just different at suffix). Each txt file contains bounding boxes for objects inside corresponding image.
```bash
object-class x_center y_center width height
```
For quick create those txt files. run file 'create_anno_same_name.py' that prepared already. Remember to come into `obj` directory first.
```bash
% python create_anno_same_name.py
```
## Config YOLOv4 
You need to tell Darknet about expected model: "Hey, Darknet, i have 4 object categories, train the dataset for me with images located at data directory, learning rate = 0.00001, epochs = 5000,...blah". All you need is open `darknet/cfg/yolov4-custom.cfg` file and modify it a little bit. refer this link to know what need to change in the file: https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects
### To be continue
