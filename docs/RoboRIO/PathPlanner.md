# Path Planner

PathPlanner is a motion profile generator for FRC robots. Path planning involves determining the trajectory or path that a robot should follow to accomplish specific tasks efficiently and accurately.

## PathPlanner Library
Follow the [Install PathPlannerLib](https://pathplanner.dev/pplib-getting-started.html) instructions to make the library available to your project.

### Inputs and Outputs


Configures the AutoBuilder for a holonomic drivetrain.
   *
   * @param poseSupplier a supplier for the robot's current pose
   * @param resetPose a consumer for resetting the robot's pose
   * @param robotRelativeSpeedsSupplier a supplier for the robot's current robot relative chassis
   *     speeds
   * @param robotRelativeOutput a consumer for setting the robot's robot-relative chassis speeds
   * @param config {@link com.pathplanner.lib.util.HolonomicPathFollowerConfig} for configuring the
   *     path following commands
   * @param shouldFlipPath Supplier that determines if paths should be flipped to the other side of
   *     the field. This will maintain a global blue alliance origin.
   * @param driveSubsystem the subsystem for the robot's drive

### Configuration

Create a new holonomic path follower config
   *
   * @param translationConstants {@link com.pathplanner.lib.util.PIDConstants} used for creating the
   *     translation PID controllers
   * @param rotationConstants {@link com.pathplanner.lib.util.PIDConstants} used for creating the
   *     rotation PID controller
   * @param maxModuleSpeed Max speed of an individual drive module in meters/sec
   * @param driveBaseRadius The radius of the drive base in meters. This is the distance from the
   *     center of the robot to the furthest module.
   * @param replanningConfig Path replanning configuration
   * @param period Control loop period in seconds (Default = 0.02)

### Dynamic Pathing

Command pathfindToPoseFlipped
Build a command to pathfind to a given pose that will be flipped based on the value of the path
   * flipping supplier when this command is run. 
   *
   * @param pose The pose to pathfind to. This will be flipped if the path flipping supplier returns
   *     true
   * @param constraints The constraints to use while pathfinding
   * @return A command to pathfind to a given pose

by changing new ReplanningConfig() to new ReplanningConfig(true, true). Thiss allows the AutoBuilder to correct for error while running the path.
/**
   * Create a path replanning configuration
   *
   * @param enableInitialReplanning Should the path be replanned at the start of path following if
   *     the robot is not already at the starting point?
   * @param enableDynamicReplanning Should the path be replanned if the error grows too large or if
   *     a large error spike happens while following the path?
   * @param dynamicReplanningTotalErrorThreshold The total error threshold, in meters, that will
   *     cause the path to be replanned
   * @param dynamicReplanningErrorSpikeThreshold The error spike threshold, in meters, that will
   *     cause the path to be replanned
   */
  public ReplanningConfig

## References

[Pathplanner Documentation](https://pathplanner.dev/home.html)