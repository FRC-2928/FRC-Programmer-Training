# Robot Simulation
Why simulation...


## Lab - Robot Simulation
Create the simulated objects for the motors and gyro.

    /* Object for simulated inputs into Talon. */
	TalonFXSimCollection m_leftDriveSim = leftLeader.getSimCollection();
	TalonFXSimCollection m_rightDriveSim = rightLeader.getSimCollection();
  
	/* Object for simulated inputs into Pigeon. */
	BasePigeonSimCollection m_pigeonSim = pigeon.getSimCollection();

Create the simulated model for the drivetrain.

    /* Simulation model of the drivetrain */
	DifferentialDrivetrainSim m_driveSim = new DifferentialDrivetrainSim(
		DCMotor.getFalcon500(2),  //2 Falcon 500s on each side of the drivetrain.
		DrivetrainConstants.lowGearRatio,    //Low Gearing reduction.
		2.1,                //The moment of inertia of the drivetrain about its center.
		26.5,                         //Mass of the robot in kg.
		DrivetrainConstants.kWheelDiameterMeters/2,  //Robot wheel radius.
		DrivetrainConstants.kTrackWidthMeters,     //Distance between wheels.
	
		/*
		 * The standard deviations for measurement noise:
		 * x and y:          0.001 m
		 * heading:          0.001 rad
		 * l and r velocity: 0.1   m/s
		 * l and r position: 0.005 m
		 */
		null //VecBuilder.fill(0.001, 0.001, 0.001, 0.1, 0.1, 0.005, 0.005) //Uncomment this line to add measurement noise.
	  );

## References