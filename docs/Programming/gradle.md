# Gradle

*Gradle* is a build automation tool.  Build process take your application source code and converts it into byte code that a computer can understand.  This is called compiling.  In the case of Java it's going to compile it into a `.jar` file.  You can compile a Java source file manually by using the `javac` executable but as you get multiple files and libraries as part of your program this become tedious and error prone.


## Why use Gradle?
Speeds up the build since it keeps track of what parts of the code has changed and doesn't recompile them.


## Gradle Install
Gets installed with the WPI installer.  May require the extension *Lanquage Support for Java*.

Also has the `settings.gradle` file.  
Has the wrapper files to run the script `gradlew` (Mac, Linux), and `gradlew.bat` (Windows)

Has the `.gitignore` file to prevent the build directories from being committed to Github. 

When you create a new project it sets it up as a gradle project.

## Running Gradle
Gradle expects all source files to be in `src/main/java`.  When you create a project this is setup by default.

To build and deploy a robot project, do one of:

- Open the Command Palette and enter/select “Build Robot Code”

- Open the shortcut menu indicated by the ellipses in the top right corner of the VS Code window and select “Build Robot Code”

- Right-click on the build.gradle file in the project hierarchy and select “Build Robot Code”


`gradlew build`.  Doesn't run on Mac since the command can't be found????

Uses a build script called `build.gradle` that lists all of the dependences.  These dependencies are downloaded from the Internet as they are needed.

There are two plugins in the `build.gradle` file:

    plugins {
        id "java"
        id "edu.wpi.first.GradleRIO" version "2022.4.1"
    }

Creates a new directory called `build` which holds all of the build files.

## Other Tasks


## References

FRC Documentation [Advanced GradleRIO](https://docs.wpilib.org/en/stable/docs/software/advanced-gradlerio/index.html)