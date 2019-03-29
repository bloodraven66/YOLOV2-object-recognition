# YOLOV2-object-recognition
Follow these steps to run yolo2 object detector.<br>
Full documentation can be found <a href = 'https://medium.com/@manivannan_data/how-to-train-yolov2-to-detect-custom-objects-9010df784f36'>here</a>.<br>

## Installation
Step 1:
Clone <a href = 'https://github.com/AlexeyAB/darknet.git'>darknet</a><br>
```
$ git clone https://github.com/AlexeyAB/darknet.git
$ cd darknet
```
Step 2:
edit Makefile and set gpu to 0 if you dont have it
```
$ vi Makefile
```
Step 3:
install darknet
```
make
```
## Trial run
Step 1:
Test yolo on pre-trained model. Download <a href = 'https://pjreddie.com/media/files/yolo.weights'>pre-trained weights</a>.
```
wget https://pjreddie.com/media/files/yolo.weights
```
Step 2:
Run the detector
```
./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg
```
Step 3:
View the output
```
xdg-open predictions.png
```
## For custom data
Step 1:
Annotate training data using <a href = 'https://github.com/tzutalin/labelImg'>labelimg</a> tool.<br>

Step 2:
Keep all training images and annotation text files in same folder.
```
#Consider 1000 training images, set 900 for train, 100 for test
import glob, os
current_dir = ''
# Percentage of images to be used for the test set
percentage_test = 10;
# Create and/or truncate train.txt and test.txt
file_train = open('train.txt', 'w')  
file_test = open('test.txt', 'w')
i = 0
for pathAndFilename in glob.iglob(os.path.join(current_dir, "*.jpg")):
    i = i+1
    if(i<900):
        title, ext = os.path.splitext(os.path.basename(pathAndFilename))
        file_train.write(current_dir + "/" + title + '.jpg' + "\n")
    else:
        title, ext = os.path.splitext(os.path.basename(pathAndFilename))
        file_test.write(current_dir + "/" + title + '.jpg' + "\n")
```
Step 4:
create/modify file cfg/obj.data as follows.
```
classes = 
train = train.txt
valid = test.txt
names = obj.names
backup = backup/
```
Step 5:
create/modify cfg/file obj.names.
```
class 1 name
class 2 name
...
```
Step 6:
Store the pre-trained model to be used in cfg/tiny-yolo.cfg.<br>

Step 7: 
Download required pre-trained weight file <a href = 'https://pjreddie.com/media/files/darknet19_448.conv.23'>here</a>.

Step 8:
Start training
```
$ ./darknet detector train cfg/obj.data cfg/yolo-obj.cfg weight_file_name
```
Note : the model saves new weight file every 100 iterations which can be used for testing. <br>

Step 9:
Test your model.
```
./darknet detector test cfg/obj.data cfg/yolo-obj.cfg new_weight_file_name.weights data/target_image.jpg
```


