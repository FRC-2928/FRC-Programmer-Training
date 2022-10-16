# Telemetry
When you are running code on a robot it's very useful to track how data is passing through the system as it operates.  This is referred to as *Telemetry*, which is the science of automatically collecting measurements and transmitting them to a receiving station.  Telemetry is critical to fine tuning a system and figuring out what went wrong in the event of a crash. Remember that robots are data driven machines, so in order to test our code we'll need to see the data.  

When creating Commands and Subsystems for our robot it's important to build in telemetry functions so as to see the data that the system is generating. The FRC documentaton has instructions on [Adding Telemetry to Robot Code](https://docs.wpilib.org/en/stable/docs/software/telemetry/telemetry.html#adding-telemetry-to-robot-code). Most of this is done using the Sendable class that is documented in [Robot Telemetry with Sendable](https://docs.wpilib.org/en/stable/docs/software/telemetry/robot-telemetry-with-sendable.html#robot-telemetry-with-sendable).


FRC has developed a tool called [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html), which will allow us to view all of the data coming from the robot in real time.  It also enables you to send data to the robot in order to make commands more flexible and change the behaviour of the robot.

The documentation explains how to [start Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/getting-started/shuffleboard-tour.html#starting-shuffleboard) depending on your development laptop.  

    Note for MacOS

    If it crashes from *Start Tool* on MacOS you have to use `~/wpilib/2022/jdk/bin/java -jar ~/wpilib/2022/tools/shuffleboard.jar` 

## Tracking Robot Position and Heading
Let's create some telemetry to track the current position and heading of the robot.  The data will be output to the *SmartDashboard*, which is now part of Shuffleboard.  Putting the data out to SmartDashboard will automatically place it into the Network Tables.  Whenever you create telemetry data it's preferrable to create a function to keep it all in one place.  We'll output details for the left and right encoders together with the robot heading.
 
    public void publishTelemetry() {
        
        // Display the meters per/second for each wheel and the heading
        SmartDashboard.putNumber("Left Encoder Velocity", m_leftEncoder.getRate());
        SmartDashboard.putNumber("Right Encoder Velocity", m_rightEncoder.getRate());
        SmartDashboard.putNumber("Heading", getHeading());
    }

This function can now be called from the `periodic()` method, which will place it in the robot's process loop.  Since the process loop runs 50 times per/second we'll see a continuous stream of changing data as the robot moves.

    public void periodic() {
        // This is added in the Telemetry tutorial.
        publishTelemetry();
    }

<!-- ## Parameterizing the AutoDistance Command
We're going to enhance the *AutoDistance* command in order to accept input from Shuffleboard.  The current command moves the robot forward and backward 10 inches but with the help of Shuffleboard we'll be able to control how far the robot moves. -->


## Telemetry Lab

<!-- Switch direction

Convert everything to meters because that's what we'll use for characterization.

Exercise - Have angle accept input from Shuffleboard

Exercise - Travel in a square, need new command AutonomousSquare. -->

## References
- FRC Documentation - [Telemetry](https://docs.wpilib.org/en/stable/docs/software/telemetry/index.html)

- FRC Documentation - [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html)

- Code Example - [RomiShuffleboard](https://github.com/FRC-2928/RomiExamples/tree/main/RomiDrivetrainBase)

<!-- <h3><span style="float:left">
<a href="romiCommandGroups">Previous</a></span>
<span style="float:right">
<a href="romiPID">Next</a></span></h3> -->