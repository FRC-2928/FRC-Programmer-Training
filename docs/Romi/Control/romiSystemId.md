# System Identification
*System Identification* is the process of determining a mathematical model for the behavior of a system through statistical analysis of its inputs and outputs.  FRC has developed the [System Identification Tool](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/introduction.html#introduction-to-robot-characterization) to aid in identifying the characterics of our robot systems.  Before starting this section you should view the [System Identification](https://www.youtube.com/playlist?list=PLlmlmzye-q9gC7DwJ0xObASeOWUeADxJW) videos by Brian Douglas.

## Running the System Identification Tool
The *System Identification Tool* (SysId) can be opened from the **Start Tool** option in VS Code. On MacOS you'll need to start it from the terminal.  Go to `~/wpilib/2022/tools` and run `python3 SysID.py`. 

Starting the tool creates a directory under your project called `.SysId`.

## Configuring the Project
The first step is to [Configuring the Project](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/configuring-project.html#configuring-a-project). 

## Deploying the Project

[Deploying the Project](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/configuring-project.html#deploying-project).

## Analysis the Data
[Analysing Data](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/analyzing-data.html)

## Lab - System Identification

Lab to be written.

<!-- ### Driving the Motors
One problem with this profiled motion command is that the robot more than likely doesn't drive straight.  Also, in the *DriveDistancePID* command we had to add a value to the **I** parameter in order to get the command to finish (reach its setpoint).  To fix these problems we'll create a new method in the *Drivetrain* class called `steerVelocity()` that calculates a voltage value for each wheel, which is then sent to the motors `setVoltage()` method. -->

<!-- Our previous PID command calculated a percentage value between `0.0` and `1.0` to drive the motors.  The *ProfiledPIDController* calculates a velocity, so we'll have to create a *Drivetrain* method to use velocity. 

Put picture here...

Create the method `tankDriveVolts()` to send a voltage to each of the motors.

    /**
    * Controls the left and right sides of the drive directly with voltages.
    * 
    * @param leftVolts the commanded left output
    * @param rightVolts the commanded right output
    */
    public void tankDriveVolts(double leftVolts, double rightVolts) {

      // Apply the voltage to the wheels
      m_leftMotor.setVoltage(leftVolts);
      m_rightMotor.setVoltage(rightVolts); 
      m_diffDrive.feed();
    }

Add the following constants to the *Constants* file.  The values are derived from doing *System Identification* on the Romi, which we'll learn about in the next module.  Information on the [SimpleMotorFeedforward](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/feedforward.html#simplemotorfeedforward) class can be found in the FRC documentation.

    // The linear inertia gain, volts
    public static final double ksVolts = 0.461;

    // The linear velocity gain, volts per (meter per second)
    // Increase this if you drive short
    public static final double kvVoltSecondsPerMeter = 6.93;

    // The linear acceleration gain, volts per (meter per second squared).
    public static final double kaVoltSecondsSquaredPerMeter = 0.0737;

    public static final SimpleMotorFeedforward kFeedForward = 
        new SimpleMotorFeedforward(ksVolts, 
                                    kvVoltSecondsPerMeter, 
                                    kaVoltSecondsSquaredPerMeter);

We're going to need a PID controller for each wheel, so create them as attributes of the *Drivetrain* class by placing them above the constructor.

    private final PIDController m_leftController =
      new PIDController(Constants.kPDriveProfiled, 
                        Constants.kIDriveProfiled, 
                        Constants.kDDriveProfiled);

    private final PIDController m_rightController =
      new PIDController(Constants.kPDriveProfiled, 
                        Constants.kIDriveProfiled, 
                        Constants.kDDriveProfiled);   

Now we have all of the components to create a new method that will accept a velocity and calculate a voltage value to be sent to each motor.

    /**
    * Drives a straight line at the requested velocity by applying feedforward
    * and PID output to maintain the velocity. This method calculates a voltage
    * value for each wheel, which is sent to the motors setVoltage() method.
    * 
    * @param velocity The velocity at which to drive
    */
    public void setOutputMetersPerSecond(double velocity) {
      
      // Calculate feedforward voltage
      double leftFeedforward = Constants.kFeedForward.calculate(velocity);
      double rightFeedforward = Constants.kFeedForward.calculate(velocity);
    
      // Send it through a PID controller
      double leftVelocity = m_leftController.calculate(m_leftEncoder.getRate(), velocity);
      double rightVelocity = m_rightController.calculate(m_rightEncoder.getRate(), velocity);
      
      // double calibratedRightSpeed = output * DrivetrainConstants.rightVoltsGain;
      tankDriveVolts(leftFeedforward + leftVelocity, rightFeedforward + rightVelocity);
    } -->

<!-- ## Robot Characterization Lab
Run robot characterization for your Romi.  Look at the results and add the following constants to your *Constants* file.  Note, that the values for your Romi may be different from the example below.

    // Dynamical constants
    public static final double kMaxSpeedMetersPerSecond = 0.5;
    public static final double kMaxAccelMetersPerSecondSquared = 0.5;

    // The linear velocity gain, volts per (meter per second)
    public static final double kvVoltSecondsPerMeter = 9.7;
    // The angular velocity gain, volts per (radians per second)
    public static final double kvVoltSecondsPerRadian = 0.345; -->

## References
- Videos [System Identification](https://www.youtube.com/playlist?list=PLlmlmzye-q9gC7DwJ0xObASeOWUeADxJW) by Brian Douglas

- FRC Documentation - [Robot Charactization](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-characterization/index.html)

- FRC Documentation - [Feedforward Control in WPILib](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/feedforward.html#feedforward-control-in-wpilib)

- Code Example - [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization)