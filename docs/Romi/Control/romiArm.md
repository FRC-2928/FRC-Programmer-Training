# Romi Robot Arm
In this module we'll be creating code to control the [Robot Arm kit for Romi](https://www.pololu.com/product/3550) from Pololu. This program will be called [BasicArm](https://github.com/mjwhite8119/romi-examples/tree/main/BasicArm) which will include the following updates to the **RomiReference** project.

![BasicArm](../../images/Romi/Romi.029.jpeg)

Before reading this section please review the [SimpleArm documentation](https://github.com/bb-frc-workshops/romi-examples/tree/main/simpleArm) on the BB FRC Workshops site.

## Controlling the Robot Arm
This is how the software stack works together to operate the robot arm.
![Servo Control](../../images/Romi/Romi.006.jpeg)

A button is attached to each of the three arm components Lift, Tilt, and Gripper.  Every time a button is pressed the related arm component is moved.  This is done incrementally.  There are min and max limits to the range of motion of the arm.  The range of the servo is 0 to 180 degrees but that is too large a range for the arm.

The `Constants` file needs to be updated to include `Joystick` and `Arm` values.

![Arm Motor Control](../../images/Romi/Romi.005.jpeg)

## Robot Arm Startup
There's an issue with the robot arm when it's attached to the Romi with the FRC software stack installed.  The microcontroller will initiate prior to you starting your java program and it will activate the arm servos outside of their safe operating ranges.  This results in the arm crashing down onto the ground if the Romi Arm is built according to the instructions.  To prevent this the firmware on the Romi has to be updated.  

The problem is illustrated in the diagram below. The microcontroller gets the PWM signals for the servos from the *external IO buffers*.  See [Romi Onboard and External IO](../SC/romiIO.md) for a description of these. Whenever your java program is not running, or is *Disabled*, the `rPiLink.buffer.extIoValues` are set to zero. This translates to a servo position of 90 degrees, which is outside of the safe range of the servo.  Unfortunatelly, there's no way to control this from your java program since it's no longer connected to the microcontroller.  The only way to resolve this problem is to modify the microcontroller firmware.

![Arm Motor Range](../../images/Romi/Romi.021.jpeg)

The firmware has been updated to prevent the servos from going outside of their safe ranges and includes three new functions: `initializeArm(), incrementWrites()`, and `doWritesForArm()`.  In order to use those functions you must press the **A Button** on the microcontroller to switch it into *Arm Mode*.  The modified [firmware program](https://github.com/mjwhite8119/wpilib-ws-robot-romi/blob/main/firmware/src/main.cpp) can be viewed on Github. To install this version of the firmware follow the instructions in the [Romi Firmware](../RomiSoftware/romiFirmware.md) module.

Follow this startup procedure.

1. Make sure that the power to the arm servos is disconnected when you switch on the Romi.  

2. Press the **A Button** on the microcontroller.

3. Connect the power to the servos.

4. Start your java program. 

Currently, the Romi will remain in *Arm Mode* until it is restarted.

## Adding an Arm Position Command
The JoystickArmCommand incrementally moves the robot arm components as you press buttons on the joystick.  It would be useful to have a command that move the arm to the fully up or down position with a single button press. 

Create a new command called **ArmPosition**.  In the VSCode file menu right click on the **commands** folder and select "Create a new class/command".  Enter the name **ArmPosition** in the box.  This will give you a template for creating your new command, which will include the class constructor and the **initialize(), execute(), end(),** and **isFinished()** methods.  

Before filling out these methods with code let's think about what subsystems and components we'll need to create this command.  We'll certainly need the Arm subsystem.  The Arm will move in response to a button pressed on the Joystick so we'll also need to get a reference to that.

Import the Arm and Joystick command at the top of the file and create a private variable to each of them inside of the class.  

Example...

Pass the Arm and Joysick into the constructor and set the Arm as a requirement.  Also assign the private arm and joystick variables to the parameters that you just passed in.

Example...

Now let's figure out what we're going to do with these. 

## Testing the Arm Command
We can test the Arm command either by using the joystick or from Shuffleboard.  We'll setup our program to use both methods.  
To use the joystick

To use Shuffleboard add the following code to the `RobotContainer` class.

## References
[Robot Arm kit for Romi](https://www.pololu.com/product/3550)

[Romi Arm example - github](https://github.com/Pearadox/Romi/tree/master/src/main/java/frc/robot)

<h3><span style="float:left">
<a href="romiCode7">Previous</a></span>
<span style="float:right">
<a href="colorSensor">Next</a></span></h3>