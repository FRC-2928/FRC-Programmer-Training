# Motion Profiled PID Control 
With the *DriveDistancePID* command there's no way of avoiding the sudden changes in velocity at the start and end of the path.  This makes it difficult to tune the PID controller to precisely arrive at a setpoint.  It would be better if we can move more smoothly to the setpoint by gradually accelerating at the beginning and decelerating at the end. To do this we need to bring in the additional parameter of velocity. So instead of just specifying a distance we need to specify both a position and velocity.  This turns our path into a trajectory. Remember, a trajectory has both direction and speed.

This is where the *ProfiledPIDController* comes in.  This controller sets up a trajectory where the position and velocity is recomputed at multiple points along the path.  Although the direction may be a straight line, the velocity will change at various points throughout the trajectory.  The command accepts a *TrapezoidProfile* class to help implement the trajectory.  See [Trapezoid Motion Profile](../../Concepts/Dynamics/pathsTrajectories.md#trapezoidProfile) for more information.

The *ProfiledPIDController* works in the following manner. Instead of using a setpoint it uses a goal to determine when the robot has reached its destination.  The goal value is not the current setpoint of the loop - rather, it is the eventual setpoint once the generated profile terminates.

![PID Command](../../images/Romi/Romi.043.jpeg)

## Lab - Motion Control Profiled PID
This lab builds on the code that you wrote in the [Motion PID Control](romiPID.md) section of the training guide.  You'll learn about the following Java programming concepts:

There are two tasks for this lab:

- Create a profiled command called *TurnToAngleProfiled* to turn the robot to a specified angle.  This profiled command should make smooth turns as opposed to the *TurnToAnglePID* command where the turns are very abrupt.

- Use the *TurnToAngleProfiled* command to make the robot drive in a square.  This task will require the use of a Group Command.

## The DriveDistanceProfiled Command
The *ProfiledPIDController* class specifies **P, I**, and **D** values together with the *TrapezoidProfile.Constraints* that specify maximum velocity and acceleration that the robot is capable of.  For the Romi these constraints to 0.5 and 0.05 respectively, which were obtained during *Robot Characterization*.  The *TrapezoidProfile.Constraints* are a constant so can be put into the *Constants* file like so.

    public static final TrapezoidProfile.Constraints kTrapezoidProfileConstraints =
            new TrapezoidProfile.Constraints(DrivetrainConstants.kMaxSpeedMetersPerSecond,
                                            DrivetrainConstants.kMaxAccelMetersPerSecondSquared);

We'll again add the *Drivetrain* and *targetDistance* parameters to the constructor.  However, this time the *targetDistance* will be passed into the command as a *TrapezoidProfile.state* that specifies its final position and velocity.  The final velocity is usually set to zero and the end state is the new position to which we want to move.  The `initialize()` method of the command sets the starting position and velocity of the system to zero.

We'll use the encoders as the measurement source as we did in the last command. 

![PID Command](../../images/Romi/Romi.048.jpeg)

Here's the final code:

    public class DriveDistanceProfiled extends ProfiledPIDCommand {
      /** Creates a new DriveDistanceProfiled. */
      private static Drivetrain m_drive;
      private static NetworkTableInstance inst = NetworkTableInstance.getDefault();
      private static NetworkTable table = inst.getTable("Shuffleboard/Drivetrain");
      
      public DriveDistanceProfiled(double targetDistance, Drivetrain drivetrain) {
        super(
            // The ProfiledPIDController used by the command
            new ProfiledPIDController(
                // The PID gains and motion profile constraints
                DrivetrainConstants.kPDriveVel,
                DrivetrainConstants.kIDriveVel,
                DrivetrainConstants.kDDriveVel,

                // The motion profile constraints
                DrivetrainConstants.kTrapezoidProfileConstraints),

            // The measurement coming from the sensors
            drivetrain::getAverageDistanceMeters,

            // The goal (can also be a constant)
            () -> new TrapezoidProfile.State(targetDistance,0),

            // Use the calculated velocity at each setpoint
            (output, setpoint) -> {
              drivetrain.driveVelocity(setpoint.velocity);
            });

        // Use addRequirements() here to declare subsystem dependencies.
        addRequirements(drivetrain);    

        // Configure additional PID options by calling `getController` here.
        getController().setTolerance(DrivetrainConstants.kDistanceToleranceMeters,
                                    DrivetrainConstants.kVelocityToleranceMetersPerS);
      }

### Driving the Motors
One problem with this profiled motion command is that the robot more than likely doesn't drive straight.  Also, in the *DriveDistancePID* command we had to add a value to the **I** parameter in order to get the command to finish (reach its setpoint).  To fix these problems we'll create a new method in the *Drivetrain* class called `steerVelocity()` that calculates a voltage value for each wheel, which is then sent to the motors setVoltage() method.

### TurnToAngleProfiled Command
This is very similar to the *DriveDistanceProfiled* command in that it uses a *TrapezoidProfile* to make the turn.  Be aware of the maximum turn rate and acceleration used with TrapezoidProfile constraint, which must be specified in degrees.  For the Romi, values of 360 for turn rate and 250 for acceleration seem to work.

![PID Command](../../images/Romi/Romi.043.jpeg)

### AutonomousDriveSquare Command

## References

- FRC Documentation - [Motion Profiling through TrapezoidProfileSubsystems and TrapezoidProfileCommands](https://docs.wpilib.org/en/latest/docs/software/commandbased/profile-subsystems-commands.html)

- FRC Documentation - [Combining Motion Profiling and PID in Command-Based](https://docs.wpilib.org/en/latest/docs/software/commandbased/profilepid-subsystems-commands.html)