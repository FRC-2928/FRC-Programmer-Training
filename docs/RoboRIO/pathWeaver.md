# Trajectories and PathWeaver
When we did the [Paths & Trajectories](../../Romi/Control/romiTrajectory) lab on Romi the *Ramsete* command handled the trajectory-following, PID calculations, and feedforwards internally, with the final outputs scaled `-12` to `12` representing units of volts. The PID calculations and feedforward needed to be done internally since the motors on the Romi do not have any PID control. As a reminder, the diagram below shows how the command to run a trajectory is configured on the Romi.  Contrast this to the RoboRIO robot, which we'll look at next, where the PID controller is built into TalonFX. 

![Ramsete Command](../../images/Romi/Romi.055.jpeg)

On the RoboRIO robot the Ramsete controller performs no PID control and calculates no feedforwards. The final outputs are the raw wheel speeds in meters per second. The TalonFX supports a mode whereby the robot controller can stream a sequence of trajectory points to express a motion profile.  Because a lot of the work is done on the Talon's, our command to run trajectory-following will be a little simpler. 

![Ramsete Command Talons](../../images/Romi/Romi.065.jpeg)

The code used to construct the trajectory-following command looks like the following.  The command only outputs the meters per second for each wheel, it's the responsiblity of the TalonFX's *Velocity Closed-Loop* control logic to maintain the correct wheel speeds.

    public RunRamseteTrajectory(Drivetrain drivetrain, Trajectory trajectory) {
        super(
                trajectory,
                drivetrain::getPose,
                new RamseteController(AutoConstants.kRamseteB, AutoConstants.kRamseteZeta),
                DrivetrainConstants.kDriveKinematics,
                drivetrain::setOutputMetersPerSecond,
                drivetrain
            );
        m_drivetrain = drivetrain;
        m_trajectory = trajectory;
    }

## PathWeaver
WPILib contains a trajectory generation suite called [PathWeaver](https://docs.wpilib.org/en/latest/docs/software/pathplanning/pathweaver/introduction.html#introduction-to-pathweaver) that can be used to generate and follow trajectories.  

<!-- Each trajectory point holds the desired velocity, position, arbitrary feedforward, and time duration to honor said point until moving on to the next point. The point also holds targets for both the primary and auxiliary PID controller, allowing for differential control (drivetrain, differential mechanisms).

Alternatively, the trajectory points can be streamed into the motor controller as the motor controller is executing the profile, so long as the robot controller sends the trajectory points faster than the Talon consumes them. This also means that there is no practical limit to how long a profile can be. -->

## Lab - Trajectories and PathWeaver
In this lab we'll setup the code required to run a trajectory-following command using PathWeaver.  It will have three tasks:

- Run System Identification and add the parameters.

- Configure a drive subsystem to track the robot’s pose using WPILib’s odometry library.

- Generate a simple trajectory through a set of waypoints using WPILib’s TrajectoryGenerator class.

- Create a command called *RunRamseteTrajectory* to follow the generated trajectory.

### Run System Identification and Add Parameters
In a previous lab you ran [System Identification on the Romi](../../Romi/Control/romiSystemId).  We're going to run System Identification again but this time for the RoboRIO robot.  Start the SysId Tool from VSCode or the terminal command line. The configuration for the RoboRIO robot needs to be entered.  The following configuration would be typical for the robots that our team uses.  

![Configure SysID](../../images/FRCTools/FRCTools.037.jpeg)

Run the SysId tests and analyze the results.  Update the voltage values and speed constraints.  An example of these is shown below, but the values for your robot will be different.

        public static final double ksVolts = 0.6024;
        public static final double kvVoltSecondsPerMeter = 0.21907;
        public static final double kaVoltSecondsSquaredPerMeter = 0.0096252;

        public static final double kMaxSpeedMetersPerSecond = 2.0;
        public static final double kMaxAccelMetersPerSecondSquared = 2.0;

Before setting up the trajectory-following command we also need to add a new subclass to the *Constants* file. These two constants are used by the trajectory-following algorithm.

    public static final class AutoConstants {    
        public static final double kRamseteB = 2;
        public static final double kRamseteZeta = 0.7;
    }   
    
### Setup Robot Odometry

See [Kinematics and Odometry](../../Romi/SC/romiOdometry)

### Create a Trajectory

### Create the *RunRamseteTrajectory* Command
Create a new command called *RunRamseteTrajectory* from the left file menu.  This command will extend *RamseteCommand*, so replace *CommandBase* with *RamseteCommand*.  The command is going to need to know what the trajectory is and have access to the drivetrain, so pass in *Drivetrain* and *Trajectory* object.  Here's how the *RunRamseteTrajectory* constructor should look with these two parameters.

    public RunRamseteTrajectory(Drivetrain drivetrain, Trajectory trajectory) {

    }

Now we need to construct the command.  Place the following code into the contructor.  You'll need to do some imports and add the m_drivetrain and m_trajectory method variables just above the contructor.

    super(
      trajectory,
      drivetrain::getPose,
      new RamseteController(AutoConstants.kRamseteB, AutoConstants.kRamseteZeta),
      DrivetrainConstants.kDriveKinematics,
      drivetrain::setOutputMetersPerSecond,
      drivetrain
    );
    m_drivetrain = drivetrain;
    m_trajectory = trajectory;


Notice that it doesn't require the function `getWheelSpeeds()` as we did on the Romi, since the wheel speeds are obtained from the internal velocity loop of the TalonFX.

Setup the function required to load a trajectory.


        public Trajectory loadTrajectory(String trajectoryJSON) {
            Trajectory trajectory = new Trajectory();

            try{
                Path trajectoryPath = Filesystem.getDeployDirectory().toPath().resolve(Paths.get("output", trajectoryJSON + ".wpilib.json"));
                trajectory = TrajectoryUtil.fromPathweaverJson(trajectoryPath);
            } catch (IOException ex) {
                DriverStation.reportError("Unable to open Trajectory:" + trajectoryJSON, ex.getStackTrace());
            }
            return trajectory;
        }

Create a trajectory using PathWeaver.


### Testing the Trajectory-Following Command



## References
- FRC Documentation - [Trajectory Tutorial](https://docs.wpilib.org/en/latest/docs/software/pathplanning/trajectory-tutorial/trajectory-tutorial-overview.html)

- FRC Documentation - [Introduction to PathWeaver](https://docs.wpilib.org/en/latest/docs/software/pathplanning/pathweaver/introduction.html#introduction-to-pathweaver)

- Pheonix docuementation - [Motion Profile Control Mode](https://docs.ctre-phoenix.com/en/stable/ch16_ClosedLoop.html#motion-profile-control-mode)

