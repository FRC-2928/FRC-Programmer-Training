# Solution for Add Reset Odometry Command
Create an *InstantCommand* called *ResetOdometry* from the left files panel in VSCode, and make the following changes:

        private static Drivetrain m_drive;

        public ResetOdometry(Drivetrain drive) {
            m_drive = drive;
            addRequirements(drive);
        }

        // Called when the command is initially scheduled.
        @Override
        public void initialize() {
            m_drive.resetGyro();
            m_drive.resetEncoders();
        }

We'll execute this command from the *SendableChooser* menu.

    m_chooser.addOption("Reset Odometry", new ResetOdometry(m_drivetrain));
