# Image Processing
When processing images coming from the camera we often refer to it as a vision pipeline. A vision pipeline represents a series of steps that are used to acquire an image, process it, and analyzing it to find a target. In most FRC games, this means processing an image in order to detect a piece of retroreflective tape or an AprilTag.

Documentation for [Creating a Pipeline](https://docs.photonvision.org/en/latest/docs/getting-started/pipeline-tuning/index.html)

## Lab - Image Processing
To be determined...

Here are a few notes on using the interface:

- Team number can be set to 2928 and it still communicates with the Simulator even though we're using 10.0.0.2.
- The Raspberry Pi must be in **Writable** mode to create a new pipeline.  I believe it can go back into **Read-Only** mode after the pipeline is created and you are executing your java program.
- While creating the pipeline you must adjust the filtering controls until you've aquired one or more targets, otherwise you won't get anything in the Network Tables.
- *Pipeline Type* must be **Reflective** mode otherwise there's no data.
Try setting output mode to right (camera is offset)
- If the stream is blank then try doing a browser reload.

## References
- PhotonVision - [Pipeline Tuning](https://docs.photonvision.org/en/latest/docs/getting-started/pipeline-tuning/index.html)

- FRC Documentation - [Introduction to Vision](https://docs.wpilib.org/en/stable/docs/software/vision-processing/introduction/what-is-vision.html)

- FRC Documentation - [Strategies for Vision Programming](https://docs.wpilib.org/en/stable/docs/software/vision-processing/introduction/strategies-for-vision-programming.html)

- QUT Robot Academy [Robot Vision](https://robotacademy.net.au/masterclass/robotic-vision/)

- Peter Corke Lecture - [Introduction to Robot Vision](https://www.youtube.com/watch?v=N_a6IP6KUSE&list=PL1pxneANaikCO1-Z0XTaljLR3SE8tgRXY)

- PhotonVision - [Documentation](https://docs.photonvision.org/en/latest/)

- Simon Prince - [Computer Vision](http://www.computervisionmodels.com)