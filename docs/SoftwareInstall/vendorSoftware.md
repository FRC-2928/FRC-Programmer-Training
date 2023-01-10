# Installing Phoenix Libraries

The **CTRE Phoenix Framework Installer** is a zip file download that contains files that must be installed into the `wpilib/20YY` directory.  Here are the [Phoenix Software](https://store.ctr-electronics.com/software/) releases for each platform. 

When you run the Phoenix software installer for Windows it installs the software in the requisit directories.  For Mac and Linux follow the *Install Phoenix on Mac and Linux* instructions below once you've downloaded the zip file.

Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option.  If you're upgrading a project from a previous year then you must first delete the `Phoenix.json`. Then select **Check for Updates Offline** since you have already the libraries installed in the `wpilib/20YY` directory. This should install a new `Phoenix.json` file with the new release.  This is detailed in the FRC documentation [Adding Offline Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#adding-offline-libraries) and also in the CTRE Documentation to [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html).

### Install Phoenix on Mac and Linux
There are three folders contained in this archive-
- "maven"
- "vendordeps"
- "Robotbuilder"

Place the `maven/com/ctre` and `vendordeps/Pheonix.json` folders inside of the respective directories of the WPILib installation folder.  This is typically located in `~/wpilib/202Y` directory.

Instructions on adding Phoenix to your FRC vscode project can be found [here](https://phoenix-documentation.readthedocs.io/en/latest/ch05a_CppJava.html).

The **Robotbuilder** folder contains Robotbuilder extensions for using CTRE classes. It currently contains the Talon FX, CANCoder, Talon SRX, Victor SPX, CANifier, and Pigeon IMU extensions. Take the desired extension folder (eg. "Talon SRX") and place it in `~\wpilib\2022\Robotbuilder\extensions`.

## Installing the Phoenix Tuner



## Install REV Robotics Software
The install of the REV Robotics libraries is similar to that of the Phoenix software for Mac and Linux.  
- Download the zip file from the [REV Robotics Software](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information#c++-and-java) releases page.
- Place the `maven/com/revrobotics` and `vendordeps/REVLib.json` folders inside the WPILib installation folder. This is typically located in `~/wpilib/2022`.
- Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option, as explained above.

Note, that you may have to uninstall and reinstall the libraries using **Manage Vendor Libraries** -> **Manage Current Libraries** in order to get IntelliSense to recognise them.

## References

- CTRE - [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html)
