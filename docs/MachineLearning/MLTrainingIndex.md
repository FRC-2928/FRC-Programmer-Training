# Training and Validating the Model
Once you have a dataset of the images that you're happy with it can be used to train and validate a detection model. A detection model is used to identify objects of interest in the field-of-view of the camera.  The model will be trained to recognize these objects, for example, the red and blue balls in the Rapid React competition.  In order to deploy a model to the competition robot it must be trained using *Google Colab* since you'll need to have a model file that can be used offline.  Google Colab uses Jupyter Notebooks that are run directly their online servers.  The Colab platform has GPU processors available that significantly speed up the training process.  It would be very expensive to setup a comparable training environment at home.


- [Training a Model with Colab](MLColabTraining.md)
- [Training a Model with Axon](MLAxonTraining.md)

