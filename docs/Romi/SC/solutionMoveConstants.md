## Solution for Move Constants 
The *Constants* file can end up getting very crowded so it's a good idea to group the constants under the appropriate subsystem as much as possible.  To do that we can create a subclass within the Constants file.  In our case, we've called this *DriveConstants*.   Update the Constants file with the following code.  You can cut and paste the two lines from the Drivetrain class.  You must also change the constant from `private` to `public`. 

    public final class Constants {
        public final class DriveConstants {
            public static final double kCountsPerRevolution = 1440.0;
            public static final double kWheelDiameterInch = 2.75591; // 70 mm
        }
    }    

Now go to the Drivetrain class and comment out the two lines that you just copied.  Reference the new constants in the `setDistancePerPulse` method in the Drivetrain constructor. You'll be prompted to import the *DriveConstants* class that you created in the *Constants* file.

    m_leftEncoder.setDistancePerPulse((Math.PI * DriveConstants.kWheelDiameterInch) / DriveConstants.kCountsPerRevolution);
        m_rightEncoder.setDistancePerPulse((Math.PI * DriveConstants.kWheelDiameterInch) / DriveConstants.kCountsPerRevolution);
    
Move the following constant from the `isFinished()` method in the *TurnDegrees* command file

        public static final double kMetersPerDegree = Math.PI * 0.141 / 360;

and reference it in the *TurnDegrees* command.  

    return getAverageTurningDistance() >= (DriveConstants.kMetersPerDegree * m_degrees);

<span style="float:right">
<a href="romiStructure">Back</a></span></h3>