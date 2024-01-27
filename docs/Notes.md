# Notes on Creating New Competition Robot

## Create a Basic Drivetrain
- Create a README file.
- Paste in *Constants* file and remove ColorSensor code.  Comment maybe code.
- Create Pigion.
- Create Transmission.  Rename from TransmissionSubsystem.
- Copy *JettDriverOI* and *AbbyOperatorOI* and create *DriverIO* and *OperatorIO*. Remove interface and overrides. 
- Copy *DrivetrainSubsystem* and rename to *Drivetrain*.
- Change all occurrences of TransmissionSubsystem
- *SpeedControllerGroup* has been renamed to *MotorControllerGroup*.  This can be removed since we won't use them for the Talons.
- Copy and paste RobotContainer and remove non-existant commands and subsystems.
- Comment out the *AutoType enum* and the `m_autoChooser` variable and replace it with:
 
        private final SendableChooser<Command> m_chooser = new SendableChooser<>();

- Comment out the switch statement in *getAutonomousCommand()* and replace the return with:

        public Command getAutonomousCommand() {
            return m_chooser.getSelected();
        }
- Delete the example command and subsystem.

## Create a Basic Trajectory
- Explain how we're going to change the SendableChooser statement to return a command instead of AutoType enum.
- Create *SimpleMotorFeedforward* under the DrivetrainConstants:

        public static final SimpleMotorFeedforward kFeedForward = 
                    new SimpleMotorFeedforward(ksVolts, 
                                                kvVoltSecondsPerMeter, 
                                                kaVoltSecondsSquaredPerMeter);


## CoppeliaSim
Scene directory: 
        /Applications/coppeliaSim.app/Contents/Resources/scenes/
Must have Python 3.11 on m1 Mac to install robotpy libraries
        python3 -m pip install pyntcore
        python3 -m pip install pynetworktables

[Installing ZeroMQ](https://www.coppeliarobotics.com/helpFiles/en/zmqRemoteApiOverview.htm)    

Set PYTHONPATH in .bash_profile
        PYTHONPATH="/Applications/coppeliaSim.app/Contents/Resources/programming/zmqRemoteApi/clients/python/"
        export PYTHONPATH

## AdvantageKit Changes for SwerveDrive
Branch the *Hotpants* code and call it *AdvantageKit*.

Clone the [AdvantageKit](https://github.com/Mechanical-Advantage/AdvantageKit/tree/main) example projects so as we can do copy and paste.

Rename the *Drive* command to *JoystickDrive*.

Add the IO classes from the AdvantageKit simple swerve example.  These go in `subsystems` *ModuleIO*, *ModuleIOFalconFX*, *GyroIO*, *GyroIOPigeon2*.

### Constants File Changes
Add new constants.

    public static final Mode currentMode = Mode.REAL;
	public static enum Mode {
		/** Running on a real robot. */
		REAL,

		/** Running a physics simulator. */
		SIM,

		/** Replaying from a log file. */
		REPLAY
	}

### Robot Changes

Add the following to `robotInit()` just before the `Logger.start()` command.

    // Set up data receivers & replay source
    switch (Constants.currentMode) {
      case REAL:
        // Running on a real robot, log to a USB stick
        Logger.addDataReceiver(new WPILOGWriter("/U"));
        Logger.addDataReceiver(new NT4Publisher());
        break;

      case SIM:
        // Running a physics simulator, log to NT
        Logger.addDataReceiver(new NT4Publisher());
        break;

      case REPLAY:
        // Replaying a log, set up replay source
        setUseTiming(false); // Run as fast as possible
        String logPath = LogFileUtil.findReplayLog();
        Logger.setReplaySource(new WPILOGReader(logPath));
        Logger.addDataReceiver(new WPILOGWriter(LogFileUtil.addPathSuffix(logPath, "_sim")));
        break;
    }
### GyroIOPigeon2 Changes
Change the configuration.

    private final Pigeon2 pigeon = new Pigeon2(Constants.CAN.pigeon, "canivore");

Add the function to *GyroIO* and here in *GyroIOPigeon2*.

    @Override
    public void resetGyro() {
        pigeon.reset();
    }

Add a new input.

    inputs.rotations = pigeon.getRotation2d().unaryMinus().getRotations();

### ModuleIOFalconFX Changes
Configure devices in *ModuleIOFalconFX*, e.g.

    driveTalon = new TalonFX(Constants.CAN.swerveFrontLeftAzimuth, "canivore");
    turnTalon = new TalonFX(Constants.CAN.swerveFrontLeftDrive, "canivore");
    cancoder = new CANcoder(Constants.CAN.swerveFrontLeftEncoder, "canivore");
    absoluteEncoderOffset = Constants.CAN.swerveFrontLeftOffset;

Change `absoluteEncoderOffset` to a `double` and use it to configure the encoder instead of tracking the offset.

    CANcoderConfiguration encoderConfig = new CANcoderConfiguration();
    encoderConfig.MagnetSensor.MagnetOffset = absoluteEncoderOffset;
    cancoder.getConfigurator().apply(encoderConfig);

Add a new variable to get `drivePositionRotations`.

    private final StatusSignal<Double> driveRotorPosition;

    inputs.drivePositionRotations = driveRotorPosition.getValueAsDouble();

### SwerveModule Changes
![AdvantageKit IOLayer](../images/AdvantageKit/AdvantageKit.005.jpeg)

In the *SwerveModule* the `azimuth`, `drive`, and `encoder` variables will get removed and replaced with `io`.

    private final ModuleIO io;
    private final ModuleIOInputsAutoLogged inputs = new ModuleIOInputsAutoLogged();

All device configuration gets done in *ModuleIOFalconFX* so the constructor should now look like this:

    public SwerveModule(final ModuleIO io,final Place place) 
    {
        this.io = io;
        this.place = place;
        this.pid.enableContinuousInput(-180, 180);
        setBrakeMode(true);
    }

Create the `setBrakeMode()` function:

    /** Sets whether brake mode is enabled. */
    public void setBrakeMode(boolean enabled) {
        io.setDriveBrakeMode(enabled);
        io.setTurnBrakeMode(enabled);
    }

Rename the `pos()` function to `updateModulePosition()` since that's what it does.  Update the `updateModulePosition()` function to get the inputs from the IO Layer:

Change `this.drive.getRotorPosition().getValue()` to `this.inputs.drivePositionRotations`, which is represented by `driveTalon.getRotorPosition().getValueAsDouble()`.

Change `this.encoder.getAbsolutePosition().getValue()` to `this.inputs.turnAbsolutePosition`, which is represented by `cancoder.getAbsolutePosition().getValueAsDouble()`

    public SwerveModulePosition updateModulePosition() {
        return new SwerveModulePosition(
            Constants.Drivetrain.driveGearMotorToWheel
                .forward(
                    // Constants.Drivetrain.motorEncoderToRotations.forward(this.drive.getRotorPosition().getValue())
                    Constants.Drivetrain.motorEncoderToRotations.forward(this.inputs.drivePositionRotations)
                ),
            // Rotation2d.fromRotations(this.encoder.getAbsolutePosition().getValue())
            this.inputs.turnAbsolutePosition
        );
    }

Changes to the `update()` function.  Update the devices inputs from the IO Layer:

    io.updateInputs(inputs);
    Logger.processInputs("Drive/Module" + Integer.toString(this.place.index), inputs);

Set the power from the IO Layer:

    // this.azimuth.set(Constants.Drivetrain.azimuthGearMotorToWheel.forward(MathUtil.clamp(-turn, -90, 90)));
    io.setTurnVoltage(Constants.Drivetrain.azimuthGearMotorToWheel.forward(MathUtil.clamp(-turn, -90, 90)));
        
    // this.drive.set(this.backwards ? -this.targetVelocity : this.targetVelocity);
    io.setDriveVoltage(this.backwards ? -this.targetVelocity : this.targetVelocity);


Replace the `halt()` function with:

    public void stop() { 
        io.setTurnVoltage(0.0);
        io.setDriveVoltage(0.0);
    }

### Drivetrain Changes
Rename `est()` to `poseEstimator()`.

Add member variables and remove the motors and gyro.

    private final GyroIO gyroIO;
    private final GyroIOInputsAutoLogged gyroInputs = new GyroIOInputsAutoLogged();
    private final SwerveModule[] modules = new SwerveModule[4]; // FL, FR, BL, BR

Change the constructor

    public Drivetrain(
        GyroIO gyroIO,
        ModuleIO flModuleIO,
        ModuleIO frModuleIO,
        ModuleIO blModuleIO,
        ModuleIO brModuleIO) {
        this.gyroIO = gyroIO;
        modules[0] = new SwerveModule(flModuleIO, Place.FrontLeft);
        modules[1] = new SwerveModule(frModuleIO, Place.FrontRight);
        modules[2] = new SwerveModule(blModuleIO, Place.BackLeft);
        modules[3] = new SwerveModule(brModuleIO, Place.BackRight);
	}

Add the functions.

    public void resetGyro() {
        gyroIO.resetGyro();
    }

    public double getGyroRotations() {
        return gyroInputs.rotations;
    }

Update the `periodic()` function.  At the beginning put.

    gyroIO.updateInputs(gyroInputs);
    Logger.processInputs("Drive/Gyro", gyroInputs);

After doing the serve update.

    // Stop moving when disabled
    if (DriverStation.isDisabled()) {
      for (var module : modules) {
        module.stop();
      }
    }

    // Log empty setpoint states when disabled
    if (DriverStation.isDisabled()) {
      Logger.recordOutput("SwerveStates/Setpoints", new SwerveModuleState[] {});
      Logger.recordOutput("SwerveStates/SetpointsOptimized", new SwerveModuleState[] {});
    }

Finally.

    this.poseEstimator.update(this.gyroInputs.yawPosition, this.getModulePositions());


### RobotContainer Changes
Add the following to *RobotContainer*.

    switch (Constants.currentMode) {
		case REAL:
			// Real robot, instantiate hardware IO implementations		
			drivetrain = new Drivetrain(
				new GyroIOPigeon2(),
				new ModuleIOTalonFX(0),
				new ModuleIOTalonFX(1),
				new ModuleIOTalonFX(2),
				new ModuleIOTalonFX(3));
			break;

		case SIM:
			// Sim robot, instantiate physics sim IO implementations
			drivetrain =
				new Drivetrain(
					new GyroIO() {},
					new ModuleIOSim(),
					new ModuleIOSim(),
					new ModuleIOSim(),
					new ModuleIOSim());
			break;

		default:
			// Replayed robot, disable IO implementations
			drivetrain =
				new Drivetrain(
					new GyroIO() {},
					new ModuleIO() {},
					new ModuleIO() {},
					new ModuleIO() {},
					new ModuleIO() {});
			break;
		}

Change.

    this.driverOI.resetFOD.whileTrue(new RunCommand(() -> this.drivetrain.resetGyro()));


## Additional Changes

### SwerveModule

    public double getDistanceMeters() {
        return 
            Constants.Drivetrain.driveGearMotorToWheel
                .forward(
                    // Constants.Drivetrain.motorEncoderToRotations.forward(this.drive.getRotorPosition().getValue())
                    Constants.Drivetrain.motorEncoderToRotations.forward(this.inputs.driveRotorPosition)
                );
    }

    /**
     * Position of the motor rotor in rotations. This position is only affected by the
     * RotorOffset config. Function driveTalon.getRotorPosition();
     * 
     * @return
     */
    public double getDriveRotorPosition() {
        return this.inputs.driveRotorPosition;
    }

    /**
     * Absolute Position of the device in rotations.
     * 
     * @return Rotation2d.fromRotations(cancoder.getAbsolutePosition().getValueAsDouble());
     */
    public Rotation2d getAngle() {
        return  this.inputs.turnAbsolutePosition;
    }

    /**
     * Position of the device in mechanism radians.  This is
     * multiplied by the gear ratio to get the number of wheel radians.
     * Finally, use the wheel radius to get the distance in meters.
     * 
     * @return current drive position of the module in meters. 
     */
    public double getPositionMeters() {
        return inputs.drivePositionRad * Constants.Drivetrain.wheelRadius;
    }

    /** Returns the current turn angle of the module. 
     * 
     * @return Rotation2d.fromRotations(turnTalon.getPosition().getValueAsDouble() / TURN_GEAR_RATIO)
    */
    public Rotation2d getOdometryAngle() {      
        return inputs.turnPosition;
    }

    /** Returns the module position delta since the last call to this method. */
    public SwerveModulePosition getPositionDelta() {
        var delta = new SwerveModulePosition(getPositionMeters() - lastPositionMeters, getAngle());
        lastPositionMeters = getPositionMeters();
        return delta;
    }

### Drivetrain

      /**
    * 
    * @return pigeon2.getRotation2d().unaryMinus().getRotations();
    */
        public double getGyroRotations() {
            return gyroInputs.rotations;
        }

    /** Returns the current odometry pose. */
    @AutoLogOutput(key = "Odometry/Robot")
    public Pose2d getPose() {
        return pose;
    }

    /** Returns the current odometry pose. */
    @AutoLogOutput(key = "OdometryEstimation/Robot")
    public Pose2d getPoseEstimation() {
        return this.poseEstimator.getEstimatedPosition();
    }

    public void updateOdometry() {
        // Update odometry
        SwerveModulePosition[] wheelDeltas = new SwerveModulePosition[4];
        for (int i = 0; i < 4; i++) {
        wheelDeltas[i] = modules[i].getPositionDelta();
        }
        // The twist represents the motion of the robot since the last
        // loop cycle in x, y, and theta based only on the modules,
        // without the gyro. The gyro is always disconnected in simulation.
        var twist = kinematics.toTwist2d(wheelDeltas);
        if (gyroInputs.connected) {
        // If the gyro is connected, replace the theta component of the twist
        // with the change in angle since the last loop cycle.
        twist =
            new Twist2d(
                twist.dx, twist.dy, gyroInputs.yawPosition.minus(lastGyroRotation).getRadians());
        lastGyroRotation = gyroInputs.yawPosition;
        }
        SmartDashboard.putNumber("Twist", twist.dx);
        // Apply the twist (change since last loop cycle) to the current pose
        pose = pose.exp(twist);
    }
- 