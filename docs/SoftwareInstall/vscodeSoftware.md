# VSCode and WPILib Install

An Integrated Development Environment (IDE) is a piece of software that is used to make writing code easier. Some of the main things IDEs will help with include: 

- Check the syntax of your code to ensure you don’t have any errors.
- Organize your files and give you a convenient way to view them.
- Allow you to navigate through your code easily.
- Applies colour schemes to your code so it’s easier to interpret.
- Some will have code completion, which will actually fill in some code for you. 

Visual Studio Code is the supported IDE for programming an FRC robot. If you don't have VSCode installed on your computer then follow the [WPILib Installation Guide](https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-2/wpilib-setup.html) on the FRC Documentation Website. If you're using Windows then these instructions will also install the WPI Library which is used for programming the FRC robot.  The link to download the latest software is somewhat hidden, you can find it under [Prerequisites](https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-2/wpilib-setup.html#prerequisites) on the WPILib Installation Guide pages, click on the Github link.  Or try the [Latest Releases](https://github.com/wpilibsuite/allwpilib/releases) page. Scroll down the Github release page to find the downloads.  Additional information is also found at [FRC Documentation VSCode Overview](https://docs.wpilib.org/en/latest/docs/software/vscode-overview/index.html).

![WPI Installer](../images/FRCTools/FRCTools.022.jpeg)

The offline installer sets up a separate copy of VS Code for WPILib development, even if you already have VS Code on your machine. This is done because some of the settings that make the WPILib setup work may break existing workflows if you use VS Code for other projects.

### Notes for MacOS
Ensure that you eject the WPILib Installer after install, otherwise you will run into problems when a new release is published and you run the new installer.

<!-- For MacOS you would need to install the WPI library via the *Extensions* option in VSCode. Follow the link to the [Visual Studio Code extensions fail to install on macOS](https://docs.wpilib.org/en/latest/docs/yearly-overview/known-issues.html#visual-studio-code-extensions-fail-to-install-on-macos) issue to install the WPI library. You may need to add your home directory to the *Finder* sidebar.  To do this go to Preferences->Sidebar->"Show in sidebar". -->

You will need to install the *XCode Command Line Tools* in order to run the simulator.  To check if you have them installed type `xcode-select -p`.  It should display `/Library/Developer/CommandLineTools` if you already have them installed.

If you don't have the *XCode Command Line Tools* on your machine then enter the command `xcode-select --install` to install. The package is about 1.2 GB.  For additional information see [How to Install Xcode Command Line Tools on a Mac](https://www.freecodecamp.org/news/install-xcode-command-line-tools/).

If the simulator crashes try [Simulation crashes on Mac after updating WPILib](https://docs.wpilib.org/en/stable/docs/yearly-overview/known-issues.html#simulation-crashes-on-mac-after-updating-wpilib) to resolve the issue.  If that doesn't work then try the solution in [macOS Simulation fails to launch in Visual Studio Code](https://docs.wpilib.org/en/2020/docs/software/wpilib-overview/known-issues.html#macos-simulation-fails-to-launch-in-visual-studio-code).

Occasionally, the system will throw a **Killed: 9** when trying to start the Simulator.  In most cases, rebooting your Mac will solve the issue.

## Upgrading Software
A new version of the WPILib is released each year on the same day that the new competition is announced. It's important that everyone has the latest software.  Upgrading to the new version is essentially the same as doing a new install.  Here's a direct link to the latest  [Release down load page](https://github.com/wpilibsuite/allwpilib/releases).  Follow the instructions for [Running the Installer](https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-2/wpilib-setup.html#running-the-installer).

After upgrading VSCode will import each project to the latest version.  Answer *Yes* to each prompt.

![After Upgrade](../images/FRCTools/FRCTools.011.jpeg)

As soon as you open the upgraded project VSCode will start to run a compile.  This will often produce build errors that you need to resolve. You will probably have to import 3rd party software like *Pheonix* and *REVLib*.  Instructions on installing and configuring these libraries is detailed in the next section. Also see [Importing a Gradle Project](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/importing-gradle-project.html) for more details on importing projects from previous season.

To upgrade from the kickoff release simply download and run the new installer and it will update your current installation. If you already have the latest WPILib vscode installed, it will detect it and you can simply click “next” for that installation step. After installation, vscode will prompt you when opening your robot project whether you want to upgrade it to this version. During the build season there will most likely be new sub-releases published that require the use of the WPILib installer. 

<!-- #### Upgrading VSCode
It's important that everyone has the latest software otherwise you won't be able to work on other team mates code.  After upgrading VSCode will import each project to the latest version.  Answer *Yes* to each prompt.

![After Upgrade](../images/FRCTools/FRCTools.011.jpeg) -->
