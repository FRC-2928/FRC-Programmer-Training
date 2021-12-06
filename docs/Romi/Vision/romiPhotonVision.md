# Using PhotoVision with Romi
PhotonVision will be replacing GRIP going forward.  It has many advantages over GRIP and makes it much more streamlined to do vision processing on the Romi.  Refer to the [PhotonVision](https://docs.photonvision.org/en/latest/docs/getting-started/index.html) documentation for full details.

## Installation on the Romi
First refer to [Installing PhotonVision on the Romi](https://github.com/PhotonVision/photonvision-docs/blob/ec29ff49e4a03f8d06e00818885efa774126c24c/source/docs/getting-started/installation/romi.rst).

Most of the documentation is there but a few things to emphasize are:

- If Romi is setup as an access point you must plug in an ethernet cable before running the install.
- Set the Romi to **Writable** mode before installing.
- After editing the CameraServer `runCamera` script remember to put it back into **Read-Only** mode before rebooting.
- The install on the Romi sets up the `photonvision.service` file in `/lib/systemd/system` to start PhotonVision on reboot.  You can also start it manually: `systemctl stop/start photonvision.service`

## Creating a Pipeline
- Team number can be set to 2928 and it still communicates with the Simulator even though we're using 10.0.0.2.
- The Raspberry Pi must be in **Writable** mode to create a new pipeline.  I believe it can go back into **Read-Only** mode after the pipeline is created and you are executing your java program.
- While creating the pipeline you must adjust the filtering controls until you've aquired one or more targets, otherwise you won't get anything in the Network Tables.
- *Pipeline Type* must be **Reflective** mode otherwise there's no data.
Try setting output mode to right (camera is offset)
- If the stream is blank then try doing a browser reload.

Documentation for [Creating a Pipeline](https://docs.photonvision.org/en/latest/docs/getting-started/pipeline-tuning/index.html)

## PhotonLib Programming
It's important to note that PhotonCamera constructor is expecting the name of the camera as listed on the UI.  

    PhotonCamera m_camera = new PhotonCamera("mmal_service_16.1");

Documentation for [PhotonLib](https://docs.photonvision.org/en/latest/docs/programming/index.html)

## References

- PhotonVision - [Install on the Romi](https://github.com/PhotonVision/photonvision-docs/blob/ec29ff49e4a03f8d06e00818885efa774126c24c/source/docs/getting-started/installation/romi.rst)

- PhotonVision PDF - [Documentation](https://docs.photonvision.org/_/downloads/en/latest/pdf/)
