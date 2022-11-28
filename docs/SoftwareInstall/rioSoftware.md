# RoboRio and Device Software

The [NI-roboRIO](https://docs.wpilib.org/en/stable/docs/software/roborio-info/roborio-introduction.html#roborio-introduction) is the main robot controller used for FRC. The roboRIO serves as the “brain” for the robot running team-generated code that commands all of the other hardware.  Prior to using the **RoboRio** you have to install, or update, the software stack.  Here are the components that need to be installed.

![RoboRIO Software Stack](../../images/FRCTools/FRCTools.016.jpeg)

Here is a step-by-step guide for accomplishing the install.

![After Upgrade](../../images/FRCTools/FRCTools.017.jpeg)

#### Step 1. Install the FRC Game Tools
The first step is to [Install the FRC Game Tools](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html).  These can be down loaded from the [NI FRC Game Tools](https://www.ni.com/en-us/support/downloads/drivers/download.frc-game-tools.html#440024) releases site, and only supports Windows.  After the install it asks you to login to your **NI Activation** account and enter a serial number.  It's not clear why you need the account, but here's information on [getting the serial number](https://knowledge.ni.com/KnowledgeArticleDetails?id=kA00Z0000019OJTSA2&l=en-US).  Once you have the serial number you can use the [Activation Wizard](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/labview-setup.html#ni-activation-wizard). 

Installed tools include:  

- The RobiRIO Imaging Tool and images

- FRC Driver Station

#### Step 2. Image the RoboRio
The *Imaging Tool* is used to install the roboRIO Firmware and the Image.  The image is the equivalent of the operating system.  If you have the older roboRIO then all of the imaging is done from the *Imaging Tool*. If you have a roboRIO 2 then the first time it's imaged (or if the microSD card is replaced with a new card), it must be imaged using the [roboRIO 2.0 microSD](https://docs.wpilib.org/en/stable/docs/software/roborio-info/roborio2-imaging.html) card imaging process. If the RoboRio has already been imaged from a previous year and already has an SD card inserted then the Imaging Tool can be used to update the software. Open the [RoboRIO Imaging Tool](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html#roborio-imaging-tool) and follow the instructions.  

This step sets up the [RoboRIO Web Dashboard](https://docs.wpilib.org/en/stable/docs/software/roborio-info/roborio-web-dashboard.html#roborio-web-dashboard), which is a webpage built into the roboRIO that can be used for checking status and updating settings of the roboRIO. 

#### Step 3. Install Phoenix Framework Software

Install the [Phoenix Framework Software](https://store.ctr-electronics.com/software/) onto your PC.  This framework is only supported on Windows and includes:  

- The C++/Java Phoenix API.  These libraries get installed into the `~/wpilib/20XX` directory. For Linux and MacOS the C++/Java API can be installed separatelly.  See [VSCode Installing Pheonix Software](../../Tools/vscode.md#pheonix).
- Device Firmware Files (that were tested with the release)
- CTRE Support of RobotBuilder
- Phoenix Tuner
- Installs Phoenix Diagnostics Server into the RoboRIO (needed for CAN diagnostics).
- Plotter/Control features
- Self-test Snapshot
- Device ID and field-upgrade

#### Step 4. Install Phoenix Firmware
Once you have the **Phoenix Tuner** installed you can install/update the firmware on the CTRE hardware devices.  The firmware can be found in `C:\Users\Public\Documents\FRC`.  Or can be downloaded from the [Phoenix Framework Software](https://store.ctr-electronics.com/software/) releases page.  The firmware is documented in the [Firmware Release Notes](https://docs.ctre-phoenix.com/en/stable/ch22_SoftReleaseNote.html#ch22-softreleasenote).

Start the [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch05_PrepWorkstation.html#frc-windows-open-phoenix-tuner). Refer to the [Field Upgrade Devices](https://docs.ctre-phoenix.com/en/stable/ch08_BringUpCAN.html#field-upgrade-devices) section of the CTRE documentation.

#### Step 5. Prepare the Robot Controller
If no FRC Robot Application has been deployed to the RobiRIO then deploy the *Temporary Diagnostic Server* from the Phoenix Tuner.  Refer to [How to prepare Robot Controller](https://docs.ctre-phoenix.com/en/stable/ch06_PrepRobot.html#how-to-prepare-robot-controller).

## Programming the Radio
You can download the latest *FRC Radio Configuration Utility Installer* from the [Prerequisits](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/radio-programming.html#prerequisites) link.  See [Programming your Radio](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/radio-programming.html) in the FRC documentation.

## Lab - RoboRIO Software
This lab will get you familiar with how the software is installed and configured on the roboRIO.  There are three tasks for this lab:

- Understand the roboRIO Imaging Tool
- View the roboRIO Web Dashboard
- Learn about the FRC Radio Configuration Utility

### Understand the roboRIO Imaging Tool
Start the roboRIO robot and connect to it's IP address from your laptop.

Start the roboRIO Imaging Tool by clicking on the desktop icon. Once the tool starts make sure that it has connected to a *roboRIO Target*, top left panel. Also check the team number to see that it's correct.

Next, take a note of the current *Firmware Version* and *Image Version*. You'll need these in the next task. It's important that the firmware and image software is kept up-to-date. 

Answer the folling questions:

Is the Firmware version up-to-date?  The firmware should be at the required minimum version. See [Updating Firmware](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html#updating-firmware) to answer this question.

Is the Image version up-to-date?  The image software should reflect the year that we are currently in.  If it's not up-to-date, read [Imaging the roboRIO](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html#imaging-the-roborio) to learn how to update the Image software.  Work with a mentor or senior student programming lead to do the update.

![roboRIO Imaging Tool](../../images/FRCroboRIO/FRCroboRIO.001.jpeg)

Read the [roboRIO Imaging Tool](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html#roborio-imaging-tool) documentation and make sure that you understand each part of the tool.


### View the roboRIO Web Dashboard
The *roboRIO Web Dashboard* gives detailed information on the configuration of the roboRIO. Open a web browser and enter the address of the roboRIO into the address bar `172.22.11.2` for USB, or `roboRIO-2928-FRC.local`.

Compare the *Firmware Version* and the *Image Version* with what you recorded in the last task. They should be the same if you're connected to the same robot. Read the documentation on [System Configuration Tab](https://docs.wpilib.org/en/stable/docs/software/roborio-info/roborio-web-dashboard.html#system-configuration-tab) and understand the purpose of each setting. Do NOT change any of the settings.

Click on the Network icon in the left sidebar to view the network settings.  Read the [Network Settings Tab](https://docs.wpilib.org/en/stable/docs/software/roborio-info/roborio-web-dashboard.html#network-configuration) documentation to understand the purpose of each setting. Do NOT change any of the settings.

### Learn the FRC Radio Configuration Utility

Task to be written.

## References
- FRC Documentation [Imaging your roboRIO](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/imaging-your-roborio.html)

- FRC Documentation [Programming your Radio](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-3/radio-programming.html) 