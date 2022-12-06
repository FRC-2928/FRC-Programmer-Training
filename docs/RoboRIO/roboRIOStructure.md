# Robot Structure
The structure of the competition roboRIO robot is very similar to that of the Romi.  The subsystems, game controllers, and *AutoChooser* menu are all defined within the *RobotContainer* class above its constructor.  The subsystems are configured when the *RobotContainer* object is constructed.  Each subsystem will designate its default command, which is the command that runs when no other command is using the subsystem, and setup any game controller buttons that are used to control the subsystem.

The **autoChooser** menu is also created when the *RobotContainer* object is built.  The **autoChooser** menu is used to select an autonomous command. The general structure of the *RobotContainer* class is show below.  

![RobotContainer Structure](../../images/FRCroboRIO/FRCroboRIO.002.jpeg)

The game controllers will be mapped to commands via a class called *DriverOI*.  You'll learn how to do this in the [Joysticks and Game Controllers](roboRIOJoysticks.md) tutorial.

The *Drivetrain* subsystem uses comments to divide it into four logical subsections; **Initialization**, **Control Input**, **System State**, and **Process Logic**.  This is the same division that we did in an eariler tutorial on the Romi and is described in the [Subsystems - Add Comments Lab](../Romi/SC/romiSubsystems.md#addComments).  One notable difference between the roboRIO robot and the Romi is how the drivetrain motors are configured.  These motors are more complex that the ones used on the Romi and have many more features that can be customized.  The Drivetrain also includes a *Transmission* class that can put the robot into high or low gear.  We'll look at that in the [Pneumatics Control](pneumatics.md) tutorial.

## Lab - Robot Structure
There are two tasks for this lab:

- Clone the reference program for the training roboRIO robot and put a copy of it into your own  Github repository.

- Look at the code from the training roboRIO robot to learn how it's structured.  

### Clone the Reference Program
We're going to start our training on the roboRIO robots by cloning a reference program for a robot that we call *Sweetpants*. You'll then upload this reference program to your own Github account so as you can make changes to it.

1. In VSCode, clone the reference program for the training roboRIO robot from the [SweetpantsReference](https://github.com/FRC-2928/SweetpantsReference) repository.  For a reminder on how to do this refer to [Cloning a Repository](../Tools/git.md#gitClone).

2. Create a repository in your own Github account called **SweetpantsReference**.  You can follow the instructions for [Pushing to the Remote Repository](../Tools/git.md#gitPush) in this training guide to see how to create a repository.  You'll need to login to your Github account first.

3. Change the remote origin of the **SweetpantsReference** repository from `FRC-2928` to your own repository URL.  From a terminal execute the following command where `<repository>` is you own repository account name. 

        git remote set-url origin https://github.com/<repository>/SweetpantsReference.git

    
4. Type in `git remote -v` to check that the remote repository has changed to your account.       

5. Push the code to your Github repository by executing the following command in the terminal:

        git push -u origin main

6. Check your Github repository to ensure that the new code has been pushed.

You're now done with this task!

### Learn the roboRIO Code Structure
The purpose of this task is to familiarize youself with the roboRIO code.

1. Look at the *RobotContainer* class in VSCode and identify each code block that's shown in the RobotContainer Structure image above.

2. Look at the *Drivetrain* class and note the methods that are in each subsection of the code.

After reviewing the code you're done with this task!

## References

- Example code [SweetpantsReference](https://github.com/FRC-2928/SweetpantsReference)