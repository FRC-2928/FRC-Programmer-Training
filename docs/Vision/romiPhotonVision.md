# Using PhotoVision with Romi
*PhotonVision* is an easy-to-use vision processing solution for FRC.  In this training guide we'll be using [PhotonVision](https://docs.photonvision.org/en/latest/docs/getting-started/description.html) on the Romi to learn the basics of *Computer Vision*.  Many teams use PhotoVision on their competition robot since it can be used with a standard USB camera. To learm more about PhotoVision refer to the [Getting Started](https://docs.photonvision.org/en/latest/docs/getting-started/index.html) documentation.

 Note, that our team uses the [Limelight](https://limelightvision.io) camera on the competition robot, which we'll learn about later.  The Limelight camera uses its own software which is easy to install and maintain.

## Installing PhotoVision on the Romi
First refer to [Installing PhotonVision on the Romi](https://docs.photonvision.org/en/latest/docs/getting-started/installation/sw_install/romi.html). 
Most of the documentation is there but a few things to emphasize are:

- If Romi is setup as an access point you must plug in an ethernet cable before running the install.
- Set the Romi to **Writable** mode before installing.
- After editing the CameraServer `runCamera` script remember to put it back into **Read-Only** mode before rebooting.
- The install on the Romi sets up the `photonvision.service` file in `/lib/systemd/system` to start PhotonVision on reboot.  You can also start it manually: `systemctl stop/start photonvision.service`
<!-- - Make sure that you follow the [Update Steps](https://docs.photonvision.org/en/latest/docs/getting-started/installation/advanced-cmd.html) if you already have PhotonVision installed. Otherwise, you end up with a bad `photonvision.service` file and PhotonVision won't start. -->

To update PhotonVision refer to [Updating PhotonVision](https://docs.photonvision.org/en/latest/docs/getting-started/installation/sw_install/raspberry-pi.html#updating-photonvision).


## Creating a Pipeline
A pipeline is created using the Web interface at `http://10.0.0.2:5800/`.  We'll go over this in the next section when we look at [Image Processing](romiImageProcessing). 


## PhotonLib Programming
Before you do any WPILib programming you have to install the PhotonLib Library. This library makes it easier for teams to retrieve vision data from their vision system. See the [PhotonLib Installation](https://docs.photonvision.org/en/latest/docs/programming/photonlib/adding-vendordep.html) page.

In this section we're going to attempt to determine the position of the robot on the game field based only on what is viewed from its camera.  I say attempt because this will involve some estimation, so our results may not be acceptable without a lot tuning. The plan is to use visual targets on the field to estimate the position of the robot. There are a several pieces that must come together to make this work so this section may be a little complex.

In order to pull this off we need to get data on the following things:
- The distance of the target from the camera.
- The position and orientation (Pose) of the target on the game field
- The Pose of the camera relative to the robot.
- The translation of the camera from the target.
- The transform of the camera to the target using the robot's gyro.

Before we start here're some housekeeping notes: 
- It's important to know that *PhotonCamera* constructor is expecting the name of the camera as listed on the UI.  

    PhotonCamera m_camera = new PhotonCamera("mmal_service_16.1");

- Here's the documentation for [PhotonLib](https://docs.photonvision.org/en/latest/docs/programming/index.html)

### Distance from Camera to Target
The return value from `calculateDistanceToTargetMeters()` is shown below.  There must be a height differential between goal and camera. The larger this differential, the more accurate the distance estimate will be.

    public static double calculateDistanceToTargetMeters(
            double cameraHeightMeters,
            double targetHeightMeters,
            double cameraPitchRadians,
            double targetPitchRadians) {
        return (targetHeightMeters - cameraHeightMeters)
                / Math.tan(cameraPitchRadians + targetPitchRadians);
    }

See [estimating distance](https://docs.limelightvision.io/en/latest/cs_estimating_distance.html) from the Limelight documentation for details on how this distance is derived.

![Distance to Target](../../images/FRCVision/FRCVision.021.jpeg)

### Camera to Target Translation

![Camera to Target Translation](../../images/FRCVision/FRCVision.022.jpeg)

### Camera to Robot and Field to Target

![Camera to Robot](../../images/FRCVision/FRCVision.023.jpeg)

### The Pose2d of the Robot on the Field

![Robot Position on Field](../../images/FRCVision/FRCVision.024.jpeg)


## References

- PhotonVision - [Install on the Romi](https://docs.photonvision.org/en/latest/docs/getting-started/installation/sw_install/romi.html)

- PhotonVision - [Documentation](https://docs.photonvision.org/en/latest/)

- Limelight - [Documentation](https://docs.limelightvision.io/en/latest/index.html)
