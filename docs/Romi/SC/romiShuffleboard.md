# Using Shuffleboard
When creating Commands and Subsystems for our robot it'll be necessary to see the data that's getting generated.  Remember that robots are data driven machines, so in order to test our code we'll need to see the data.
For that purpose we'll be using WPI tool called **Shuffleboard**, which will allow us to view all of the data coming from the robot in real time.  It also enables you to send data to the robot in order to make commands more flexible and change the behaviour of the robot.

The documentation explains how to [start Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/getting-started/shuffleboard-tour.html#starting-shuffleboard) depending on your development laptop.

## Parameterizing the AutoDistance Command
We're going to enhance the *AutoDistance* command in order to accept input from Shuffleboard.  The current command moves the robot forward and backward 10 inches but with the help of Shuffleboard we'll be able to control how far the robot moves.



Switch direction

Convert everything to meters because that's what we'll use for characterization.

Exercise - Have angle accept input from Shuffleboard

Exercise - Travel in a square, need new command AutonomousSquare.

## References

- FRC Documentation - [Shuffleboard](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/shuffleboard/index.html)

- Code Example - [RomiShuffleboard](https://github.com/mjwhite8119/romi-examples/tree/main/RomiShuffleboard)

<!-- <h3><span style="float:left">
<a href="romiCommandGroups">Previous</a></span>
<span style="float:right">
<a href="romiPID">Next</a></span></h3> -->