# Social-Distancing-in-Real-Time
Social distancing in Real-Time using live video stream/IP camera in OpenCV.

> This is an improvement/modification to (https://www.pyimagesearch.com/2020/06/01/opencv-social-distancing-detector/).

> Please refer to the added [Features](#features).

Output       |  Output
:-------------------------:|:-------------------------:
![Output](mylib/videos/output.gif?raw=true "Output")  |  ![Output](mylib/videos/output1.gif?raw=true "Output")

- Use case: counting the number of people in the stores/buildings/shopping malls etc., in real-time.
- Sending an alert to the staff if the people are way over the social distancing limits.
- Optimizing the real-time stream for better performance (with threading).
- Acts as a measure to tackle COVID-19.

---

## Simple Theory
**Object detection:**
- We will be using YOLOv3, trained on COCO dataset for object detection.
- In general, single-stage detectors like YOLO tend to be less accurate than two-stage detectors (R-CNN) but are significantly faster.
- YOLO treats object detection as a regression problem, taking a given input image and simultaneously learning bounding box coordinates and corresponding class label probabilities.
- It is used to return the person prediction probability, bounding box coordinates for the detection, and the centroid of the person.

---
**Distance calculation:**
- NMS (Non-maxima suppression) is also used to reduce overlapping bounding boxes to only a single bounding box, thus representing the true detection of the object. Having overlapping boxes is not exactly practical and ideal, especially if we need to count the number of objects in an image.
- Euclidean distance is then computed between all pairs of the returned centroids. Simply, a centroid is the center of a bounding box.
- Based on these pairwise distances, we check to see if any two people are less than/close to 'N' pixels apart.

## Running Inference
- Install all the required Python dependencies:
```
pip install -r requirements.txt
```
- If you would like to use GPU, set ```USE_GPU = True``` in the config. options at 'mylib/config.py'.

- Download the weights file from [**here**](https://drive.google.com/file/d/1O2zmGIIHLX8SGs24W7mjRyFKvE_CSY8n/view?usp=sharing) and place it in the 'yolo' folder.

- To run inference on a test video file, head into the directory/use the command:
```
python run.py -i mylib/videos/test.mp4
```
- To run inference on an IP camera, Setup your camera url in 'mylib/config.py':

```
# Enter the ip camera url (e.g., url = 'http://191.138.0.100:8040/video')
url = ''
```
- Then run with the command:

```
python run.py
```
> Set url = 0 for webcam.

## Features
The following are examples of the added features. Note: You can easily on/off them in the config. options (mylib/config.py):

***1. Real-Time alert:***
- If selected, we send an email alert in real-time. Use case: If the total number of violations (say 10 or 30) exceeded in a store/building, we simply alert the staff.
- You can set the max. violations limit in config (```Threshold = 15```).
- This is pretty useful considering the COVID-19 scenario.

***2. Desired violations limits:***
- You can also set your desired minimum and maximum violations limits. For example, ```MAX_DISTANCE = 80``` implies the maximum distance 2 people can be closer together is 80 pixels. If they fell under 80, we treat it as an 'abnormal' violation (yellow).
- Similarly ```MIN_DISTANCE = 50``` implies the minimum distance between 2 people. If they fell under 50 px (which is closer than 80), we treat it as a more 'serious' violation (red).
- Anything above 80 px is considered as a safe distance and thus, 'no' violation (green).

## References
***Main:***
- YOLOv3 paper: https://arxiv.org/pdf/1804.02767.pdf
- YOLO original paper: https://arxiv.org/abs/1506.02640
- YOLO TensorFlow implementation (darkflow): https://github.com/thtrieu/darkflow
---

## Thanks for the read & have fun!

- **Option 1**
    - 🍴 Fork this repo and pull request!

- **Option 2**
    - 👯 Clone this repo:
    ```
    $ git clone https://github.com/Pushkar78/Social-Distance-Detection.git
    ```
--

