# Installing Phoenix API Libraries

To install the Pheonix API libraries copy the link found on [Phoenix Releases](https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html) page.  You'll need to click on the **Online** tab to find the link.  Click on the WPI pallet and type **Manage Vendor Libraries** then click on the menu option that appears. Click **Install new libraries (online)** and a textbox should appear. Paste the link into the textbox.

<!-- The **CTRE Phoenix Framework Installer** is a zip file download that contains files that must be installed into the `wpilib/20YY` directory.  Here are the [Phoenix Software](https://pro.docs.ctr-electronics.com/en/latest/docs/installation/installation.html) releases for each platform. 

When you run the Phoenix software installer for Windows it installs the software in the requisit directories.  For Mac and Linux follow the *Install Phoenix on Mac and Linux* instructions below once you've downloaded the zip file.

Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option.  If you're upgrading a project from a previous year then you must first delete the `Phoenix.json`. Then select **Check for Updates Offline** since you have already the libraries installed in the `wpilib/20YY` directory. This should install a new `Phoenix.json` file with the new release.  This is detailed in the FRC documentation [Adding Offline Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#adding-offline-libraries) and also in the CTRE Documentation to [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html).

### Install Phoenix on Mac and Linux
There are three folders contained in this archive-
- "maven"
- "vendordeps"
- "Robotbuilder"

Place the `maven/com/ctre` and `vendordeps/Pheonix6.json` folders inside of the respective directories of the WPILib installation folder.  This is typically located in `~/wpilib/202Y` directory. -->

<!-- Instructions on adding Phoenix to your FRC vscode project can be found [here](https://phoenix-documentation.readthedocs.io/en/latest/ch05a_CppJava.html). -->

<!-- The **Robotbuilder** folder contains Robotbuilder extensions for using CTRE classes. It currently contains the Talon FX, CANCoder, Talon SRX, Victor SPX, CANifier, and Pigeon IMU extensions. Take the desired extension folder (eg. "Talon SRX") and place it in `~\wpilib\2022\Robotbuilder\extensions`. -->

### Migrating to Phoenix 6
To migrate a project to Phoenix6 API follow these steps:

- Remove the older phoenix libraries. Go to `vendordeps` folder and delete the `phoenix.json` file.

- Install [Phoenix 6](https://pro.docs.ctr-electronics.com/en/latest/docs/installation/installation.html) libraries in VSCode. See above.  

- Remove any red-underlined imports in your code.

- Remove the `WPI_` from all of the class definitions.  Use the mouse over to `import` new classes.  Note that the second `c` in the *CANcoder* class is lowercase.

- Update your code to apply the new syntax for configuring Phoenix hardware.  See [Configuration](https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/api-usage/configuration.html) Phoenix6 documentation. Also see [Configuring Motors and Encoders](../RoboRIO/motorConfig.md) in this documentation.

- Update your code to apply hardware control requests.  See the [Control Requests](https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/api-usage/control-requests.html) documentation.  Also see [Motor Control](../RoboRIO/motorControl.md) in this documentation.

Another resource is the [Phoenix 6](https://github.com/CrossTheRoadElec/Phoenix6-Examples/tree/main/java) example code on GitHub.  Also see [Phoenix 6 Migration](https://pro.docs.ctr-electronics.com/en/latest/docs/migration/migration-guide/index.html) in the Phoenix documentation.

In order to select data values (referred to as *Signal Values*) from the Phoenix hardware you need to import these classes:

    import com.ctre.phoenix6.BaseStatusSignal;
    import com.ctre.phoenix6.StatusSignal;

## Installing the Phoenix Tuner
Phoenix Tuner is the companion application allowing you to configure, analyze, update and control device. See [Phoenix 6 Tuner X](https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/index.html) for installing the tuner.

There are multiple ways to connect the tuner to the RoboRio.  If you have a USB cable connected it defaults to `172.22.11.2` which is the roboRIO IP when connected via USB.  You can also connect via the *DriverStation* as long as it's connected to the RoboRio.  If the RoboRio is newly imaged and there is no robot program running you'll need to bring up a *Temporary Diagnostic Server*, which is done from the Tuner.  

![Connect Tuner X](../images/FRCroboRIO/FRCroboRIO.012.jpeg)

To upgrade the devices see the [Devices](https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/device-list.html) page.

The Tuner will automatically download the latest firmware when it's started.  See [Firmware Upgrade](https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/device-details-page.html#field-upgrade-firmware-version).

## Install REV Robotics Software
The install of the REV Robotics libraries is similar to that of the Phoenix software for Mac and Linux.  
- Download the zip file from the [REV Robotics Software](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information#c++-and-java) releases page.
- Place the `maven/com/revrobotics` and `vendordeps/REVLib.json` folders inside the WPILib installation folder. This is typically located in `~/wpilib/2022`.
- Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option, as explained above.

Note, that you may have to uninstall and reinstall the libraries using **Manage Vendor Libraries** -> **Manage Current Libraries** in order to get IntelliSense to recognise them.

## References

- CTRE - [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html)
