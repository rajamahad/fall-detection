###-------- READ ME ( STEPS FOR COMPLETE SETUP OF FALL DETECTION ) ---------###
###---- HOW TO RUN FALL DETECTION MODEL ----###

## 📦 1. Clone the Repository :-
      .)clone the repository "https://muneeb_saif@bitbucket.org/reedling/yolov5_deployments.git".
      .)at the end of git clone command add " --branch fall ".
 
## 2. RENAME SCRIPT :-  
      .)after the repo is cloned change the name of "detect.py" to "inference.py".
      .)next step is to create environment suitable for our code.


## 3. SETTING UP ENVIRONMENT :-
      .) first create a new environment "conda create --name fall_det python=3.9"
      .) activate the environment using command "conda activate fall_det"
      .) install the requirements using pip command :-

## 4. INSTALLING REQUIRED REQUIREMENTS:-
      .) pip install torch
      .) pip install opencv-python
      .) pip install Ultralytics
      .) pip install dill
      
## 5. GIVING WEIGHT PATH :-

      .) after all these steps goto your inference.py file
      .) the .pt file is present in " /weights/fal_it5_athetic_data.pt "
      .) give the path of the weight file in run and parse argument in " default = "path to your pt file" "
      
## 6. ALL SET RUN THE CODE :-

      .) after it run your code in terminal "python inference.py "
      
## 7. RESULTS SAVED AT :-

      .) the result will be saved in folder " final_videos_images " ; output =  image and .json



### -----NOTE : IF YOU WANT TO RUN RTSP OR VIDEO ----###

## 1. ADD FUNCTION :-

    import os
    import sys
    os.environ["DISPLAY"] = ""
    os.environ["QT_QPA_PLATFORM"] = "offscreen"
    os.environ["OPENCV_FFMPEG_CAPTURE_OPTIONS"] = "rtsp_transport;udp"
    
## 2. GIVING PATH OF RTSP/VIDEO :-

    .) give the path of rtsp in the code
    .) give path in parse arguments " default="path to rtsp or video" "
    
## 3. ALL SET RUN THE CODE :-

    .) run the inference code 
    .) use command " python inference.py "
