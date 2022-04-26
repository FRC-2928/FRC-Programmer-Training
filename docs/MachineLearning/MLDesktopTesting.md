## Testing the Model
 Before deploying the model to the robot it can be useful to test it on a laptop by connecting the OAK camera to it.  On a laptop you can prepare, train, and test on the model all on a single device, which makes the workflow more efficient.  You can also carry it around the target environment such a game field or your school hallway to test if it detects the objects of interest. All or the while you still have an internet connection for researching any issues that you may encounter. You can of course also test on the Raspberry Pi or Jetson Nano.  This is documented in the following section [Deploying the Model to the Robot](MLDeployment.md).  

### Laptop Install
To install the *Luxonis OAK* software on your PC operating system follow the [Installation](https://docs.luxonis.com/projects/api/en/latest/install/#) instructions on the Luxonis site.  Once the install is done install the depthai python package.  Starting at your desired install path, such as `~/Documents`:

        cd <install_path>
        git clone https://github.com/luxonis/depthai-python.git
        cd depthai-python

It's recommended that you install the dependencies in a virtual environment, so that they don’t interfere with other Python tools/environments on your system.

        virtualenv venv && source venv/bin/activate

Using a virtual environment run the following to install the requirements for this example repository:

        cd examples
        python3 install_requirements.py

To test the install run the default model:

        cd Yolo
        python3 tiny_yolo.py

This model detects 80 common objects.  You should see a camera stream pop up on your desktop.  Point it at yourself and it should recognize a `person`.  

### Test your Custom Model 
The script that that you executed above is specific to the test model that contains the 80 common objects.  Our models will contain different objects so we'll need a more customizable script in order to load and run them. To deploy this script follow these steps:

- Clone the models and python scripts from GitHub:

        cd <install_path>/depthai-python/examples
        git clone https://github.com/FRC-2928/FRC-OAK-Deployment-Models.git

- Install the python package requirements:

        cd FRC-OAK-Deployment-Models/
        python3 -m pip install -r requirements.txt        

There is a custom model that is supplied in the *FRC-OAK-Deployment-Models* package that detects the *Rapid-React* balls from the 2022 competition.  To use that model run the following commands:

        python3 spacial_tiny_yolo_tester.py

The python script will first check the directory for a file named `custom.blob`. If it finds that file it will load the associated `custom_config.json` file and run the model.  The above default model is also supplied in the *FRC-OAK-Deployment-Models* directory. If you want to test on the default model then rename the `custom.blob` file to something else.  To test your own model replace the `custom.blob` and `custom_config.json` files with your own files.

### Roboflow Deployment 
If you'd like to test your model on a PC prior to deploying it to the Raspberry Pi you can install the Roboflow Inference docker image. Instructions for this can be found on the [Luxonis OAK](https://docs.roboflow.com/inference/luxonis-oak) documentation page. This can only run on a Linux PC and requires that you have [Docker](https://docs.docker.com) installed.

It's useful to create a directory to hold docker files such as `build` and `run` files.  For example `my-containers`.  Then for each docker container image you can create a subdirectory, e.g. `roboflow`.   