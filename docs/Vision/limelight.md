# Limelight Camara
Before using the Limelight you should [update the firmware](https://docs.limelightvision.io/docs/docs-limelight/getting-started/imaging).

Setup the networking, follow the [Comms/Networking Setup](https://docs.limelightvision.io/docs/docs-limelight/getting-started/networking) steps. Our interface can be reached at `http://10.TE.AM.11:5801/` after connecting to the robot's access point.  Where `TE.AM` is replaced with the team number, e.g. `29.28`.

## Limelight API
The first step to implementing *Limelight* in your project is to download the `LimelightHelpers.java` file.  This is a single-file library released by the Limelight team. All you need to do is copy this file from the [latest release](https://github.com/LimelightVision/limelightlib-wpijava/releases) into your Java project's `robot` folder. See [LimelightHelpers](https://github.com/LimelightVision/limelightlib-wpijava/tree/main) for more information.

## Robot Localization 

To localize the robot using metatags see [Robot Localization with MegaTag](https://docs.limelightvision.io/docs/docs-limelight/pipeline-apriltag/apriltag-robot-localization).


## References
- Limelight [Documentation](https://docs.limelightvision.io/docs/docs-limelight/getting-started/summary)

- [LimelightHelpers](https://github.com/LimelightVision/limelightlib-wpijava/tree/main) java file.