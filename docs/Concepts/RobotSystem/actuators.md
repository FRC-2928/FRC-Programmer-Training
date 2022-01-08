# Actuators

Most of the actuators that are used for our competition robot are supplied by [Cross The Road Electronics CTRE](https://docs.ctre-phoenix.com/en/latest/index.html).  These mainly include the **TalonFX 500** for the drivetrain.  And the **Victor SPX** for onboard mechanisms.  Before using any of this hardware we need to install the additional 3rd-Party *Phoenix* software.


## Installing Software
The **Cross The Road Electronics** vendor libraries must be installed installed into the `wpilib/20YY` directory.  Here are the [Phoenix Software](https://store.ctr-electronics.com/software/) releases for each platform. 

When you run the Phoenix software installer for Windows it installs the software in the requisit directories.  For Mac and Linux follow the *Install Pheonix on Mac and Linux* instructions below once you've downloaded the zip file.

Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option.  This is detailed in the FRC documentation [Adding Offline Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#adding-offline-libraries) and also in the CTRE Documentation to [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html).

### Install Pheonix on Mac and Linux
There are three folders contained in this archive-
- "maven"
- "vendordeps"
- "Robotbuilder"

For VSCode Integration:
Place the `maven/com/ctre` and `vendordeps/Pheonix.json` folders inside the WPILib installation folder.
This is typically located in `~/wpilib/2022`.

Instructions on adding Phoenix to your FRC vscode project can be found [here](https://phoenix-documentation.readthedocs.io/en/latest/ch05a_CppJava.html).


For **Robotbuilder**:
The Robotbuilder folder contains Robotbuilder extensions for using CTRE classes.
It currently contains the Talon FX, CANCoder, Talon SRX, Victor SPX, CANifier, and Pigeon IMU extensions.

Take the desired extension folder (eg. "Talon SRX") and place it in `~\wpilib\2022\Robotbuilder\extensions`.

## TalonFX Falcon 500

Documentation for the [TalonFX Falcon 500](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html)


## References

- FRC Documentation [Motor Controllers](https://docs.wpilib.org/en/latest/docs/software/hardware-apis/motors/index.html)

- FRC Documentation - [Pneumatics](https://docs.wpilib.org/en/latest/docs/software/hardware-apis/pneumatics/pneumatics.html)

- CTRE [Documentation](https://docs.ctre-phoenix.com/en/latest/index.html)

- CTRE [Examples](https://github.com/CrossTheRoadElec/Phoenix-Examples-Languages) 

- CTRE [Phoenix Tuner](https://docs.ctre-phoenix.com/en/latest/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner)

- CTRE [Phoenix Software](https://store.ctr-electronics.com/software/)