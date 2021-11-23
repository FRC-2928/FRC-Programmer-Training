# Robot Characterization
*Characterization* or, more formally, *System Identification* - is the process of determining a mathematical model for the behavior of a system through statistical analysis of its inputs and outputs.

## Running the Robot Characterization Tool
To install the Robot Characterization Tool follow the [Installing and Launching](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-characterization/introduction.html#installing-and-launching-the-toolsuite) instructions.  Once you have it installed, open a powershell or terminal and type `frc-characterization drive new`

Load the characterization program [Romi Characterization](https://github.com/mjwhite8119/romi-examples/tree/main/romi-characterization).  This program was sourced from [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization), which includes the instructions for running it and gathering the data.

<!-- Start up the *romi-characterization* program and follow the [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization) instructions. -->

## Interpreting the Romi Analysis
To be completed.

## Robot Characterization Lab
Run robot characterization for your Romi.  Look at the results and add the following constants to your *Constants* file.  Note, that the values for your Romi may be different from the example below.

    // Dynamical constants
    public static final double kMaxSpeedMetersPerSecond = 0.5;
    public static final double kMaxAccelMetersPerSecondSquared = 0.5;

    // The linear velocity gain, volts per (meter per second)
    public static final double kvVoltSecondsPerMeter = 9.7;
    // The angular velocity gain, volts per (radians per second)
    public static final double kvVoltSecondsPerRadian = 0.345;

## References
- FRC Documentation - [Robot Charactization](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-characterization/index.html)

- FRC Documentation - [Feedforward Control in WPILib](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/feedforward.html#feedforward-control-in-wpilib)

- Code Example - [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization)