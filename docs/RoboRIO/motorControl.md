# Motor Control

## Closed Loop Modes
- Position - The Position Closed-Loop control mode can be used to abruptly servo to and maintain a target position.

- Current - The Current Closed-Loop logic can be used to approach a target current-draw. 

- Velocity - The Velocity Closed-Loop logic is used to maintain a target velocity. 

- Motion Magic - Motion Magic is a control mode for Talon SRX that provides the benefits of Motion Profiling without needing to generate motion profile trajectory points. When using Motion Magic, Talon SRX / Victor SPX will move to a set target position using a motion profile, while honoring the user specified acceleration, maximum velocity (cruise velocity), and optional S-Curve smoothing.

All closed-loop modes update every 1ms (1000Hz) unless configured otherwise.

See [Closed-Loop Control Modes](https://docs.ctre-phoenix.com/en/latest/ch16_ClosedLoop.html?highlight=demandtype#position-closed-loop-control-mode)

<!-- setOutputMetersPerSecond

ControlMode.PercentOutput -->

[Auxiliary Closed Loop PID](https://docs.ctre-phoenix.com/en/latest/ch16_ClosedLoop.html?highlight=auxpid#auxiliary-closed-loop-pid-1)

`setStatusFramePeriod()` [Setting Status Frame Periods](https://docs.ctre-phoenix.com/en/latest/ch18_CommonAPI.html?highlight=setStatusFramePeriod#setting-status-frame-periods)

## Arbitrary Feed Forward

The Arbitrary Feed Forward is a strategy for adding any arbitrary values to the motor output regardless of control mode. It can be used for gravity compensation, custom velocity and acceleration feed forwards, static offsets, and any other term desired.
[Arbitrary Feed Forward](https://docs.ctre-phoenix.com/en/latest/ch16_ClosedLoop.html?highlight=DemandType.ArbitraryFeedForward#arbitrary-feed-forward) 

`DemandType.ArbitraryFeedForward`

## Sensors
<!-- DriveStraight_AuxIntegratedSensor This example has the robot driving in Percent Output with an auxiliary closed loop on integrated sensor difference to keep the robot straight. -->

See [Talon FX/SRX Sensors](https://docs.ctre-phoenix.com/en/latest/ch14_MCSensor.html?highlight=configSelectedFeedbackSensor#bring-up-talon-fx-srx-sensors) in the Phoenix documentation.

##  Pheonix Tuner   

## References

- [Pheonix Examples](https://github.com/CrossTheRoadElec/Phoenix-Examples-Languages)    

- [Pheonix Documentation](https://docs.ctre-phoenix.com/en/stable/)

- [Pheonix Tuner](https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner)