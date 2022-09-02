# Jetson_nano_2GB_Setup_tips
Setup Tips 


1) INITIAL SETUP PROCEDURE (Physical)  
   - Download the Jetson Nano image
     - For JETSON NANO 2GB
       - https://developer.nvidia.com/jetson-nano-2gb-sd-card-image
     - For JETSON NANO 4GB 
       - https://developer.nvidia.com/jetson-nano-sd-card-image
   - Use "SD Card Formatter" to format the card (do this even if you are using a new card forthe first time)
   - Use "Balena Etcher" to burn the image to a "256GB SAMSUNG EVO+" card (This is really nice and fast)
   - Insert card into Jetson Nano
   - connect the following
     - HDMI monitor
     - Mouse
     - Keyboard
     - Webcam
     - Network cable (don't use wifi dongle, it can slow the camera down)
   - Insert the power cable and it will boot up - follow the onscreen instructions until you get to desktop


2) HOW TO INSTALL A FAN ON HEATSINK (Use a "Noctua NF-A4x20 5V PWM" fan)
   - Shutdown / remove power
   - Install a fan (e.g. Noctua NF-A4x20 5V PWM ) and connect the 4 pin plug to the 4 pin connector
   - Insert power cable to boot up
   - Open a terminal and execute
     - git clone  https://github.com/Pyrestone/jetson-fan-ctl
     - cd jetson-fan-ctl
     - sudo ./install.sh 


3) INITIAL SETUP (Software) 
   - In a terminal, execute the following (EXECUTE INDIVIDUALLY AND RESPOND "Y" WHEN NECESSARY)
     - sudo apt-get update
     - sudo apt-get upgrade
     - sudo apt-get install nano

4) Upgrade to Python 3.7 (This is to enable use of pandas above version 1.2.3)
   - sudo apt-get install python3-pip python3.7-dev
   - sudo apt install python3.7-dev
   - sudo rm /usr/bin/python3
   - sudo ln -s /usr/bin/python3.7 /usr/bin/python3

5) INSTALL VSCODE
   - Get the ".deb"file
     - Download the Ubuntu ".deb" file for "ARM64" from https://code.visualstudio.com/download
     - Right-click on the downloaded file and press "open Folder"
     - When the folder contents are displayed, double click the ".deb" file 
     - In the next window, press "install". 
     - After it has installed, the VsCode icon can be accessed when you press the bottom left icon (like START in Windows!) and look in the "programming" section.   
     - Right click the VsCode icon and Select "add to desktop" for easy access

6) FOR 2GB VERSION ONLY - HOW TO RESOLVE MEMORY PROBLEMS (Nano 2GB slows down/hangs if this is not done) 
   - Adjust the memory by Opening a terminal window and entering the following commands
     - free -m
     - sudo systemctl disable nvzramconfig
     - sudo fallocate -l 4G /mnt/4GB.swap
     - sudo chmod 600 /mnt/4GB.swap
     - sudo mkswap /mnt/4GB.swap
     - sudo nano /etc/fstab 
       - Add (or replace the existing swap line) the following line at the end, then "CtrlX" and then "CtrlO" to save
       - /mnt/4GB.swap swap swap defaults 0 0
       

7) REBOOT !!

8) MORE SOFTWARE TO INSTALL (EXECUTE INDIVIDUALLY AND RESPOND "Y" WHEN NECESSARY),
   - Please note the problems with dlib V24, use dlib v22 instead.  Instructions here https://medium.com/@ageitgey/build-a-face-recognition-system-for-60-with-the-new-nvidia-jetson-nano-2gb-and-python-46edbddd7264
     - sudo apt-get install python3-pip 
     - Add to path
       - FOR NANO
         - export PATH="/usr/local/bin:$PATH"
       - FOR AGX
         - export PATH="/home/jetsonagx/.local/bin:$PATH"  (replace "jetsonagx" with "your system name")
     - pip3 install cython
     - pip3 install numpy
     - pip3 install gtts
     - pip3 install playsound
     - pip3 install pyttsx3
     - pip3 install pyserial
     - sudo apt-get update
     - sudo apt-get install cmake 
     - sudo apt-get install libopenblas-dev 
     -
     - sudo apt-get install libblas-dev 
     - sudo apt-get install liblapack-dev 
     - sudo apt-get install libjpeg-dev
     - wget http://dlib.net/files/dlib-19.22.tar.bz2 
     - tar jxvf dlib-19.22.tar.bz2
     - cd dlib-19.22
     - sudo python3 setup.py install
     -
     - sudo -H pip3 install face_recognition
     -
     - sudo apt-get install portaudio19-dev  
     - sudo apt-get install python-all-dev 
     - sudo apt-get install python3-all-dev 
     - pip3 install pyaudio
     - pip3 install SpeechRecognition
     - 
     - sudo apt-get update
     - sudo apt-get install build-essential
     - sudo apt-get install libncurses5-dev
     - sudo apt-get install libgdbm-dev
     - sudo apt-get install libnss3-dev
     - sudo apt-get install libssl-dev
     - sudo apt-get install libreadline-dev
     - sudo apt-get install libffi-dev
     - git clone https://github.com/openai/openai-python.git
     - cd openai-python
     - sudo python3 setup.py install
     - sudo apt-get install flac
     - sudo pip3 uninstall numpy
     - pip3 install numpy

9) INSTALL THE COURSE JUPYTER NOTEBOOK. 
   - In a terminal window, execute the following commands (it takes quite a bit of time to install). When it finishes, check what the IP address of the nano is <Nano_ip> and you can access the jupyter notebook with a web browser using a url of <NANO_IP):8888  (use "dlinano" as the password) 
     - mkdir -p ~/nvdli-data
     - echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/nvdli-data:/nvdli-nano/data --device /dev/video0  nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh
     - chmod +x docker_dli_run.sh
     - ./docker_dli_run.sh    
   - If you reboot, you can bring back the Jupyter notebook by executing the last command of the above as follows (There is no need to re-install from scratch):
     - ./docker_dli_run.sh


10) ENROL IN COURSE HERE !!!
   - https://courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/courseware/b2e02e999d9247eb8e33e893ca052206/63a4dee75f2e4624afbc33bce7811a9b/


11) NOT NEEDED - JUST HERE FOR INFO - HOW TO INSTALL PYTHON VIRTUAL ENVIRONMENT. Execute the following in a terminal
   - In a terminal window, execute the following commands:
     - sudo apt-get install -y python3-venv
     - python3 -m venv .py3venv
     - ls -a
     - ls .py3venv/
     - source ~/.py3venv/bin/activate
     - deactivate


    
Now your Jetson Nano is ready to do face recognition with full CUDA GPU acceleration and speech recognition. Look here for programs
 - Face Recognition
   - https://github.com/CurlyWurly-1/Face_Recognition_Doorcam
 - Speech Recognition with a spoken response taken from GPT-3 
   - https://github.com/CurlyWurly-1/Chatbot


