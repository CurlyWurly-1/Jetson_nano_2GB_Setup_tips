# Jetson_nano_2GB_Setup_tips
Setup Tips 

1) HOW TO INSTALL A FAN ON HEATSINK
- Install a fan (e.g. Noctua NF-A4x20 5V PWM )
- Install fan control
    Open a terminal and execute
        git clone  https://github.com/Pyrestone/jetson-fan-ctl  
        sudo ./install.sh 

2) HOW TO DO INITIAL SETUP  
- Download the Jetson Nano 2GB image here
- Use Balena Etcher to burn the image to a 64GB card
- Insert card into Jetson Nano
- connect the following
    - HDMI monitor
    - Mouse
    - Keyboard
    - Network cable (don't use wifi dongle, it can slow thee camera down)
- Insert the power cable and it will boot up - follow the onscreen instructions until you get to desktop
- In a terminal, execute the follwing 
    sudo apt-get install nano
    sudo apt-get update
    sudo apt-get upgrade

3) HOW TO RESOLVE MEMORY PROBLEMS (Nano 2GB slows down/hangs if this is not done) 
- Adjust the memory by Opening a terminal window and entering the following commands
    free -m
    sudo systemctl disable nvzramconfig
    sudo fallocate -l 4G /mnt/4GB.swap
    sudo chmod 600 /mnt/4GB.swap
    sudo mkswap /mnt/4GB.swap
    sudo nano /etc/fstab 
        - Add or replace the existing swap line with the following line (at the end), then CtrlX and then CtrlO to save
            /mnt/4GB.swap swap swap defaults 0 0

4) REBOOT !!

5) Enrol in course here
    https://courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/courseware/b2e02e999d9247eb8e33e893ca052206/63a4dee75f2e4624afbc33bce7811a9b/

5) Install the Jupyter notebook with the following commands (it takes quite a bit of time to install) . When it finishes, check what the IP address of the nano is and you can access the jupyter notebook at <NANOIP):8888 - use "dlinano" as the password 
 - echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/nvdli-data:/nvdli-nano/data --device /dev/video0  nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh
 - chmod +x docker_dli_run.sh
 - ./docker_dli_run.sh    
 
6) If you reboot, the Jupyter notebook will not be running - you can bring it back by executing the last line of above as follows. There is no need to re-install from scratch
 - ./docker_dli_run.sh
