# Introduction to Machine Learning
**Machine Learning** is a powerful tool that can accomplish computational tasks that traditional algorithms cannot. In the FIRST Robotics Competition, machine learning can be used to detect game pieces, scoring locations, or even other robots.  For a good overview of Machine Learning see Brendan Fortuner's [Machine Learning Glossary](https://ml-cheatsheet.readthedocs.io/en/latest/nn_concepts.html).  Also see an [Introduction to Machine Learning](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/machine-learning.html) in the FRC Documentation. 

In order to implement Machine Learning we'll generally follow a process of collecting images, annotating the images, training a model, deploying the trained model on the robot, and finally using the trained model to control the robot.  This is an iterative process that will be continued until we have a satisfactory outcome.  
![Machine Learning Process](../images/FRCMachineLearning/FRCMachineLearning.001.jpeg)

There are multiple ways to carry out this process.  FRC has a tool called **Axon** that implements the training, testing, and exporting of the machine learning model.  See an [Introduction to Axon](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/introduction.html) in the FRC Documentation.  Other processes are used to Annotate the images. Various options to carry out this process are detailed in the following sections.

- [Image Collection](MLImageCollection.md)
- [Annotating Images](MLAnnotation.md)
- [Training the Model](MLTraining.md)
- [Deployment](MLDeployment.md)
- [Machine Learning Based Control](MLControl.md)

## References
- FRC Documentation - [Axon](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/axon/introduction.html)

- Brendan Fortuner [ML Glossary](https://ml-cheatsheet.readthedocs.io/en/latest/nn_concepts.html)

- Jeremy Howard, Sylvain Gugger - [Practical Deep Learning for Coders](https://course.fast.ai/)

- Deep Learning - [Video Playlist](https://www.youtube.com/playlist?list=PLZbbT5o_s2xq7LwI2y8_QtvuXZedL6tQU) - deeplizard