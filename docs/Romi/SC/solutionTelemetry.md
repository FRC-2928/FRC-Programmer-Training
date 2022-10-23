## Solution for Telemetry Lab 
Go to the `setupShuffleboard()` method and add these entries to the Drivetrain tab.  

    m_leftWheelPositionEntry = m_driveTab.add("Left Wheel Pos.", getLeftDistanceMeters())
        .withWidget(BuiltInWidgets.kGraph)      
        .withSize(3,3)  
        .withPosition(4, 0)
        .getEntry();  
    m_rightWheelPositionEntry = m_driveTab.add("Right Wheel Pos.", getRightDistanceMeters())
        .withWidget(BuiltInWidgets.kGraph)      
        .withSize(3,3)
        .withPosition(7, 0)
        .getEntry(); 

The entries are of type NetworkTableEntry, which need to be defined as member variables of the Drivetrain class. 

Place these statements in the `publishTelemetry()` method of the Drivetrain.  These statements will populate the NetworkTable entries every 50 milliseconds giving you a Real-Time view of the data. 

    // Display the distance travelled for each wheel
    m_leftWheelPositionEntry.setDouble(getLeftDistanceMeters());
    m_rightWheelPositionEntry.setDouble(getRightDistanceMeters()); 