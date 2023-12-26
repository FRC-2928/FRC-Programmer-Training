# Advantage Kit
AdvantageKit is a logging framework that records all of the data flowing into the robot code. After a match, these values can be replayed to the robot code in a simulator. The log needs to be replayed using the same version of code that generated this log. See [What is Advantage Kit](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/WHAT-IS-ADVANTAGEKIT.md) for more information.

## Installation
To install the *Advantage Kit* libraries into your project go to [Advantage Kit Installation Page](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#new-projects).  Follow the instructions on that page for setting up a project. Commands need to be added to the `build.gradle` file, and to the *Robot* class. You should also add the *Gversion Plugin (Git Metadata)*, as documented on the installation page.

The following `build.gradle` code may not be needed and will cause build errors:

    task(checkAkitInstall, dependsOn: "classes", type: JavaExec) {
        mainClass = "org.littletonrobotics.junction.CheckInstall"
        classpath = sourceSets.main.runtimeClasspath
    }
    compileJava.finalizedBy checkAkitInstall

## AdvantageKit Modes
*AdvantageKit* runs in two modes, **Record** or **Playback**. 

Record mode happens by running the real robot or by running a WPI Simulation.  While running on the real robot it's set to write out to log files on the RoboRio and can also publish to the Network Tables.  It's recommended that you use a thumb drive on the RoboRio to hold the log files. You can also write log files from the WPI Simulator.  In this case the log files are written to the project directory under the top level `logs` folder.

Once you have a log file you can simply play it back in *AdvantageScope*.  If you want to make changes, such as collecting more data, then you can use *AdvantageKit*'s playback mode to create a new logfile containing the additional data.

In playback mode it reads the log file previously written during a match or simulation and plays it back to the robot code. You start the playback by running the **Simulate Robot Code** option in VSCode. When the playback runs it creates a new logfile that can be viewed in *AdvantageScope*.  Note that the code will run as fast as possible in the WPI Simulator and then exit, so the review must be done from *AdvantageScope*.

![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.002.jpeg)

Here's how these modes are enabled in the `initRobot()` function.

    Logger.recordMetadata("ProjectName", "MyProject"); // Set a metadata value

    // Set up data receivers & replay source
    switch (Constants.currentMode) {
      case REAL:
        // Running on a real robot, log to a USB stick ("/U/logs")
        Logger.addDataReceiver(new WPILOGWriter());
        Logger.addDataReceiver(new NT4Publisher());
        break;

      case SIM:
        // Running a physics simulator, log to NT
        Logger.addDataReceiver(new NT4Publisher());
        Logger.addDataReceiver(new WPILOGWriter());
        break;

      case REPLAY:
        // Replaying a log, set up replay source
        setUseTiming(false); // Run as fast as possible
        String logPath = LogFileUtil.findReplayLog();
        Logger.setReplaySource(new WPILOGReader(logPath));
        Logger.addDataReceiver(new WPILOGWriter(LogFileUtil.addPathSuffix(logPath, "_sim")));
        break;
    }

    Logger.start();

### Configure the Robot
After installing *AdvantageKit* you need to configure the robot code in order to use it.  See [Configuring the Robot](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#robot-configuration) in the *AdvantageKit* documentation.  There are two steps to this:

- Open the *Robot* class extend *LoggedRobot*.

- Add code into `robotInit()` in the *Robot* class.  Make sure that you import the AdvantageKit logger and not the system logger:

      import org.littletonrobotics.junction.Logger;

### Setting up the IO Layers
By necessity, any interaction with external hardware must be isolated such that all input data is logged and can be replayed in the simulator where that hardware is not present. We need to restructure the subsystem such that hardware interfacing occurs in a separate object called the "IO" layer. The IO layer includes an interface defining all methods used for interacting with the hardware along with one or more implementations that make use of vendor libraries to carry out commands and read data.

The following example sets up an IO Layer for the gyro.

### Setting up the GyroIO Interface


![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.001.jpeg)

Create an `interface` class that can use to implement different types of hardware. In our case we'll implement the *Pideon2* gyro, but we can also implement simulation class.

Inside of this interface we define a class to to get the inputs from the hardware. This class implements *LoggableInputs*, which is a set of values which can be logged and replayed (for example, the hardware inputs for a subsystem). The data is stored in *LogTable* objects.  We'll only get the gyro yaw data in this example.

Finally, we update the set of loggable inputs in the `updateInputs()` method.

    public interface GyroIO { }

        public static class GyroIOInputs implements LoggableInputs {

            public double gyroYawPosition;

            public void toLog(LogTable table) {
                table.put("GyroYawPosition", gyroYawPosition);
            }

            public void fromLog(LogTable table) {
                gyroYawPosition = table.getDouble("GyroYawPosition", gyroYawPosition);
            }

        } 

        public default void updateInputs(GyroIOInputs inputs) {}

    }

### Implementing the GyroIO Interface
We're going to use the *Pigeon2* hardware so create a class called *GyroIOPigeon2*.  We can name the hardware that's being implemented.  This is where the actual hardware is accessed so we define the *Pigeon2* with its port ID.  

    public class GyroIOPigeon2 implements GyroIO {

        public Pigeon2 pigeon = new Pigeon2(Constants.CANBusIDs.kPigeonIMU);
        
        public GyroIOPideon2() {            
        }

        // Updates the set of loggable inputs. 
        public void updateInputs(GyroIOInputs inputs) {
            inputs.gyroYawPosition = this.pigeon.getAccumGyroZ().getValue();
        }
    }

### Implementing Control Inputs
Any control inputs must be defined in the interface class.

    public default void resetGyro() {}
    
In the GyroIOPigeon2 class implement the control input.

    @Override
    public void resetGyro() {
        this.pigeon.reset();
    }

See [Recording Inputs](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/RECORDING-INPUTS.md) in the AdvantageKit documentation.

### Subsystem Setup

Brings in the DrivetrainIO class. This gives us direct control to the hardware/simulated swerve module

    private final DrivetrainIO io;
    private final DrivetrainIOInputs inputs = new DrivetrainIOInputs();
    private final GyroIO gyroIO; 
    private final GyroIOInputs gyroInputs = new GyroIOInputs();

The hardware interface gets passed into the *Drivetrain*'s constructor.

    public Drivetrain(GyroIO gyroIO,
                    DrivetrainIO drivetrainIO) {
        this.io = drivetrainIO;
        this.gyroIO = gyroIO;
    }

Update input data from the IO layer, and then send the input data to the logging framework (or update from the log during replay).

    periodic() {
        io.updateInputs(inputs);
        Logger.getInstance().processInputs("Drive/" + name + "Module", inputs);
    }    

Use the inputs in the subsystem.

    double currentAngle = inputs.steerPositionRad;

The logging framework supports recording this output data on the real robot and during replay. 

    Logger.getInstance().recordOutput("Drive/Angle", getPose().getRotation().getRadians());    

### Update RobotContainer    

    public RobotContainer() {
      if (isReal()) {
          // Instantiate IO implementations to talk to real hardware
          driveTrain = new DriveTrain(new GyroIOPideon2(),
                                  new DrivetrainIOFalcon500(), 
                                  m_transmission::getGearState);
      } else {
          // Use anonymous classes to create "dummy" IO implementations
          driveTrain = new DriveTrain(new GyroIOPideon2(),
                                  new DrivetrainIOFalcon500(), 
                                  m_transmission::getGearState);
      }
    }

## Advantage Scope
AdvantageScope is a robot diagnostics, log review/analysis, and data visualization application for FIRST Robotics Competition teams. It reads logs in WPILOG, DS log, and RLOG file formats, plus live robot data viewing using NT4 or RLOG streaming. AdvantageScope can be used with any WPILib project, but is also optimized for use with our AdvantageKit logging framework. Here's the online documentation of [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope/blob/main/docs/INDEX.md).

![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.003.jpeg)

To install *Advantage Scope* go to the [releases page](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v3.0.0-beta-5) and download the package for you operation system.  

Standard WPILib data logging can be used with *AdvantageScope*.  This provides a lot of data and may be sufficient without using *AdvantageKit*.  See [On-Robot Telemetry Recording Into Data Logs](https://docs.wpilib.org/en/stable/docs/software/telemetry/datalog.html) in the FRC documentation.

### Running Live Simulator Streams
You can connect it to a running Simulator via *File->Connect to Simulator*

## References

- [Advantage Kit](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/WHAT-IS-ADVANTAGEKIT.md)