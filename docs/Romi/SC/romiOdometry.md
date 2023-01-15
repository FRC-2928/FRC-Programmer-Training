# Kinematics and Odometry

*ChassisSpeeds*. See [The Chassis Speeds Class](https://docs.wpilib.org/en/latest/docs/software/kinematics-and-odometry/intro-and-chassis-speeds.html#the-chassis-speeds-class)



For more information on differential drive kinematics see the [Kinematics](../../Concepts/Dynamics/kinematics.md) module.     

More information can be found in the [Robot Kinematics](../../../Concepts/Dynamics/kinematics) section of this training guide.

To learn more about Pose Estimation and Odometry see the [Pose Estimation](../../../Concepts/OptimalEstimation/poseEstimation) module of this training guide.

## Lab - Kinematics and Odometry
There are two tasks for this lab:

- Setup Robot Kinematics

- Setup Robot Odometry

### <a name="kinematicsTask"></a>Setup Robot Kinematics
The *DifferentialDriveKinematics* class allows us to use the trackwidth (horizontal distance between the wheels) to convert from chassis speeds to wheel speeds.  Internally, it uses the *DifferentialDriveWheelSpeeds* and *ChassisSpeeds* classes.  The following diagram shows how this is structured.

![DifferentialDriveKinematics](../../images/FRCKinematics&Odometry/FRCKinematics&Odometry.007.jpeg)


The trackwidth of the robot is of course a constant value, so the following value should be placed in the *Contants* file.

    public static final double kTrackwidthMeters = 0.142072613;
    public static final DifferentialDriveKinematics kDriveKinematics =
        new DifferentialDriveKinematics(kTrackwidthMeters);

Create the following method in the Drivetrain.  When we create commands to run trajectories later in the course we'll require a class that returns *DifferentialDriveWheelSpeeds*.

    public DifferentialDriveWheelSpeeds getWheelSpeeds() {
        return new DifferentialDriveWheelSpeeds(m_leftEncoder.getRate(), m_rightEncoder.getRate());
    }

In our previous Telemetry lab we had created two commands in the `publishTelemetry()` function to output the wheel speeds of the drivetrain.  Now we'll replace those commands with the following code where we get the *DifferentialDriveWheelSpeeds*.

    DifferentialDriveWheelSpeeds wheelSpeeds = getWheelSpeeds();
    SmartDashboard.putNumber("Left wheel speed", wheelSpeeds.leftMetersPerSecond);
    SmartDashboard.putNumber("Right wheel speed", wheelSpeeds.leftMetersPerSecond); 


### <a name="odometryTask"></a>Setup Robot Odometry
In this task we'll setup the odometry in order to track the robot's position and orientation on the field and show it on the dashboards.  The dashboards will show a diagram of the field with the current position and orientation of the robot within it.  We'll use the [DifferentialDriveOdometry](https://docs.wpilib.org/en/latest/docs/software/kinematics-and-odometry/differential-drive-odometry.html#differential-drive-odometry) and [Field2d](https://docs.wpilib.org/en/latest/docs/software/dashboards/glass/field2d-widget.html#the-field2d-widget) classes of WPILib to accomplish this.

Add the following class members to the *Drivetrain* class.  The *DifferentialDriveOdometry* object is used to keep track of the robot's position and orientation (Pose), and the *Field2d* object will display the pose to the dashboards.

    // Odometry class for tracking robot pose
    private final DifferentialDriveOdometry m_odometry;

    // Show a field diagram for tracking odometry
    private final Field2d m_field2d = new Field2d();

Place the following code in the contructor.  Initialize the robot's pose and odometry.

    Pose2d initialPose = new Pose2d(0, 0, m_gyro.getRotation2d()); 
    m_field2d.setRobotPose(initialPose);

    m_odometry = new DifferentialDriveOdometry(m_gyro.getRotation2d(), 
                   getLeftDistanceMeters(), getRightDistanceMeters(), 
                   initialPose);

In order for the field information to update in the dashboards we need to update it in the `periodic()` method of the Drivetrain.  Place this code into the `publicTelemetry()` method, which gets called from `periodic()`. 

    // Update the odometry in the periodic block
    Pose2d currentPose = m_odometry.update(m_gyro.getRotation2d(), 
                                           m_leftEncoder.getDistance(), 
                                           m_rightEncoder.getDistance());
    
    m_field2d.setRobotPose(currentPose); 

Finally, there needs to be a method to reset the odometry back to the starting point.  This method will also reset the encoders and zero the heading of the gyro.

    public void resetOdometry(Pose2d pose) {
        resetEncoders();
        resetGyro();
        m_odometry.resetPosition(m_gyro.getRotation2d(),
            getLeftDistanceMeters(), getRightDistanceMeters(),      
    }

#### Testing the Odometry    

## References
- FRC documentation - [Kinematics and Odometry](https://docs.wpilib.org/en/latest/docs/software/kinematics-and-odometry/index.html)
