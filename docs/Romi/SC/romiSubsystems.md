# Subsystems

Subsystems are the basic units of functionally for our robot, such as drive trains and mechanical arms.  They encapsulate low-level hardware objects (motor controllers, sensors, etc) and provide methods that can be used by Commands.  We represent subsystems in code by using [Objects](../../Programming/objects.md). Objects are composed of [Data Structures](../../Programming/dataStructures.md) and [Procedures](../../Programming/procedures.md), as explained in [Introduction to Programming](../../Programming/introProgramming.md). You should learn those concepts before we look at the subsystems that are on the Romi.  

 For this module the majority of the code implementation can be found in the [Subsystems](https://docs.wpilib.org/en/latest/docs/software/commandbased/subsystems.html) section of the FRC Documentation.  

![Subsystems](../../images/Romi/Romi.012.jpeg)

## DriveTrain Subsystem
The Romi uses a drive train called a [Differential Drive](https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html#using-the-differentialdrive-class-to-control-differential-drive-robots). There are three types of Differential [Drive Train Modes](https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html#drive-modes) that can be implemented. The Romi will make use of at least two of them. To understand what type of maneuvers are possible using a Differential Drive robots you can look at the [Robot Kinematics](../../Concepts/Dynamics/kinematics.md) module. The *DifferentialDrive* class takes the left and right motors as parameters that are wrapped in the *PWMSpeedController* class.  The primary task of the *DifferentialDrive* class is to convert a single speed (unicycle model) into speed for the left and right side of the chassis.

![Differential Drive](../../images/Romi/Romi.038.jpeg)

The primary job of the *Drivetrain* subsystem is to send speed commands to its motors.  This is done in the `arcadeDrive()` method where we pass in the required translational and rotational speed.  The *DifferentialDrive* object will take care of controlling the speed to the left and right motors based on the kinematics of the drivetrain type. 

    public void arcadeDrive(double xaxisSpeed, double zaxisRotate) {
        m_diffDrive.arcadeDrive(xaxisSpeed, zaxisRotate);
      }

Other procedures in the Drivetrain class will take care of resetting and reading the wheel encoders.  It'll also translate the wheel encoder values into distances, as explained in the [Pose Estimation](../Concepts/OptimalEstimation/odometry) module.  Also checkout [Using the WPILib Classes to Drive your Robot](https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html?highlight=speedcontroller#multi-motor-differentialdrive-with-speedcontrollergroups) for more information on implementing a drive train in FRC.

## RomiGyro Subsystem
The RomiGyro subsystem reads values from its gyro in order to perform [Pose Estimation](../../Concepts/OptimalEstimation/poseEstimation.md). The raw data that comes from gyros is very complex and difficult to intepret.  The RomiGyro subsystem translates the data into simple angles and rates-of-turn that are much easier to understand.

## Subsystems Lab
There are three tasks for this lab:

- Update the RomiGyro to make use of an Interface.
- Create a method to get the current heading of the robot.
- Add comments to divide the *Drivetrain* class into subsections.

### Add Interface to RomiGyro
For this update we will use the *Gyro* [interface](https://www.w3schools.com/java/java_interface.asp) supplied by WPILib.  An interface is an [abstract class](https://www.w3schools.com/java/java_abstract.asp) used to group related methods that specify the behavior of a class.  You can think of an interface as a blueprint of a class.  Like a class, an *interface* can have methods and variables, but the methods declared in an interface are by default abstract which means that it only has the name of the method and not the code to implement it.  The code gets implemented by the class that `implements` it. 

[RomiGyro Interface solution](solutionRomiGyro.md)

### <a name="heading"></a>Create Heading Method
In future modules we're going to need to get the current heading of the Drivetrain.  This heading is obtained from the Gyro that is defined as a subsystem of the Drivetrain as indicated above.

[Add heading solution](solutionCreateHeading.md)

### Add Comments
In the Drivetrain class we'll add the following comments to make it easier to find methods that are logically related.  The comment sections will be:

- **Initialization** - Includes everything required to construct and initialize the *Drivetrain* object.
- **Control Input** - Any method that causes the robot, or subsystem, to react in some way.
- **System State** - Methods that give information on what the current state of the *Drivetrain*  object is.
- **Process Logic** - Mostly logic that occurs during the `periodic()` loop, or processes/interprets incoming data before performing some kind of *Control Input* to the robot.  Incoming data will normally be provided by methods found in the *System State* subsection.

The comments will span most of the line width to form a separator and will look like this:

    // -----------------------------------------------------------
    // Control Input
    // -----------------------------------------------------------

Think about what code will go in each section.

## References
- FRC Documentation - [Subsystems](https://docs.wpilib.org/en/latest/docs/software/commandbased/subsystems.html)

- FRC Documentation - [Differential Drive Robots](https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html)

- FRC Documentation - [Using the WPILib Classes to Drive your Robot](https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html?highlight=speedcontroller#multi-motor-differentialdrive-with-speedcontrollergroups)

- Java Tutorial on [W3Schools](https://www.w3schools.com/java/default.asp)

- QUT Robot Academy [Measuring Motion](https://robotacademy.net.au/masterclass/measuring-motion/)