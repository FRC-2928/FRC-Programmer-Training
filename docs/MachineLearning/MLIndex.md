# Introduction to Machine Learning
**Machine Learning** is a powerful tool that can accomplish computational tasks that traditional algorithms cannot. In the FIRST Robotics Competition, machine learning can be used to detect game pieces, scoring locations, or even other robots.  For a good overview of Machine Learning see Brendan Fortuner's [Machine Learning Glossary](https://ml-cheatsheet.readthedocs.io/en/latest/nn_concepts.html).  Also see an [Introduction to Machine Learning](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/machine-learning.html) in the FRC Documentation. 

In order to implement Machine Learning we generally follow a process of collecting images, annotating the images, preprocessing and augmenting the images, training a model, deploying the trained model on the robot, and finally using the trained model to control the robot.  This is an iterative process that can be continued until we have a satisfactory outcome.  
![Machine Learning Process](../images/FRCMachineLearning/FRCMachineLearning.001.jpeg)

There are multiple ways to carry out the machine learning process.  FRC has a tool called **Axon** that implements the training, testing, and exporting of the machine learning model.  See an [Introduction to Axon](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/introduction.html) in the FRC Documentation.  The model is then deployed to a Raspberry Pi using a [Coral USB Accelerator](https://coral.ai/products/accelerator) for real-time processing of the images. 

An alternative process, and the one that will be documented in the next sections, is to use the [Roboflow](https://roboflow.com) platform to upload video, annotate, preprocess, and augment the images in preparation for training. While we are refining the model it can be also be trained and validated on Roboflow. However, in order to deploy the model for competition it must be trained in [Google Colab](https://colab.research.google.com/?utm_source=scs-index).

The trained model is deployed to a Raspberry Pi using an OAK-D camera provided by [Luxonis](https://www.luxonis.com).  The OAK-D camera has an onboard Tensor Processing Unit (TPU) that runs the model and processes the images before sending it to the Raspberry Pi.

![Roboflow OAK-D Process](../images/FRCMachineLearning/FRCMachineLearning.002.jpeg)

The deployment process for *Roboflow*, *Google Colab*, with the *OAK-D Camera* and *Raspberry Pi* will be documented in the following sections.


- [Preparing Images](MLProcessImages.md)
- [Training the Model](MLTraining.md)
- [Deploying the Model](MLDeployment.md)
- [Machine Learning Based Control](MLControl.md)

## References
- FRC Documentation - [Axon](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/introduction.html)

- Brendan Fortuner [ML Glossary](https://ml-cheatsheet.readthedocs.io/en/latest/nn_concepts.html)

- Deep Learning - [Video Playlist](https://www.youtube.com/playlist?list=PLZbbT5o_s2xq7LwI2y8_QtvuXZedL6tQU) - deeplizard

- Roboflow [OpenCV AI Kit Training Course](https://roboflow.com/course)

- OAK-D Camera [Luxonis](https://www.luxonis.com)

- Zebra Labs [ZebraVision 7.0](https://drive.google.com/file/d/1a6TjJwKlH-76vkAthQSiNZgopLMd_BW_/view)