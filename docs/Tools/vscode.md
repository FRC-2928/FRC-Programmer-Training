# Visual Studio Code
An Integrated Development Environment (IDE) is a piece of software that is used to make writing code easier. Some of the main things IDEs will help with include: 

- Check the syntax of your code to ensure you don’t have any errors.
- Organize your files and give you a convenient way to view them.
- Allow you to navigate through your code easily.
- Applies colour schemes to your code so it’s easier to interpret.
- Some will have code completion, which will actually fill in some code for you. 

Visual Studio Code is the supported IDE for programming an FRC robot. If you don't have VSCode installed on your computer the best way of doing this is to follow the instructions on the [FRC Documentation Website](https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-2/wpilib-setup.html). These instructions will also install the WPI Library which is used for programming the FRC robot. Additional information is also found at [FRC Documentation VSCode Overview](https://docs.wpilib.org/en/latest/docs/software/vscode-overview/index.html). The link to download the latest software is somewhat hidden, you can find it under [Prerequisites](https://docs.wpilib.org/en/latest/docs/zero-to-robot/step-2/wpilib-setup.html#prerequisites) and click on the Github link.  Scroll all the way down to the bottom of the Github release page to find the downloads.

It's important that everyone has the latest software otherwise you won't be able to work on other team mates code.  After upgrading VSCode will import each project to the latest version.  Answer *Yes* to each prompt.

![After Upgrade](../images/FRCTools/FRCTools.011.jpeg)

## Installing Pheonix Software
The **Cross The Road Electronics** vendor libraries must be installed installed into the `wpilib/20YY` directory.  Here are the [Phoenix Software](https://store.ctr-electronics.com/software/) releases for each platform. 

When you run the Phoenix software installer for Windows it installs the software in the requisit directories.  For Mac and Linux follow the *Install Pheonix on Mac and Linux* instructions below once you've downloaded the zip file.

Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option.  This is detailed in the FRC documentation [Adding Offline Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#adding-offline-libraries) and also in the CTRE Documentation to [Create a Project](https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html).

### Install Pheonix on Mac and Linux
There are three folders contained in this archive-
- "maven"
- "vendordeps"
- "Robotbuilder"

For VSCode Integration:
Place the `maven/com/ctre` and `vendordeps/Pheonix.json` folders inside the WPILib installation folder.
This is typically located in `~/wpilib/2022`.

Instructions on adding Phoenix to your FRC vscode project can be found [here](https://phoenix-documentation.readthedocs.io/en/latest/ch05a_CppJava.html).


For **Robotbuilder**:
The Robotbuilder folder contains Robotbuilder extensions for using CTRE classes.
It currently contains the Talon FX, CANCoder, Talon SRX, Victor SPX, CANifier, and Pigeon IMU extensions.

Take the desired extension folder (eg. "Talon SRX") and place it in `~\wpilib\2022\Robotbuilder\extensions`.

## Using Live Share
Live Share enables you to quickly collaborate with a friend, classmate, or professor on the same code without the need to sync code or to configure the same development tools, settings, or environment.

#### Step 1. Install the Live Share Extention

![Live Share Install](../images/Tools/LiveShareInstall.png)

#### Step 2. Start a Live Share Session

You will be asked to share your Visual Studio screen via either a Microsoft or GitHub account. If you have one of these accounts then pick it.  Otherwise you will need to create an account with one of these organizations.  Github is also owned by Microsoft.

![Start Live Share](../images/FRCTools/FRCTools.009.jpeg)

#### Step 3. Verify your Account

If you are using Live Share via Github you will be asked to authorize sharing.  Click on <i>Authorize VisualStudioLiveShare</i>.  You will only need to do this the first time that you use Live Share.

![Live Share Github](../images/Tools/LSUseGithub.png)

Then click on <i>Open Visual Studio Code</i>.  A URL will automatically get saved in you clip board. You will need this to send to the person you are sharing with.

![Live Share Verify](../images/Tools/LSVerifyGithub.png)

Allow the extension to open the URL.

![Live Share Allow Share](../images/Tools/LSAllowShare.png)

#### Step 4. Start Shared Session on Remote Computer
Once you have received the URL for the shared session, click on the Live Share Icon and select <i>Join</i>

![Receive Session](../images/Tools/LSStartSession.png)

A box will pop up for you to enter in the URL that you have received from your classmate or mentor. Paste in the URL and press enter.  You will be given the choice to join Anonymously or through your Microsoft/Github account.  If you join Anonymously, you won't be able to edit the other person's code.

![Enter Session URL](../images/Tools/LSJoin.png)

The person sharing their session will receive the following message.  Click <i>Accept</i> to share your screen.

![Accept Session](../images/Tools/LSAcceptSession.png)

#### Step 5. End Live Share Session
To end a Live Share session click on the **X** next to the participants name.

![End Session](../images/Tools/LSEndSession.png)

## Notes on Compilation
There is an issue where a project created on a Windows laptop would not compile on the Mac.  Need to investigate further.

## References

- FRC Documentation - [VSCode Overview](https://docs.wpilib.org/en/latest/docs/software/vscode-overview/index.html)

- [Install and Use Visual Studio Code on Windows 10](https://www.youtube.com/watch?v=MlIzFUI1QGA)

- [How to Install Visual Studio Code on Mac](https://www.youtube.com/watch?v=tCfbi5PF1y0)

- [VSCode Crash Course](https://www.youtube.com/watch?v=WPqXP_kLzpo)

<!-- <h3><span style="float:left">
<a href="../index">Home</a></span> -->