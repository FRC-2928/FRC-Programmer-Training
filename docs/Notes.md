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


        Exception in thread "JavaFX Application Thread" java.lang.NullPointerException: Cannot invoke "edu.wpi.first.pathweaver.Waypoint.isLockTangent()" because the return value of "javafx.beans.value.ObservableValue.getValue()" is null
        at edu.wpi.first.pathweaver.EditWaypointController.lambda$enableSaving$11(EditWaypointController.java:170)
- 