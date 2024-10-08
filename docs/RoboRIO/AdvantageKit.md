# Advantage Kit
AdvantageKit is a logging framework that records all of the data flowing into the robot code. After a match, these values can be replayed to the robot code in a simulator. The log needs to be replayed using the same version of code that generated the log. See [What is Advantage Kit](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/WHAT-IS-ADVANTAGEKIT.md) for more information.

## Installation
To install the *Advantage Kit* libraries into your project go to [Advantage Kit Installation Page](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#new-projects).  Follow the instructions on that page for setting up a project. Commands need to be added to the `build.gradle` file, and to the *Robot* class. You should also add the *Gversion Plugin (Git Metadata)*, as documented on the installation page. This ensures that you can match the logfiles with the correct version of the code that generated them. 

## <a name="AKModes"></a>AdvantageKit Modes
*AdvantageKit* runs in two modes, **Record** or **Playback**. 

Record mode happens by running the real robot or by running a *WPI Robot Simulator*.  While running on the real robot it's set to write out to log files on the RoboRio and can also publish to the Network Tables.  It's recommended that you use a thumb drive on the RoboRio to hold the log files. When running the WPI Simulator the log files are written to the project directory under the top level `logs` folder. Data can also be written to the Network Tables and viewed live in *AdvantageScope*.

Once you have a log file you can simply play it back in *AdvantageScope*.  If you want to make changes, such as collecting more data, then you can use *AdvantageKit*'s playback mode to create a new logfile containing the additional data.

In playback mode it reads the log file previously written during a match or in simulation and plays it back to the robot code. You start the playback by running the **Simulate Robot Code** option in VSCode. When the playback runs it can create a new logfile that includes any new data that was collected.  Note that the code will run as fast as possible in the WPI Simulator and then exit, so the review must be done from *AdvantageScope*.

![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.002.jpeg)

Here's an example of how these modes can be enabled.  This code goes in the `initRobot()` function of the *Robot* class.  Log generation takes place when running the real robot or in simulation.  Once the data is collected it can be played back using the same user program that generated it.

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
After installing *AdvantageKit* you need to configure the robot code in order to use the framework.  See [Configuring the Robot](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/INSTALLATION.md#robot-configuration) in the *AdvantageKit* documentation.  There are two steps to this:

- Open the *Robot* class and extend *LoggedRobot*.

- Add code into `robotInit()` in the *Robot* class.  Make sure that you import the *AdvantageKit* logger NOT the system logger.

### Setting up the IO Layers
By necessity, any interaction with external hardware must be isolated such that all input data is logged and can be replayed in *AdvantageScope* where that hardware is not present. It's recommended to restructure the subsystem such that hardware interfacing occurs in a separate object called the *IO Layer*. The IO layer includes an interface defining all methods used for interacting with the hardware along with one or more implementations that make use of vendor libraries to carry out commands and read data.

![Extracting IO Layers](../images/AdvantageKit/AdvantageKit.004.jpeg)

The following section shows how to set up an IO Layer for the gyro and implement it in the *Drivetrain* subsystem.

### Setting up the GyroIO Interface
Each hardware system should have an interface class that defines the input/output data that should be logged, and methods used to control the hardware. Specific hardware classes can then be created that implement the IO interface.  The example diagram the real Pigeon2 hardware and a simulation of it.

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
Since any interaction with the hardware now goes through the IO Layer, any control inputs must be defined in the interface class and implemented in the hardware specific class.  To define in the interface class:

    public default void resetGyro() {}
    
And to implement in the hardware specific *GyroIOPigeon2* class:

    @Override
    public void resetGyro() {
        this.pigeon.reset();
    }

See [Recording Inputs](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/RECORDING-INPUTS.md) in the AdvantageKit documentation for more information.

### Subsystem Setup
With the IO layers created they can now be used by the subsystem.  The example Gyro IO Layer can be used by the Drivetrain class.  We would also define an IO layer for the drive train motors so the example below shows how they would both be used.


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

In the `periodic()` routine the input data gets updated from the IO layer, and then get sent to the logging framework (or updates from the log during replay).

    periodic() {
        io.updateInputs(inputs);
        Logger.getInstance().processInputs("Drive/" + name + "Module", inputs);
    }    

In the subsystem methods the data from the IO layer is used.

    public Rotation2d getHeading() {
        return gyroInputs.gyroYaw;
    }

The logging framework supports recording this output data on the real robot and during replay. 

    Logger.getInstance().recordOutput("Drive/Angle", getHeading());   

See [Recording Outputs](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/RECORDING-OUTPUTS.md) in the AdvantageKit documentation for more information.

### Passing the IO Layer to the Subsystem
Here's an example of how the IO Layer would get passed to the subsystem during its creation.  This code would go into the constructor of the *RobotContainer* class.  On the real robot we would use the actual hardware, otherwise the simulation IO layer would be used.  In playback mode we want to disable the IO layer since the data is coming from the previously generated log files.

    if (Robot.isReal()) {
      // Instantiate IO implementations to talk to real hardware
      m_drivetrain = new Drivetrain(new GyroIOPideon2(),
                                    new DrivetrainIOFalcon500(), 
                                    m_transmission::getGearState);
    } else {
      
      switch (Constants.currentMode) {     
        case SIM:
          // Use anonymous classes to create "dummy" IO implementations
          DrivetrainIOSim drivetrainSim = new DrivetrainIOSim();
          m_drivetrain = new Drivetrain(new GyroIOSim(drivetrainSim),
                                        drivetrainSim, 
                                        m_transmission::getGearState);
          break;

        default:
          // Replayed robot, disable IO implementations
          m_drivetrain = new Drivetrain(new GyroIOSim(null) {},
                                        new DrivetrainIOSim() {}, 
                                        null);
          break;
      }
    }

See the [Differential Drive example project](https://github.com/Mechanical-Advantage/AdvantageKit/tree/main/example_projects/diff_drive/src/main) for a full implementation.  For an example of a swerve drive implementation see the [Swerve Drive example](https://github.com/Mechanical-Advantage/AdvantageKit/tree/main/example_projects/swerve_drive/src/main).

## Advantage Scope
AdvantageScope is a robot diagnostics, log review/analysis, and data visualization application for FIRST Robotics Competition teams. It reads logs in WPILOG, DS log, and RLOG file formats, plus live robot data viewing using NT4 or RLOG streaming. AdvantageScope can be used with any WPILib project, but is also optimized for use with our AdvantageKit logging framework. Here's the online documentation of [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope/blob/main/docs/INDEX.md).

![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.003.jpeg)

To install *Advantage Scope* go to the [releases page](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v3.0.0-beta-5) and download the package for you operation system.  

Standard WPILib data logging can be used with *AdvantageScope*.  This provides a lot of data and may be sufficient without using *AdvantageKit*.  See [On-Robot Telemetry Recording Into Data Logs](https://docs.wpilib.org/en/stable/docs/software/telemetry/datalog.html) in the FRC documentation.

### Creating and Reading Log Files
By default, *DataLogManager* stores to a USB stick if one is present at startup, or `/home/lvuser` otherwise, but you can alternatively specify the directory when calling `start()`.

To log to a USB stick (`/U/logs`) by default

	Logger.addDataReceiver(new WPILOGWriter());

You can choose where you want the log files to be stored, `/media/sda1` refers specifically to a USB drive attached to the Rio (`media/sda2` refers to the other USB port).  Also, I see that I can reference `/U/` and `/V/` (presumably for the two USB ports).


### Running Live Simulator Streams
If you run your robot code in simulator mode using **Simulate Robot Code** from VSCode, then you can connect it to a running simulator in *AdvantageScope* via **File->Connect to Simulator**.  In the example below the robot pose has been dropped into the *Odometry* tab.  As you move the robot in the WPI Simulator it should also move in the AdvantageScope view.

![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.007.jpeg)


## References

- [AdvantageKit](https://github.com/Mechanical-Advantage/AdvantageKit/blob/main/docs/WHAT-IS-ADVANTAGEKIT.md) documentation.

- [AdvantageKit](https://github.com/Mechanical-Advantage/AdvantageKit/tree/main/example_projects) example projects

- [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope/blob/main/docs/INDEX.md) documentation

- [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope/releases) releases