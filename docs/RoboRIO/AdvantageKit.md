# Advantage Scope/Kit
AdvantageScope is a robot diagnostics, log review/analysis, and data visualization application for FIRST Robotics Competition teams. It reads logs in WPILOG, DS log, and RLOG file formats, plus live robot data viewing using NT4 or RLOG streaming. AdvantageScope can be used with any WPILib project, but is also optimized for use with our AdvantageKit logging framework. See [What is Advantage Kit](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/WHAT-IS-ADVANTAGEKIT.md) for an introduction of what it does.

## Installation
To install *Advantage Scope* go to the [releases page](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v3.0.0-beta-5) and download the package for you operation system.

To install the *Advantage Kit* libraries into your project go to [Advantage Kit Installation Page](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#new-projects).  Follow the instructions on that page for setting up a project. Commands need to be added to the `build.gradle` file, and to the *Robot* class. You should also add the *Gversion Plugin (Git Metadata)*, as documented on the installation page.

Install [Pheonix6](https://pro.docs.ctr-electronics.com/en/latest/docs/installation/installation.html) libraries in VSCode.  

## Advantage Kit

Diagram for implementation.

### Configure the Robot
After installing *AdvantageKit* you need to configure the robot code in order to use it.  See [Configuring the Robot](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#robot-configuration) in the *AdvantageKit* documentation.  There are two steps to this:

- Open the *Robot* class extend *LoggedRobot*.

- Add code into `robotInit()` in the *Robot* class.  Make sure that you import the AdvantageKit logger and not the system logger:

      import org.littletonrobotics.junction.Logger;

### Update RobotContainer    

    public RobotContainer() {
      if (isReal()) {
          // Instantiate IO implementations to talk to real hardware
          driveTrain = new DriveTrain(DriveTrainIOReal());
          elevator = new Elevator(ElevatorIOReal());
          intake = new Intake(IntakeIOReal());
      } else {
          // Use anonymous classes to create "dummy" IO implementations
          driveTrain = new DriveTrain(DriveTrainIO() {});
          elevator = new Elevator(ElevatorIO() {});
          intake = new Intake(IntakeIO() {});
      }
    }

### Subsystem Setup

Brings in the swerve module IO class. This gives us direct control to the hardware/simulated swerve module

    private final SwerveModuleIO io;

Brings in the swerve module data for positioning calculations.  The *SwerveModuleIOInputsAutoLogged* class gets auto generated.

    private final SwerveModuleIOInputsAutoLogged inputs = new SwerveModuleIOInputsAutoLogged();

Update input data from the IO layer, and then send the input data to the logging framework (or update from the log during replay).

    io.updateInputs(inputs);
    Logger.getInstance().processInputs("Drive/" + name + "Module", inputs);

Use the inputs in the subsystem.

    double currentAngle = inputs.steerPositionRad;

The logging framework supports recording this output data on the real robot and during replay. 

    Logger.getInstance().recordOutput("Drive/Angle", getPose().getRotation().getRadians());    

## Advantage Scope

## References