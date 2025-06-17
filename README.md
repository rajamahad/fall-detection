##--------  STEPS FOR COMPLETE SETUP OF FALL DETECTION MODEL  ---------##


## 📦 1. Clone the Repository :-
      .) Clone the repository "https://muneeb_saif@bitbucket.org/reedling/yolov5_deployments.git".
      .) At the end of git clone command add " --branch fall ".
      .) Full command is " git clone https://muneeb_saif@bitbucket.org/reedling/yolov5_deployments.git --branch fall ".
 
## 2. RENAME SCRIPT :-  
      .) After the repository is cloned change the name of "detect.py" to "inference.py".
      .) Next step is to create environment suitable for our code.


## 3. 🧪 SETTING UP ENVIRONMENT :-
      .) First create a new environment "conda create --name fall_det python=3.9"
      .) Activate the environment using command "conda activate fall_det"
      .) Now install the requirements.

## 4. 📥 INSTALLING REQUIRED DEPENDENCIES :-
      .) pip install torch
      .) pip install opencv-python
      .) pip install Ultralytics
      .) pip install dill
      
## 5. ⚙️ CONFIGURE WEIGHT PATH :-

      .) After all these steps goto your inference.py file
      .) The .pt file is present in " /weights/fal_it5_athetic_data.pt "
      .) Give the path of the weight file in run and parse argument in " default = "path to your pt file" "
      
## 6. ✅ You're All Set! :-

      .) After it run your code in terminal "python inference.py "
      
## 7. RESULTS SAVED AT :-

      .) The result will be saved in folder " final_videos_images " ; output =  image and .json



### -----NOTE : IF YOU WANT TO RUN RTSP OR VIDEO ----###

## 1. ADD FUNCTION :-

    import os
    import sys
    os.environ["DISPLAY"] = ""
    os.environ["QT_QPA_PLATFORM"] = "offscreen"
    os.environ["OPENCV_FFMPEG_CAPTURE_OPTIONS"] = "rtsp_transport;udp"
    
    NOTE : os is already imported so you just have to add the "DISPLAY" and "QT_QPA_PLATFORM " line of codes .. Also import sys IF not imported .
    
## 2. GIVING PATH OF RTSP/VIDEO :-

    .) Give the path of rtsp in the code
    .) Give path in parse arguments " default="path to rtsp or video" "
    
## 3. ✅ You're All Set! :-

    .) Run the inference code 
    .) Use command " python inference.py "
    
    
## 📁 PROJECT STRUCTURE :-

    fall_detection/
    ├── inference.py           # Main script
    ├── weights/               # Trained model (.pt files)
    ├── data/                  # Data configs (YAMLs)
    ├── utils/                 # Helper scripts
    └── final_videos_images/   # Output results (images + JSON)
