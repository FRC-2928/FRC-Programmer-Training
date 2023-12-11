# Configuring Motors and Encoders
Our team primarily uses the **Falcon 500** motors with integrated **Talon FX** controllers and the **Talon SRX** controllers.  The Falcon 500 is a brushless motor, custom designed specifically for the FIRST Robotics Competition. The Talon SRX controller can be used with **CIM** or **775pro** motors.  The Falcon 500 motors are more compact and require less wiring.

![Motors and Controllers](../images/FRCroboRIO/FRCroboRIO.006.jpeg)

These motors have a lot of features that need to be configured. This can be done using the [Phoenix6 Tuner](https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/index.html) or from the your program code.  Although you can set them up in the Tuner during testing, it's highly recommended to ultimately set them via the program code. This way, in the event a device is replaced, you can rely on your software to properly configure the new device, without having to remember to use Tuner to apply the correct values.

The code segment below shows how the motors are configured for our differential drivetrain.  To configure the motors we create a configuration object of a class called *TalonFXConfiguration* that is then applied to each motor.  We first apply a new *TalonFXConfiguration* object to each motor controller to ensure that it is restored to a known state, thus allowing you to only configure the settings that you intend to change. There are four motors to configure so we can apply the default within a parameterized loop so as not to duplicate code.   Here's an explaination of some of the other configuration parameters.

- The `OpenLoopRamps.DutyCycleOpenLoopRampPeriod` prevents instantaneous changes in throttle.  See [Ramping](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configopenloopramp#ramping) for more information.

<!-- - A device’s neutral deadband is the region where the controller demotes its output to neutral. This is configured using the `configNeutralDeadband()` function. See [Neutral Deadband](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configneutraldeadband#neutral-deadband) in the Phoenix documentation. -->

- The `Feedback.FeedbackSensorSource` parameter is used to select the encoder that's used with the motor.  The Talon FX has a sensor integrated into the controller. This is necessary for the brushless commutation and allows the user to use the Talon FX with a high resolution sensor without attaching any extra hardware. The default is *RotorSensor*, which uses the internal rotor sensor in the Talon FX.

 - Implement motor safety via `setSafetyEnabled()`.  See [Motor Safety](https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/wpilib-integration/motorcontroller-integration.html#motor-safety) in the Phoenix6 documentation.

You'll learn about some of the other configuration parameters during the lab.

    for (TalonFX fx : new TalonFX[] { this.leftLeader, this.leftFollower, this.rightLeader, this.rightFollower }) {    
      // Apply default configuration
      fx.getConfigurator().apply(new TalonFXConfiguration());     
    }

    /* Configure the devices */
    var leftConfiguration = new TalonFXConfiguration();
    var rightConfiguration = new TalonFXConfiguration();

    leftConfiguration.OpenLoopRamps.DutyCycleOpenLoopRampPeriod = 0.1;  
    rightConfiguration.OpenLoopRamps.DutyCycleOpenLoopRampPeriod = 0.1; 

    leftConfiguration.Feedback.FeedbackSensorSource = FeedbackSensorSourceValue.RotorSensor;
    rightConfiguration.Feedback.FeedbackSensorSource = FeedbackSensorSourceValue.RotorSensor;

    leftConfiguration.MotorOutput.NeutralMode = NeutralModeValue.Brake;
    rightConfiguration.MotorOutput.NeutralMode = NeutralModeValue.Brake;

    // Have the wheels on each side of the drivetrain run in opposite directions
    leftConfiguration.MotorOutput.Inverted = InvertedValue.CounterClockwise_Positive;
    rightConfiguration.MotorOutput.Inverted = InvertedValue.Clockwise_Positive;

    // Apply the configuration to the wheels
    this.leftLeader.getConfigurator().apply(leftConfiguration);
    this.leftFollower.getConfigurator().apply(leftConfiguration);
    this.rightLeader.getConfigurator().apply(rightConfiguration);
    this.rightFollower.getConfigurator().apply(rightConfiguration);

    // Set up followers to follow leaders
    this.leftFollower.setControl(new Follower(leftLeader.getDeviceID(), false));
    this.rightFollower.setControl(new Follower(rightLeader.getDeviceID(), false));
  
    // Enable safety
    this.leftLeader.setSafetyEnabled(true);
    this.rightLeader.setSafetyEnabled(true);

Each motor will play a different roll depending on its position in the drivetrain.  For a differential drive robot the motors on the left side of the drivetrain would need to be going in the opposite direction of those on the right. In addition, we want the rear motors to exactly follow the commands sent to the front motors.   

To invert the motors on one side of the drivetrain we use the *MotorOutput.Inverted* configuration parameter. A class called *InvertedValue* is provided to specify the direction of the wheels.

    leftConfiguration.MotorOutput.Inverted = InvertedValue.CounterClockwise_Positive;
    rightConfiguration.MotorOutput.Inverted = InvertedValue.Clockwise_Positive;

To get the rear motors to follow the front motors we use the *Follower* class and the `setControl()` function.  The rear motor is sent the device ID of the motor to follow.  This is set to `false`.  

    this.leftFollower.setControl(new Follower(leftLeader.getDeviceID(), false));
    this.rightFollower.setControl(new Follower(rightLeader.getDeviceID(), false));
  

<!-- Detailed information can be found in [Open-Loop Features](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=setInverted#open-loop-features) of the Phoenix documentation. -->

The Talon FX has a new set of inverts that are specific to it, see [Talon FX Specific Inverts](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=talonfxinverttype#talon-fx-specific-inverts).

![Motor Configuration](../images/FRCroboRIO/FRCroboRIO.004.jpeg)

## Remote Sensors
The TalonFX/SRX can execute closed-loop modes with sensor values sourced by other Talons, CANifiers, or Pigeon IMUs on the same CAN-bus.  This is useful when you want to use Auxiliary PID1 Closed-Loop (differential mechanisms) that requires more than one sensor source (including MotionProfileArc, and all other Closed-Loop control modes).

See [Bring Up: Remote Sensors](https://docs.ctre-phoenix.com/en/latest/ch14a_BringUpRemoteSensors.html#bring-up-remote-sensors)

Select what remote device and signal to assign to Remote Sensor 0 or Remote Sensor 1. After binding a remote device and signal to Remote Sensor X, you may select Remote Sensor X as a PID source for closed-loop features.  Once the remote sensor has been assigned you can select the feedback device for the motor controller.

    talonLeader.configRemoteFeedbackFilter(m_pigeon.getDeviceID(), 
                                    RemoteSensorSource.Pigeon_Pitch, 
                                    0);

    talonLeader.configSelectedFeedbackSensor(FeedbackDevice.RemoteSensor0);

For instance, if using an absolute magnetic encoder you can use the following command.

    fx.configSelectedFeedbackSensor(FeedbackDevice.CTRE_MagEncoder_Absolute);

## Limit Switches
Configure the limit switch:

    // Use a limit switch connected directly to the motor controller
    talon1.configForwardLimitSwitchSource(LimitSwitchSource.FeedbackConnector, 
                                            LimitSwitchNormal.NormallyOpen, 0);

    talon1.configReverseLimitSwitchSource(LimitSwitchSource.FeedbackConnector, 
                                            LimitSwitchNormal.NormallyOpen, 0); 

    talon1.setInverted(true);
    talon1.setSensorPhase(true);

    talon1.configForwardSoftLimitThreshold(7300);
    talon1.configReverseSoftLimitThreshold(-7300);
    talon1.configForwardSoftLimitEnable(false, 0);
    talon1.configReverseSoftLimitEnable(false, 0);
    talon1.overrideSoftLimitsEnable(false);

Test limit switch:

    public boolean isLimitSwitchClosed(){
        return talon1.getSensorCollection().isFwdLimitSwitchClosed() == 1;
    }

Enable limit switch:

    public void setLimitSwitchEnabled(){
        talon1.overrideLimitSwitchesEnable(true);
    }

## Lab - Configure Motors
<!-- In this lab your task is to research some of the motor configuration parameters.  Go to the [Phoenix Documentation Website](https://docs.ctre-phoenix.com/en/latest/index.html).  Use the search field to find results for the `setNeutralMode()`, `configSupplyCurrentLimit()`, `configSelectedFeedbackSensor()` motor configuration parameters and read the information provided.  You may have to do a page search after clicking on the result to find the parameter.  After doing your research consider the following questions.

1. What are the two Netural Modes set by `setNeutralMode()` and what is the difference between them?  Switch the Netural Mode from **Coast** to **Brake** and observe the difference while driving the robot.

2. What condition does the `configSupplyCurrentLimit()` setting try to prevent?

3. What does the `configSelectedFeedbackSensor()` configuration do?  What are the default sensors for the TalonFX and TalonSRX?  See [Talon FX/SRX Sensors](https://docs.ctre-phoenix.com/en/latest/ch14_MCSensor.html?highlight=configSelectedFeedbackSensor#bring-up-talon-fx-srx-sensors) in the Phoenix documentation.

Once you understand the configuration parameters you're done with this task! -->

## References

- CTRE - [Phoenix6](https://pro.docs.ctr-electronics.com/en/latest/) documentation.

- CTRE - [Bring Up: Talon FX/SRX and Victor SPX](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html)

- CTRE - [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner)

- FRC Documentation [Motors APIs](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/index.html)

- CTRE - [Falcon 500 Motor User Guide](https://robotics.choate.edu/wp-content/uploads/2020/01/Falcon500UserGuide-20191101.pdf)

- CTRE - [Talon SRX - User’s Guide](https://store.ctr-electronics.com/content/user-manual/Talon%20SRX%20User's%20Guide.pdf)