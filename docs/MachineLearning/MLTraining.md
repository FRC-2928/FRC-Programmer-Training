# Training a Model
There are options that we have to train our model.  It can be trained in **Roboflow** or we can export it for training in **Google Colab**.  To train in Roboflow watch the [Training](https://www.youtube.com/watch?v=njWwmKLWVyE)  YouTube Video and read the [Train](https://docs.roboflow.com/train) documentation.  

## Training Model in Colab
This process will show how to train a [Yolo](https://pjreddie.com/darknet/yolo/) model for deployment on an [Luxonis OAK](https://shop.luxonis.com/collections/usb) device.  The training process in Google Colab follows these steps.

![Training](../images/FRCMachineLearning/FRCMachineLearning.010.jpeg)

Make sure that you're familiar with how Colab works. For a quick introduction watch this [Google Colab](https://www.youtube.com/watch?v=RLYoEyIHL6A&ab_channel=CodewithDogaOzgon) YouTube video. 

Use the [Roboflow-YOLOv3-tiny-Darknet-to-OAK.ipynb](https://colab.research.google.com/drive/1Sc6B-clNJZ3OBxCxIoddhLFxIYoJJzRT#scrollTo=JlPEcD7UkE4Q) Colab Notebook to train the model.  Once the Colab notebook loads save a copy of the it in your Google Drive.  When you execute the first cell it will allocate a GPU for you. You should try and complete all of the training steps in a single session.  If you page is inactive for more than an hour the session will disconnect.

### Setup the Colab Notebook for Training

- Configure our GPU environment on Google Colab

- Install the [Darknet](https://github.com/pjreddie/darknet) YOLOv4 training environment.  The darknet software is cloned from GitHub and then compiled.  After install you'll see the `darknet` directory structure in the *Files* sidebar menu.

### Importing the Dataset
In order to train the dataset in Colab it has to be exported to a format compatible with the training model.  In our case, we'll be using the YoloV4 training model, so we'll export it in *YOLO Darknet* format. The export process generates a `.txt` file along with each image that describe the bounding boxes and the label identifier. The txt file will have the same name as the image and contains one line per bounding box.

Once the export is complete you will be displayed a token that can be used to import the dataset into Colab.

![Generating Dataset](../images/FRCMachineLearning/FRCMachineLearning.006.jpeg)

- Download custom dataset for YOLOv4 and set up directories. This is where you get your dataset from Roboflow into the Colab notebook.  Copy the export key from the Roboflow export and paste it into the notebook.  Your key should look something like this:

        !pip install roboflow

        from roboflow import Roboflow
        rf = Roboflow(api_key="yourkey")
        project = rf.workspace("martin-white").project("infinte-recharge")
        dataset = project.version(2).download("darknet")

Set up training file directories for custom dataset

### Setup Configuration Files for Training
The weights file for YoloV4-tiny is downloaded in order to generate a custom training configuration file for Darknet.  The configuration file contains a set of parameters for the training process. These parameters are customized for your dataset will contain good default values so there shouldn't be much need to change them.


### Train Detector Model
The next step is to train a custom YOLOv4 object detector.

As the training progresses files with the suffix `.weights` will show up in the `darknet/backup` directory.  As soon as you have a file with a partial name of `_best.weights` you can stop the training process.

### Run Inference to Test the Model
This step will load the YOLOv4 trained weights and make inference on test images.  

### Convert to OpenVino Blob File
When you are satisfied with the model's testing results you can convert it to a file format that runs on the OAK-D camera.  Save and download the `.weights` file that was created in *Train Detector Model* step.  This file can be converted to other formats suitable for deployment.

The first step in creating the blob file is to install Tensorflow and convert the `.weights` file to a `.pb`

The next step is to install the [yolo2openvino](https://github.com/luxonis/yolo2openvino) library into the Colab environment.  This library will export the `.weights` file into the OpenVINO *Intermediate Representation* IR format. A JSON configuration file is required for the conversion. The most important parameter to check in the configuration file is the number of classes.  This should match the number of object types that you're tracking, for example, if you have *Blueballs* and *Redballs* then the number of classes should be 2.

        [
                {
                        "id": "TFYOLOV3",
                        "match_kind": "general",
                        "custom_attributes": {
                        "classes": 2,
                        "anchors": [10, 14, 23, 27, 37, 58, 81, 82, 135, 169, 344, 319],
                        "coords": 4,
                        "num": 6,
                        "masks": [[3, 4, 5], [0, 1, 2]],
                        "entry_points": ["detector/yolo-v4-tiny/Reshape", "detector/yolo-v4-tiny/Reshape_4"]
                        }
                }
        ]

Lastly, we install the *blobconverter* library and use it to convert IR files to the *OpenVino* to blob format that can be run on OAK devices. After this step is complete the *IR* and *blob* files can be downloaded to your PC.

The full conversion process is shown below.

![Converting Model](../images/FRCMachineLearning/FRCMachineLearning.008.jpeg)


## References
- Roboflow [Training](https://www.youtube.com/watch?v=njWwmKLWVyE) - Youtube Video

- [Google Colab](https://www.youtube.com/watch?v=RLYoEyIHL6A&ab_channel=CodewithDogaOzgon) - YouTube Video