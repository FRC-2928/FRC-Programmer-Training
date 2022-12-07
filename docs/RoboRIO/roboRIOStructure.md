# Robot Structure
The structure of the competition roboRIO robot is very similar to that of the Romi.  The subsystems, game controllers, and *AutoChooser* menu are all defined within the *RobotContainer* class above its constructor.  The subsystems are configured when the *RobotContainer* object is constructed.  Each subsystem will designate its default command, which is the command that runs when no other command is using the subsystem, and setup any game controller buttons that are used to control the subsystem.

The **autoChooser** menu is also created when the *RobotContainer* object is built.  The **autoChooser** menu is used to select an autonomous command. The general structure of the *RobotContainer* class is show below.  

![RobotContainer Structure](../../images/FRCroboRIO/FRCroboRIO.002.jpeg)

The game controllers will be mapped to commands via a class called *DriverOI*.  You'll learn how to do this in the [Joysticks and Game Controllers](roboRIOJoysticks.md) tutorial.

The *Drivetrain* subsystem uses comments to divide it into four logical subsections; **Initialization**, **Control Input**, **System State**, and **Process Logic**.  This is the same division that we did in an eariler tutorial on the Romi and is described in the [Subsystems - Add Comments Lab](../Romi/SC/romiSubsystems.md#addComments).  One notable difference between the roboRIO robot and the Romi is how the drivetrain motors are configured.  These motors are more complex that the ones used on the Romi and have many more features that can be customized.  The Drivetrain also includes a *Transmission* class that can put the robot into high or low gear.  We'll look at that in the [Pneumatics Control](pneumatics.md) tutorial.

## Lab - Robot Structure
There are three tasks for this lab:

- Folk the *SweetpantsReference* program for the training roboRIO robot into your own Github account.

- Clone to *SweetpantsReference* program to your laptop.

- Look at the code from the training roboRIO robot to learn how it's structured.  

### Folk the Reference Program
We're going to start our training on the roboRIO robots by **Folking** a reference program for a robot that we call *Sweetpants*. This will put a copy of the program into your own Github account so as you can make changes to it.

1. Navigate to the [SweetpantsReference](https://github.com/FRC-2928/SweetpantsReference) program for the training roboRIO robot.  

2. Follow the instructions on [Folking a Repository](../Tools/git.md#gitFolk) in this training guide to get a copy of the program into your own Github account.

3. Check your Github account to ensure that the new repository has been copied.  While you're there copy the repository URL by clicking on the **Code** button.

You're now done with this task!

### Clone the Reference Program
Now that you have your own copy of the reference program you can clone it onto your laptop and make changes to it.

In VSCode, clone the reference program for the training roboRIO robot from your own *SweetpantsReference* repository.  For a reminder on how to do this refer to [Cloning a Repository](../Tools/git.md#gitClone).

Once you have your own personal copy to the reference program you're done with this task!

### Learn the roboRIO Code Structure
The purpose of this task is to familiarize youself with the roboRIO code.

1. Look at the *RobotContainer* class in VSCode and identify each code block that's shown in the RobotContainer Structure image above.

2. Look at the *Drivetrain* class and note the methods that are in each subsection of the code.

After reviewing the code you're done with this task!

## References

- Example code [SweetpantsReference](https://github.com/FRC-2928/SweetpantsReference)