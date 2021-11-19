# Command Groups
Simple commands can be composed into [Command Groups](https://docs.wpilib.org/en/latest/docs/software/commandbased/command-groups.html) to accomplish more-complicated tasks. There are several ways in which Command Groups can be composed, as shown the documentation.  We'll look at a full example of a **Sequential** Command Group from the Romi sample code.

## The AutonomousDistance Command
The **AutonomousDistance** command is used to drive forward, turn 180 degrees, drive back, and turn another 180 degrees, hopefully ending up exactly where we started.  That's four commands executed one after another and is a prime candidate for a **SequentialCommandGroup**.
We'll be using the **Drivetrain** subsystem in this command so that needs to be imported together with the SequentialCommandGroup library.

The four commands are composed in the class constructor using the `addCommands()` method.  The four command are specified using just two procedures since these procedures were parameterized.  The commands are listed in the order in which we would like them to run.

    package frc.robot.commands;
    import frc.robot.subsystems.Drivetrain;
    import edu.wpi.first.wpilibj2.command.SequentialCommandGroup;

    public class AutonomousDistance extends SequentialCommandGroup {
      /**
      * Creates a new Autonomous Drive based on distance. This will drive out for a specified distance,
      * turn around and drive back.
      *
      * @param drivetrain The drivetrain subsystem on which this command will run
      */
      public AutonomousDistance(Drivetrain drivetrain) {
        addCommands(
            new DriveDistance(-0.5, 10, drivetrain),
            new TurnDegrees(-0.5, 180, drivetrain),
            new DriveDistance(-0.5, 10, drivetrain),
            new TurnDegrees(0.5, 180, drivetrain));
      }
    }

### Change Motor Direction
We'd like our Romi going in a different direction from that specified in the *romiReference* example.  So switch the first `DriveDistance()` parameter from -0.5 to 0.5.

## References

- FRC Documentation - [Command Groups](https://docs.wpilib.org/en/latest/docs/software/commandbased/command-groups.html)

<!-- <h3><span style="float:left">
<a href="romiCommands">Previous</a></span>
<span style="float:right">
<a href="romiShuffleboard">Next</a></span></h3> -->