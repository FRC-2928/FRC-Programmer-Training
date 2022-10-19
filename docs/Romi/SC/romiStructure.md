# Basic Robot Structure
By this point you should have your development environment setup and have ensured that you can communicate with the Romi robot by running the [example program](romiExample.md).  It's now time to get down the serious work of learning how to program the robot.  We're going to start with an [Introduction to Programming](../../Programming/introProgramming.md) where you'll get a high level overview of programming concepts.  Then we'll learn a little bit about [Robot Systems](../../Concepts/RobotSystem/intro.md) so as you can understand what it is that you're programming. This will prepare you to understand how the code of an FRC robot is constructed.  

Programs based on the WPILib library are organized around two fundamental concepts: **Subsystems** and **Commands**. Subsystems and Commands run within the context of an FRC Robot program that is built using four basic components.  Each component is kept in separate java files called `Main.java, Robot.java, RobotContainer.java`, and `Constants.java`.  

![Robot Structure](../../images/Romi/Romi.010.jpeg)

## The Main Class
The `Main.java` file is the starting point for the entire program and calls the Robot class to start building the robot. To learn more about the `Main` class refer to the [W3 Schools](https://www.w3schools.com/java/java_syntax.asp) tutorial.  In our code you should never need to change this file.

## Robot Class
The `Robot.java` file defines the Robot class and its purpose is to maintain the overall state of the robot.  It is implemented as a **State Machine** that represents the state of the robot at various points in time.  We touched on State Machines in [Introduction to Programming](../../Programming/introProgramming.md) but we'll learn a lot more about them later.

During a competition a robot will first **Initialize** and then go into **Autonomous** mode (state) for the first 15 seconds.  In this mode it will run **Autonomous Commands** which tell it what to do during this time period.  We'll be looking at Autonomous Commands in a later turorial.  After 15 seconds, the robot will switch into **Teleoperated** mode.  During this time the driver will operate the robot using a joystick controller.  At the end of the competition the robot will go into **Disabled** mode.  During each of these modes a [Command Scheduler](https://docs.wpilib.org/en/latest/docs/software/commandbased/command-scheduler.html) will run that is responsible for running commands.

![Robot Class](../../images/Romi/Romi.011.jpeg)

## RobotContainer Class

The `RobotContainer.java` file is where the main structure of the robot is built.  It defines Subsystems and their Default Commands.  It sets up Autonomous Commands that are run during the Autonomous mode of the competition.  The RobotContainer is also where mappings between the Joystick and various Commands are defined.  In our Romi example project this class will also define Shuffleboard options that allow you to control what happens during the Autonomous mode of the competition.  We'll learn a lot more about Shuffleboard later.

![RobotContainer Class](../../images/Romi/Romi.014.jpeg)

## The Constants File

The `Constants.java` file provides a central location for defining constants.  Constants are values assigned to variables that do not change throughout the execution of the program.  

## <a name="joystick"></a>Joystick Controllers
A joystick/gamepad can be connected to your laptop via a USB port or Bluetooth.  The WPI Library provides all of the necessary software to interface with XBox, PS3/4, or Logitech game controllers. Detailed information on [Joysticks](https://docs.wpilib.org/en/latest/docs/software/basic-programming/joystick.html) can be found in the FRC Documentation.

![Joysticks](../../images/Romi/Romi.032.jpeg)

The Joystick object is created in the *RobotContainer* class.

    private final Joystick m_joystick = new Joystick(0);

To use the Xbox controller use this initialization:

    private final XboxController m_joystick = new XboxController(0);

A method is created in the class that uses the joystick to control the robot:

    public Command getArcadeDriveCommand() {
        return new ArcadeDrive(
            m_drivetrain, () -> -m_joystick.getRawAxis(1), () -> m_controller.getRawAxis(2));
    }

You may need to change the second `getRawAxis()` to suit your controller.    

## Lab - Robot Structure.
There are three tasks for this lab:

- Modify the *Drivetrain* class to change inches to meters.
- Move constants to the *Constants* file.
- Rename the Joystick variable.

### Move constants to Constants File
There are a few values in the *romiReference* program that really should be in the Constants file.  Move the two constants `kWheelDiameterInch` and `kCountsPerRevolution` that are in the DriveTrain class into the Constants.java file. Constants are defined as [public](https://www.w3schools.com/java/ref_keyword_public.asp) [static](https://www.w3schools.com/java/ref_keyword_static.asp) [final](https://www.w3schools.com/java/ref_keyword_final.asp) in Java.  The `static` keyword creates attributes that can be accessed without creating an object of a class, whereas the `final` keyword makes attributes non-changeable.  You can declare any classes, attributes, methods and constructors, as `public` making them accessible by any other class.

[Move constants solution](solutionMoveConstants.md)
### Change Inches to Meters
This is an exercise to change the distance from inches to meters. You'll learn about object methods, which are methods that can only be accessed after you have created a class object.

[Inches to meters solution](solutionInchMeters.md)

### Rename Joystick Variable
Also, we should change the name of the Joystick variable name to `m_joystick` instead of `m_controller`.  In later lessons a *Controller* is a class that manages the movement of the robot.  Renaming this variable to `m_joystick` will avoid future confusion.

## References
- FRC Documentation - [Structuring a Command-Based Robot Project](https://docs.wpilib.org/en/latest/docs/software/commandbased/structuring-command-based-project.html?highlight=RobotContainer)

- FRC Documentation - [Joysticks](https://docs.wpilib.org/en/latest/docs/software/basic-programming/joystick.html)

- FRC Documentation - [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html)

- Code Example - [DrivetrainBase](https://github.com/FRC-2928/RomiExamples/tree/main/RomiDrivetrainBase)

<!-- <h3><span style="float:left">
<a href="romiExample">Previous</a></span>
<span style="float:right">
<a href="romiJoysticks">Next</a></span></h3> -->
