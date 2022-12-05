# Configuring Motors and Encoders
Our team primarily uses the Talon FX/SRX motors supplied supplied by *Cross The Road Electronics CTRE* for the competition robot. These motors have a lot of features that need to be configured. This can be done using the [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner) or from the your Java program code.  Although you can set them up in the Tuner during testing, it's highly recommended to ultimately set them via the program code. This way, in the event a device is replaced, you can rely on your software to properly configured the new device, without having to remember to use Tuner to apply the correct values.

The code segment below shows how the motors are configured for our drivetrain.  There are four motors to configure, which is done inside of a parameterized loop so as not to duplicate code. We first call the `configFactoryDefault()` routine to ensure motor controller is restored to a known state, thus allowing you to only config the settings that you intend to change.  Here's an explaination of some of the configuration parameters.

- The voltage compensation saturation config `configVoltageCompSaturation()` is used to determine what voltage represents 100% output.  The motors will adjust their outputs in response to the battery voltage measurement.

- The `configOpenloopRamp()` prevents instantaneous changes in throttle.  See [Ramping](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configopenloopramp#ramping) for more information.

- A device’s neutral deadband is the region where the controller demotes its output to neutral. This is configured using the `configNeutralDeadband()` function. See [Neutral Deadband](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=configneutraldeadband#neutral-deadband) in the Phoenix documentation.

You learn about the other configuration parameters during the lab.


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

    //Setting followers, followers don't automatically follow the Leader's inverts so you must set the invert type to FollotLeader
    m_leftFollower.follow(m_leftLeader, FollowerType.PercentOutput);
    m_leftFollower.setInverted(InvertType.FollowMaster);
    m_rightFollower.follow(m_rightLeader, FollowerType.PercentOutput);
    m_rightFollower.setInverted(InvertType.FollowMaster);

    m_rightLeader.setInverted(InvertType.InvertMotorOutput);


Each motor will play a different roll depending on its position in the drivetrain.  For a differential drive robot we want the rear motors to exactly follow the commands sent to the front motors. In addition, the motors on the left side of the drivetrain would need to be going in the opposite direction than those on the right.  The `follow()` and `setInverted(InvertType)` functions are used to implement this arrangement.  Note that the the `setInverted(InvertType)` will instruct the motor controller to either match or oppose the direction of the master. Detailed information can be found in [Open-Loop Features](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html?highlight=setInverted#open-loop-features) of the Phoenix documentation.

![Motor Configuration](../../images/FRCroboRIO/FRCroboRIO.004.jpeg)

## Lab - Configure Motors
In this lab your task is to research some of the motor configuration parameters.  Go to the [Phoenix Documentation Website](https://docs.ctre-phoenix.com/en/latest/index.html).  Use the search field to find results for the `setNeutralMode()`, `configSupplyCurrentLimit()`, `configSelectedFeedbackSensor()` motor configuration parameters and read the information provided.  You may have to do a page search after clicking on the result to find the parameter.  After doing your research answer the following questions.

1. Why would you use **Coast** for `setNeutralMode()` in the drivetrain rather than **Brake**?

## References

- CTRE - [Bring Up: Talon FX/SRX and Victor SPX](https://docs.ctre-phoenix.com/en/latest/ch13_MC.html)

- CTRE - [Phoenix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner)