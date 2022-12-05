# Joysticks and Game Controllers
This tutorial shows how to map keys on a game controller to commands that will be run on the robot. On the competition robot you'll be using two game controllers one for the driver and the other for the operator.  Each controller will map to a set of commands that are appropriate for the robot driver and operator.  The mapping is determined by the nature of the competition game rules and how tasks are divided between the driver and operator.  Some commands will be duplicated on both controllers.  

It can become difficult to keep track of this key mapping, especially as more commands are added, so there needs to be a good way of organizing them.

## The *DriverOI* Class
The *DriverOI* class is used to accept **O**perator **I**nput from game controllers.  It's main purpose is to map keys on the game controller to function names that describe the operation that's to be performed.  It somewhat follows a functional programming paradigm which describes *what* the function is doing and not *how* to do it.

Talk about inline commands...

## Lab - Joysticks and Gamepads
There's one task for the lab:

- Create the OperatorOI class to map keys to commands

## References
- [Joysticks](https://docs.wpilib.org/en/latest/docs/software/basic-programming/joystick.html#joysticks) - FRC Documentation 