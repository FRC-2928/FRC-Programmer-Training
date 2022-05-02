# OAK Camera Deployment for Jetson Nano
This section will show you how to deploy a Yolo object detection model on a Jetson Nano using the OAK camera.  The deployment will require the installation of the Depthai software. 

![Machine Learning Process](../images/FRCMachineLearning/FRCMachineLearning.013.jpeg)

To prepare for the install read the [Depthai Jetson Install](https://docs.luxonis.com/projects/api/en/latest/install/#jetson) on the Luxonis site.  This documentation shows you how to configure a 4GB swap space on your Jetson Nano and setup a python pip `virtualenv`.  If you already have your device setup with those options you can just follow the next steps.

Create a virtual environment.  The following command will create and put you into the virtual environment.  

    mkvirtualenv depthAI -p python3

Download and install the dependency package for the Depthai software:

    sudo wget -qO- https://docs.luxonis.com/install_dependencies.sh | bash

Clone the `depthai` github repository.  This example clones it in your $HOME directory:

    git clone https://github.com/luxonis/depthai-python.git
    cd ${HOME}/depthai-python

Edit your `.bashrc` with the following line:

    echo "export OPENBLAS_CORETYPE=ARMV8" >> ~/.bashrc

Install requirements for depthAI:

    cd ${HOME}/depthai-python/examples
    python install_requirements.py      

### Installing the FRC Detection Scripts        
In order to run the OAK-D camera and display the detected objects you need to deploy a custom python script that is specific to the type of detection model that you want to deploy.  In our case, we're going to use the `YoloV4-Tiny` model.  The script will use a default model that detects various common objects and display the output to a Web URL. It'll also place information about objects that have been detected into Network Tables so a it can be used by your WPILib java program.

To deploy the script follow these steps:

- Clone the FRC models and python scripts from GitHub:

        cd ${HOME}/depthai-python/examples
        git lfs clone https://github.com/FRC-2928/FRC-OAK-Deployment-Models.git

- Install the python requirements for the FRC scripts. The requirements are `pkgconfig`, `robotpy-cscore`, and `Pillow`. `robotpy-cscore` installs the *WPI Network Tables*. This can take 10-15 minutes to install.

        cd ${HOME}/depthai-python/examples/FRC-OAK-Deployment-Models
        python3 -m pip install -r requirements.txt        

There is also a model supplied with the *FRC-OAK-Deployment-Models* package that detects the Rapid-React balls from the 2022 competition.

### Running Inference
To run the inference script:

    cd ${HOME}/depthai-python/examples/FRC-OAK-Deployment-Models
    python oak_yolo_spacial.py -m rapid-react

The camera streamed output can be viewed from `<Your server IP address>:8080`.  
> Note: The camera stream does not work from a Safari browser, use Chrome or Firefox.

To run the inference script within a desktop GUI window:

    python oak_yolo_spacial.py -m rapid-react --gui

## The Inference Script
This script connects to the OAK-D camera and passes images to the inference model.  If an object-of-interest is detected a bounding box is drawn on the image together with positional data.  The positional data is put into the Network Tables so as it can be made use of by the WPILib robot program.  The overlayed images can be viewed in a Web browser.

The [python inference script](https://github.com/FRC-2928/FRC-OAK-Deployment-Models/blob/main/oak_yolo_spacial.py) can be found on FRC-2928 GitHub account. For a descripton of how this script works see [The Inference Script Explained](MLDeploymentScript.md).

## References
- [Depthai Jetson Install](https://docs.luxonis.com/projects/api/en/latest/install/#jetson)
