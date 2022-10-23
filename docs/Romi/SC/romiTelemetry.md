# Telemetry
When you are running code on a robot it's very useful to track how data is passing through the system as it operates.  This is referred to as *Telemetry*, which is the science of automatically collecting measurements and transmitting them to a receiving station.  Telemetry is critical for fine tuning a system and figuring out why a system isn't operating the way is should. Remember that robots are data driven machines, so in order to test our code we'll need to see the data.  

When creating Commands and Subsystems for our robot it's important to build in telemetry functions so as to see the data that the system is generating. The FRC documentaton has instructions on [Adding Telemetry to Robot Code](https://docs.wpilib.org/en/stable/docs/software/telemetry/telemetry.html#adding-telemetry-to-robot-code). Most of this is done using the *Sendable* interface provided by WPILib that is documented in [Robot Telemetry with Sendable](https://docs.wpilib.org/en/stable/docs/software/telemetry/robot-telemetry-with-sendable.html#robot-telemetry-with-sendable). The Sendable interface automatically sends telemetry values every robot loop, removing the need to handle the periodic logic of sending and receiving values from the dashboard.  For simple data structures you can also directly output values to the SmartDashboard from the `periodic()` loop.  

## Tracking Robot Velocity and Heading
Let's create some simple telemetry to track the current position and heading of the robot.  The data will be output to the *SmartDashboard*, which is now part of Shuffleboard.  Putting the data out to SmartDashboard will automatically place it into the Network Tables.  Whenever you create telemetry data it's preferrable to create a function to keep it all in one place.  We'll output details for the left and right encoders together with the robot heading.
 
    public void publishTelemetry() {
        
        // Display the meters per/second for each wheel and the heading
        SmartDashboard.putNumber("Left Encoder Velocity", m_leftEncoder.getRate());
        SmartDashboard.putNumber("Right Encoder Velocity", m_rightEncoder.getRate());
        SmartDashboard.putNumber("Heading", getHeading());
    }

This function can now be called from the `periodic()` method, which will place it in the robot's process loop.  Since the process loop runs 50 times per/second we'll see a continuous stream of changing data as the robot moves.

    public void periodic() {
        publishTelemetry();
    }

## Shuffleboard
FRC has developed a tool called *Shuffleboard*, which allows you to view all of the data that's generated from the robot in Real-Time.  It also enables you to send data to the robot in order to make commands more flexible and change the behaviour of the robot.

The documentation explains how to [start Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/getting-started/shuffleboard-tour.html#starting-shuffleboard) depending on your development laptop.  

    Note for MacOS

    If it crashes from *Start Tool* on MacOS you have to use `~/wpilib/2022/jdk/bin/java -jar ~/wpilib/2022/tools/shuffleboard.jar` 

We can configure how Shuffleboard displays data from our robot code.  Shuffleboard enables you create multiple tabs that lets you to view data in a logical fashion.  The following code shows how to create a new Shuffleboard tab and add some telemetry data to it.  The tab will be called **Drivetrain**, which will allow us to separate out data for that subsystem.  See the [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html) documentation for details on the code syntax.


    private void setupShuffleboard() {
        // Create a tab for the Drivetrain
        ShuffleboardTab m_driveTab = Shuffleboard.getTab("Drivetrain");
        m_headingEntry = m_driveTab.add("Heading Deg.", getHeading())
            .withWidget(BuiltInWidgets.kGraph)      
            .withSize(3,3)
            .withPosition(0, 0)
            .getEntry();  
    }

In order to keep all of our telemetry data code in one place we have created the method `setupShuffleboard()`.  This method gets called from the Drivetrain's constructor to make it visible in Shuffleboard the robot starts up.

    public Drivetrain() {
        ...
        setupShuffleboard();
    }

Telemetry data needs to be put in the NetworkTables to become visible in Shuffleboard.  The following entry is placed in the `publishTelemetry()` method of the Drivetrain.  This statement will populate the NetworkTable entriy every 50 milliseconds giving you a Real-Time view of the data.

    m_headingEntry.setDouble(getHeading());


## Telemetry Lab
In this lab we'll add some more telemetry data to the **Drivetrain** tab in Shuffleboard.  Add two variables `m_leftWheelPositionEntry` and `m_rightWheelPositionEntry` to track how far each wheel has travelled.  Use the methods `getLeftDistanceMeters()` and `getRightDistanceMeters()` to populate the data entries.  Follow the example above to add the entries to the Shuffleboard tab.

[Telemetry Lab Solution](solutionTelemetry.md)

## References
- FRC Documentation - [Telemetry](https://docs.wpilib.org/en/stable/docs/software/telemetry/index.html)

- FRC Documentation - [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html)

- Code Example - [RomiShuffleboard](https://github.com/FRC-2928/RomiExamples/tree/main/RomiDrivetrainBase)

<!-- <h3><span style="float:left">
<a href="romiCommandGroups">Previous</a></span>
<span style="float:right">
<a href="romiPID">Next</a></span></h3> -->