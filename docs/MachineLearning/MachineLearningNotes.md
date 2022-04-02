# Notes

## Train Models on Macbook
Models can be transferred from the Jetson and trained on Macbook using Pytorch or Tensorflow.  Pytorch does not currently utilize the GPU on the M1 processor.  Comment out any statements that transfer the model to the GPU if using Pytorch.  Training is still a little faster on the M1 Macbook's CPU than on the Jetson's GPU.

You can transfer between notebooks by right click download, and then use the upload button to upload it to the target notebook.