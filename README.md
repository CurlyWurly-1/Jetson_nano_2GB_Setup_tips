# Jetson_nano_2GB_Setup_tips
Setup Tips 



1) HOW TO INSTALL A FAN ON HEATSINK
   - Install a fan (e.g. Noctua NF-A4x20 5V PWM )
   - Install fan control
     - Open a terminal and execute
       - git clone  https://github.com/Pyrestone/jetson-fan-ctl    
       - sudo ./install.sh 



2) INITIAL SETUP PROCEDURE  
   - Download the Jetson Nano 2GB image here
     - https://developer.nvidia.com/jetson-nano-2gb-sd-card-image
   - Use Balena Etcher to burn the image to a 64GB card
   - Insert card into Jetson Nano
   - connect the following
     - HDMI monitor
     - Mouse
     - Keyboard
     - Webcam
     - Network cable (don't use wifi dongle, it can slow the camera down)
   - Insert the power cable and it will boot up - follow the onscreen instructions until you get to desktop
   - In a terminal, execute the following 
     - sudo apt-get install nano
     - sudo apt-get update
     - sudo apt-get upgrade



3) HOW TO RESOLVE MEMORY PROBLEMS (Nano 2GB slows down/hangs if this is not done) 
   - Adjust the memory by Opening a terminal window and entering the following commands
     - free -m
     - sudo systemctl disable nvzramconfig
     - sudo fallocate -l 4G /mnt/4GB.swap
     - sudo chmod 600 /mnt/4GB.swap
     - sudo mkswap /mnt/4GB.swap
     - sudo nano /etc/fstab 
       - Add (or replace the existing swap line) the following line at the end, then "CtrlX" and then "CtrlO" to save
       - /mnt/4GB.swap swap swap defaults 0 0



4) REBOOT !!



5) ENROL IN COURSE HERE !!!
   - https://courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/courseware/b2e02e999d9247eb8e33e893ca052206/63a4dee75f2e4624afbc33bce7811a9b/



5) INSTALL THE COURSE JUPYTER NOTEBOOK
Install the Jupyter notebook with the following commands (it takes quite a bit of time to install) . When it finishes, check what the IP address of the nano is and you can access the jupyter notebook at <NANOIP):8888 - use "dlinano" as the password 
   - mkdir -p ~/nvdli-data
   - echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/nvdli-data:/nvdli-nano/data --device /dev/video0  nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh
   - chmod +x docker_dli_run.sh
   - ./docker_dli_run.sh    
 
 
 
6) IF YOU REBOOT, GET THE JUPYTER NOTEBOOK BACK
If you reboot, the Jupyter notebook will not be running - You can bring it back by executing the last line of the above as follows (There is no need to re-install from scratch)
   - ./docker_dli_run.sh


7) INSTALL VSCODE
Download the .deb file for 'ARM64'. 
When downloaded, click on it, and in the next window, press "install". After it has installed, VsCode can be accessed in the "programming" section when you press the bottom left icon (like START in Windows!). Right click the VsCode icon and Select "add to desktop" for easy access
   - https://code.visualstudio.com/download


8) INSTALL PYTHON VIRTUAL ENVIRONMENT. Execute the following in a terminal
   - sudo apt-get install -y python3-venv
   - python3 -m venv .py3venv
   - ls -a
   - ls .py3venv/
   - source ~/.py3venv/bin/activate
   - deactivate


8) INSTALL PIP3
Execute the following command in a terminal
   - sudo apt-get -y install python3-pip


9) INSTALL USEFUL LIBRARIES 
   - pip2 install numpy
   - pip3 install gtts
   - pip3 install playsound
   - pip3 install pyttsx3
   - pip3 install pyserial

10) INSTALL "face_recognition". Follow the instructions here https://medium.com/@ageitgey/build-a-face-recognition-system-for-60-with-the-new-nvidia-jetson-nano-2gb-and-python-46edbddd7264
    - sudo apt-get update
    - sudo apt-get install python3-pip cmake libopenblas-dev liblapack-dev libjpeg-dev
    - sudo pip3 -v install Cython face_recognition


pip3 install numpy
This command will take 15 minutes since it has to compile numpy from scratch. Just wait until it finishes and don’t get worried it seems to freeze for a while.

Now we are ready to install dlib, a deep learning library created by Davis King that does the heavy lifting for the face_recognition library.

However, there is currently a bug in Nvidia’s own CUDA libraries for the Jetson Nano that keeps it from working correctly. To work around the bug, we’ll have to download dlib, edit a line of code, and re-compile it. But don’t worry, it’s no big deal.

In Terminal, run these commands:

wget http://dlib.net/files/dlib-19.17.tar.bz2 
tar jxvf dlib-19.17.tar.bz2
cd dlib-19.17
That will download and uncompress the source code for dlib. Before we compile it, we need to comment out a line. Run this command:

gedit dlib/cuda/cudnn_dlibapi.cpp
This will open up the file that we need to edit in a text editor. Search the file for the following line of code (which should be line 854):

forward_algo = forward_best_algo;
And comment it out by adding two slashes in front of it, so it looks like this:

//forward_algo = forward_best_algo;
Now save the file, close the editor, and go back to the Terminal window. Next, run these commands to compile and install dlib:

sudo python3 setup.py install
This will take around 30–60 minutes to finish and your Jetson Nano might get hot, but just let it run.

Finally, we need to install the face_recognition Python library. Do that with this command:

sudo pip3 install face_recognition
Now your Jetson Nano is ready to do face recognition with full CUDA GPU acceleration. On to the fun part!

