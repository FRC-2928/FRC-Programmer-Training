# Solution for the Inches to Meters Exercise
Add a new constant to the *Constants* class.

    public static final double kWheelDiameterMeters = 0.07; // 70 mm

and initialize the encoder in the *Drivetrain* class with the new value.

    m_leftEncoder.setDistancePerPulse((Math.PI * Constants.DriveConstants.kWheelDiameterMeters) / Constants.DriveConstants.kCountsPerRevolution);
    m_rightEncoder.setDistancePerPulse((Math.PI * Constants.DriveConstants.kWheelDiameterMeters) / Constants.DriveConstants.kCountsPerRevolution);

Change the methods in the Drivetrain class to get the metric distances instead of the distances measured in inches. 

    public double getLeftDistanceMeters() {
      return m_leftEncoder.getDistance();
    }

    public double getRightDistanceMeters() {
      return m_rightEncoder.getDistance();
    }

    public double getAverageDistanceMeters() {
      return (getLeftDistanceMeters() + getRightDistanceMeters()) / 2.0;
    }

You'll also need to update the *TurnDegrees* command with the new Drivetrain methods.  Update the `isFinished()` and `getAverageTurningDistance()` methods as shown below.

    public boolean isFinished() {
      double metersPerDegree = Math.PI * 0.141 / 360;
      // Compare distance travelled from start to distance based on degree turn
      return getAverageTurningDistance() >= (metersPerDegree * m_degrees);
    }

    private double getAverageTurningDistance() {
      double leftDistance = Math.abs(m_drive.getLeftDistance());
      double rightDistance = Math.abs(m_drive.getRightDistance());
      return (leftDistance + rightDistance) / 2.0;
    }

In the *DriveDistance* command do a group change of the variable `inches` over to `meters`. You should also change the distance specified in the *AutonoumousDistance* command from `10` to `0.5`, since this is now meters.  

<span style="float:right">
<a href="romiStructure">Back</a></span></h3>