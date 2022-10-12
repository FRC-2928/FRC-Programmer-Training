## Solution for Create Heading

Update the Drivetrain class with the following method to get the current heading of the robot.  The `getRotation2d().getDegrees()` method comes from the *Gyro* class that we just implemented in the *RomiGyro* class.

    public double getHeading() {
      return m_gyro.getRotation2d().getDegrees();
    }

The gyro on the Romi shows continuous angles and does not reset when it reaches 360 degrees.  In order for our PID controller to work we need to reset the angle to zero degrees when it passes 360 or -360 degrees.  The new `getHeading()` method is shown below.

    public double getHeading() {
      double angle = getGyroAngleZ();
      double rotations = Math.round(angle/360);
      double heading = angle - (rotations*360);
      return heading;
    }

We also may need to reset the heading:

    public void zeroHeading() {
      m_gyro.reset();
    }

<!-- <span style="float:right">
<a href="romiSubsystems">Back</a></span></h3> -->