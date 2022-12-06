# Joysticks and Game Controllers
This tutorial shows how to map keys on a game controller to commands that will be run on the robot. On the competition robot you'll be using two game controllers one for the driver and the other for the operator.  Each controller will map to a set of commands that are appropriate for the robot driver and operator.  The mapping is determined by the nature of the competition game rules and how tasks are divided between the driver and operator.  Some commands will be duplicated on both controllers.  

It can become difficult to keep track of this key mapping, especially as more commands are added, so there needs to be a good way of organizing them.

## The *DriverOI* Class
The *DriverOI* class is used to accept **O**perator **I**nput from game controllers.  It's main purpose is to map keys on the game controller to function names that describe the operation that's to be performed.  It somewhat follows a functional programming paradigm which describes *what* the function is doing and not *how* to do it.

<!-- Talk about inline commands... -->

## Lab - Joysticks and Game Controllers
There's one task for the lab:

- Create the *OperatorOI* class to map keys to the commands that will be initiated from the operator's game controller.

### Create the *OperatorOI* Class

Create a new empty class from the files panel in VSCode.  Right mouse-click on the `oi` folder and select "Create a new class/command".  Select "Empty Class" from the dropdown and enter *OperatorOI* for the name of your new class.

This class will be using the *XboxController*, so inside of the curly braces create a member variable for it:

        private XboxController m_controller;

Every class needs a constructor so create one for this class.  It must have the same name as the class.  Capture the *XboxController* and assign it to the method variable.

    public OperatorOI(XboxController controller) {
        m_controller = controller;
    }

Create two buttons to shift the transmission gears. The methods that return the buttons should be descriptive of the function that they will perform, such as `getShiftLowButton()`. When prompted to import the Button class make sure that you select the `edu.wpi.first.wpilibj2.command.button.Button`. The buttons will be assigned to the **X** and **Y** buttons of the game controller.

    public Button getShiftLowButton() {
        return new JoystickButton(m_controller, XboxController.Button.kX.value);
    }

    public Button getShiftHighButton() {
        return new JoystickButton(m_controller, XboxController.Button.kY.value);
    }

You should now have a functional *OperatorOI* class.  To test it you'll need to add it to the RobotContainer. In the *RobotContainer* class create a new method variable called `m_operatorController` of type *XboxController* that will get assigned to the operators game controller.  Assign it to port `1` since the driver's controller is already assigned to port `0`. 

    private final XboxController m_operatorController = new XboxController(1);

Create a method variable called `m_operatorOI` and assign the new `m_operatorController` to it.

    private final OperatorOI m_operatorOI = new OperatorOI(m_operatorController);

In the `configureDrivetrain()` method add the button commands to shift the *Transmission* gears.

    m_operatorOI.getShiftLowButton().whenPressed(new InstantCommand(m_transmission::setLow, m_transmission));
    m_operatorOI.getShiftHighButton().whenPressed(new InstantCommand(m_transmission::setHigh, m_transmission));

#### Testing the OperatorOI Class

    

## References
- [Joysticks](https://docs.wpilib.org/en/latest/docs/software/basic-programming/joystick.html#joysticks) - FRC Documentation 