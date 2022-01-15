# Robot Characterization
*Characterization* or, more formally, *System Identification* - is the process of determining a mathematical model for the behavior of a system through statistical analysis of its inputs and outputs.

## Running the System Identification Tool
The System Identification Tool is installed with WPILib read [Introduction to Robot Characterization](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/introduction.html#introduction-to-robot-characterization) for an overview of what the tool does.  The tool only supports Windows. The system identification tool can be opened from the **Start Tool** option in VS Code or by using the shortcut inside the WPILib Tools desktop folder (Windows).  Starting the tool creates a directory under your project called `.SysId`.

#### Configuring the Project
The first step is to [Configuring the Project](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/configuring-project.html#configuring-a-project). 

#### Deploying the Project

[Deploying the Project](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/configuring-project.html#deploying-project).

<!-- Load the characterization program [Romi Characterization](https://github.com/mjwhite8119/romi-examples/tree/main/romi-characterization).  This program was sourced from [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization), which includes the instructions for running it and gathering the data. -->

#### Analysis the Data
[Analysing Data](https://docs.wpilib.org/en/stable/docs/software/pathplanning/robot-characterization/analyzing-data.html)

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
- FRC Documentation - [Robot Charactization](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-characterization/index.html)

- FRC Documentation - [Feedforward Control in WPILib](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/feedforward.html#feedforward-control-in-wpilib)

- Code Example - [Romi Characterization](https://github.com/bb-frc-workshops/romi-examples/tree/main/romi-characterization)