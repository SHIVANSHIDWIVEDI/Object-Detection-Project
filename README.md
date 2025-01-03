# Object Detection with OpenCV and SSD MobileNet v3

## Table of Contents
1. [Description](#description)
2. [Requirements](#requirements)
3. [Instructions](#instructions)
4. [Code Breakdown](#code-breakdown)


## Description
Object detection is a computer vision technique that involves locating instances of objects within images or videos. Machine learning (ML) plays a crucial role in object detection, as it enables algorithms to learn from large datasets and produce accurate results.

# Key Steps in an Object Detection Project : 
#1. Data Preparation: Collect and preprocess a large dataset of labeled images or videos containing the objects of interest.

#2. Model Selection: Choose a pre-trained object detection model, such as YOLO, CocoSSD, or MobileNet, or train a custom model using TensorFlow Lite.

#3. Model Optimization: Optimize the model for on-device deployment, ensuring it runs efficiently on mobile devices or other constrained hardware.

#4. Object Detection and Tracking: Use the optimized model to detect and track objects in real-time, either from images or live camera feeds.

#5. Optional Classification: Classify detected objects into predefined categories using a coarse classifier or a custom image classification model.


This Python script leverages OpenCV and a pre-trained SSD MobileNet v3 deep learning model for real-time object detection in video streams. It utilizes the COCO (Common Objects in Context) dataset for object class recognition.

This description gives potential users an overview of what the repository offers and how they can get started. 

## Requirements

- Python 3.x ([Download](https://www.python.org/downloads/))
- OpenCV library: Install via `pip install opencv-python`
- NumPy library: Install via `pip install numpy` (usually included with OpenCV)
- `coco.names` file: Contains class labels for the COCO dataset (download from a reliable source)
- `ssd_mobilenet_v3_large_coco_2020_01_14.pbtxt` file: Model configuration file (download from a reliable source)
- `frozen_inference_graph.pb` file: Model weights file (download from a reliable source)

## Instructions

1. **Download necessary files:**
   - Obtain the `coco.names`, `ssd_mobilenet_v3_large_coco_2020_01_14.pbtxt`, and `frozen_inference_graph.pb` files from a trusted source (ensure compatibility with your OpenCV version). Place them in the same directory as your Python script.

2. **Run the script:**
   - Open a terminal or command prompt, navigate to the directory containing your script and files.
   - Execute the script using:
     ```sh
     python object_detection.py
     ```

## Code Breakdown

1. **Imports:**
   - `cv2`: Imports the OpenCV library for computer vision tasks.

2. **Threshold and Video Capture:**
   - `thres`: Sets the confidence threshold for object detection (adjust as needed).
   - `cap`: Initializes a video capture object (`VideoCapture(1)`) to access your webcam (or a different video source by providing its index or path).
   - `cap.set()`: Sets video capture properties:
     - `3`: Width (adjust for desired resolution)
     - `4`: Height (adjust for desired resolution)
     - `10`: Brightness (adjust for lighting conditions)

3. **Load Class Names:**
   - `classNames`: Creates an empty list to store object class names.
   - `classFile`: Path to the `coco.names` file.
   - Loads class names from the file and splits them into a list.

4. **Load Model Configuration and Weights:**
   - `configPath`: Path to the `ssd_mobilenet_v3_large_coco_2020_01_14.pbtxt` file.
   - `weightsPath`: Path to the `frozen_inference_graph.pb` file.
   - `net`: Creates a detection model object using `cv2.dnn_DetectionModel()`.
   - Sets model input size, scale, mean, and color channel swapping parameters for compatibility with the model.

5. **Main Loop:**
   - `while True`: Continuously captures frames from the video stream.
   - `success, img`: Reads a frame and checks for success. Exits if unsuccessful.
   - `classIds`, `confs`, `bbox`: Performs object detection using the model on the current frame.
     - `classIds`: List of detected object class IDs.
     - `confs`: List of corresponding confidence scores (0-1).
     - `bbox`: List of bounding boxes (coordinates) for detected objects.
   - Prints detected object IDs and bounding boxes for debugging (optional).

6. **Draw Bounding Boxes and Labels:**
   - Checks if any objects were detected (`len(classIds) != 0`).
   - Iterates through detected objects using `zip`:
     - `box`: Current bounding box coordinates.
     - `classId`: Current object class ID (minus 1 for indexing).
     - `confidence`: Current object confidence score.
   - Draws a green rectangle around the detected object using `cv2.rectangle()`.
   - Displays the corresponding class name (uppercase) and confidence score (rounded to two decimal places) using `cv2.putText()`.

7. **Display and Exit:**
   - `cv2.imshow()`: Displays the processed frame with bounding boxes and labels in a window titled "Output".
   - `cv2.waitKey(1)`: Waits for a key press for 1 millisecond.
   - Exits the loop and releases resources if the 'q' key is pressed.

8. **Cleanup:**
   - Releases the video capture object and closes all OpenCV windows.
     ```python
     cap.release()
     cv2.destroyAllWindows()
     ```
