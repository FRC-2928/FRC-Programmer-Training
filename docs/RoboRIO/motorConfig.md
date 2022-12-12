# Configuring Motors and Encoders
Our team primarily uses the **Falcon 500** motors with integrated **Talon FX** controllers and the **Talon SRX** controllers.  The Falcon 500 is a brushless motor, custom designed specifically for the FIRST Robotics Competition. The Talon SRX controller can be used with **CIM** or **775pro** motors.  The Falcon 500 motors are more compact and require less wiring.

![Motors and Controllers](../../images/FRCroboRIO/FRCroboRIO.006.jpeg)

These motors have a lot of features that need to be configured. This can be done using the [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner) or from the your program code.  Although you can set them up in the Tuner during testing, it's highly recommended to ultimately set them via the program code. This way, in the event a device is replaced, you can rely on your software to properly configure the new device, without having to remember to use Tuner to apply the correct values.

The code segment below shows how the motors are configured for our drivetrain.  There are four motors to configure, which is done within a parameterized loop so as not to duplicate code. We first call the `configFactoryDefault()` routine to ensure motor controller is restored to a known state, thus allowing you to only configure the settings that you intend to change.  Here's an explaination of some of the other configuration parameters.

- The voltage compensation saturation config `configVoltageCompSaturation()` is used to determine what voltage represents 100% output.  The motors will adjust their outputs in response to the battery voltage measurement.

- The `configOpenloopRamp()` prevents instantaneous changes in throttle.  See [Ramping](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configopenloopramp#ramping) for more information.

- A device’s neutral deadband is the region where the controller demotes its output to neutral. This is configured using the `configNeutralDeadband()` function. See [Neutral Deadband](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configneutraldeadband#neutral-deadband) in the Phoenix documentation.

- The `configSelectedFeedbackSensor()` parameter is used to select the encoder that's used with the motor.  The Talon FXs have intergrated encoders.  See [Talon FX/SRX Sensors](https://docs.ctre-phoenix.com/en/latest/ch14_MCSensor.html?highlight=configSelectedFeedbackSensor#bring-up-talon-fx-srx-sensors) in the Phoenix documentation.

You'll learn about some of the other configuration parameters during the lab.


    // Configure the motors
    for(TalonFX fx : new TalonFX[] {m_leftLeader, m_leftFollower, m_rightLeader, m_rightFollower}){

          //Reset settings for safety
          fx.configFactoryDefault();

          //Sets voltage compensation to 10, used for percent output
          fx.configVoltageCompSaturation(10);
          fx.enableVoltageCompensation(true);

          //Setting just in case
          fx.configNominalOutputForward(0);
          fx.configNominalOutputReverse(0);
          fx.configPeakOutputForward(1);
          fx.configPeakOutputReverse(-1);

          fx.configOpenloopRamp(0.1);

          //Setting deadband(area required to start moving the motor) to 1%
          fx.configNeutralDeadband(0.01);

          //Set to brake mode, will brake the motor when no power is sent
          fx.setNeutralMode(NeutralMode.Coast);

          /** 
           * Setting input side current limit (amps)
           * 45 continious, 80 peak, 30 millieseconds allowed at peak
           * 40 amp breaker can support above 40 amps for a little bit
           * Falcons have insane acceleration so allowing it to reach 80 for 0.03 seconds should be fine
           */
          fx.configSupplyCurrentLimit(new SupplyCurrentLimitConfiguration(true, 40, 55, 20));

          //Either using the integrated Falcon sensor or an external one, will change if needed
          fx.configSelectedFeedbackSensor(FeedbackDevice.IntegratedSensor); 
    }

    //Setting followers, followers don't automatically follow the Leader's inverts so you must set the invert type to FollowLeader
    m_leftFollower.follow(m_leftLeader, FollowerType.PercentOutput);
    m_leftFollower.setInverted(InvertType.FollowMaster);
    m_rightFollower.follow(m_rightLeader, FollowerType.PercentOutput);
    m_rightFollower.setInverted(InvertType.FollowMaster);

    m_rightLeader.setInverted(InvertType.InvertMotorOutput);

    // New Talon FX inverts
    // m_leftLeader.setInverted(TalonFXInvertType.CounterClockwise);
    // m_rightLeader.setInverted(TalonFXInvertType.Clockwise);


Each motor will play a different roll depending on its position in the drivetrain.  For a differential drive robot we want the rear motors to exactly follow the commands sent to the front motors. In addition, the motors on the left side of the drivetrain would need to be going in the opposite direction than those on the right.  The `follow()` and `setInverted(InvertType)` functions are used to implement this arrangement.  Note that `setInverted(InvertType.FollowMaster)` will instruct the motor controller to match the direction of the lead motor. Detailed information can be found in [Open-Loop Features](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=setInverted#open-loop-features) of the Phoenix documentation.

The Talon FX has a new set of inverts that are specific to it, see [Talon FX Specific Inverts](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=talonfxinverttype#talon-fx-specific-inverts).

![Motor Configuration](../../images/FRCroboRIO/FRCroboRIO.004.jpeg)

## Lab - Configure Motors
In this lab your task is to research some of the motor configuration parameters.  Go to the [Phoenix Documentation Website](https://docs.ctre-phoenix.com/en/latest/index.html).  Use the search field to find results for the `setNeutralMode()`, `configSupplyCurrentLimit()`, `configSelectedFeedbackSensor()` motor configuration parameters and read the information provided.  You may have to do a page search after clicking on the result to find the parameter.  After doing your research consider the following questions.

1. What are the two Netural Modes set by `setNeutralMode()` and what is the difference between them?  Switch the Netural Mode from **Coast** to **Brake** and observe the difference while driving the robot.

2. What condition does the `configSupplyCurrentLimit()` setting try to prevent?

3. What does the `configSelectedFeedbackSensor()` configuration do?  What are the default sensors for the TalonFX and TalonSRX?

Once you understand the configuration parameters you're done with this task!

## References

- CTRE - [Bring Up: Talon FX/SRX and Victor SPX](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html)

- CTRE - [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner)

- FRC Documentation [Motors APIs](https://docs.wpilib.org/en/stable/docs/software/hardware-apis/motors/index.html)

- CTRE - [Falcon 500 Motor User Guide](https://robotics.choate.edu/wp-content/uploads/2020/01/Falcon500UserGuide-20191101.pdf)

- CTRE - [Talon SRX - User’s Guide](https://store.ctr-electronics.com/content/user-manual/Talon%20SRX%20User's%20Guide.pdf)