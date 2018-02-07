# ActionRunner
ActionRunner is a library aimed at rookie to low tier FRC teams, or teams who simply do not have good programming. It's easy to use, extremely flexible, and can be used to run multiple autonomous modes easily and (semi-) elegantly!

### Usage
```java
public class Robot extends IterativeRobot {
    //This is a simple example to show off
    //What this library is capable of
    
    DifferentialDrive drive = new DifferentialDrive(0, 1);
    SendableChooser<ActionRunner> autoChooser = new SendableChooser<>();
    
    public void robotInit() {
        ActionRunner baselineAuto = new ActionRunner();
        baselineAuto.addAction(() -> {
            //Go forward at 25% speed for 5 seconds
            drive.arcadeDrive(0.25, 0);
            
            return Timer.hasPeriodElapsed(5);
        });
        baselineAuto.addAction(() -> {
            //Turn right as 25% speed for 2 seconds
            drive.arcadeDrive(0, 0.25);
            
            return Timer.hasPeriodElapsed(2);
        });
        baselineAuto.onActionComplete(() -> {
            //Stop the robot after every action
            drive.arcadeDrive(0, 0);
            
            return Timer.hasPeriodElapsed(1);
        });
        
        autoChooser.addObject(baselineAuto);
        SmartDashboard.putData("autonomous", autoChooser);
    }
    
    public void autonomousInit() {
        autoChooser.getSelected().initialize();
    }
    
    public void autonomousPeriodic() {
        autoChooser.getSelected().update();
        //This next thing is optional but I'd do it
        SmartDashboard.putBoolean("autonomous done", autoChooser.getSelected().isDone());
    }
}
```
***Note:*** This is not the code everyone should write for this. Don't copy/paste this code and hope for the best. It's up to *you* to use it the way *you* need to.

### Installation
As of right now there are 2 ways to use ActionRunner.
1. Download the JAR
    1. Click on releases
    2. Download the JAR that should *hopefully* be there
    3. Add the JAR to your classpath in eclipse (I don't know that one, I use gradle and [gradlerio](https://github.com/Open-RIO/GradleRIO).)
2. Copy and paste the sources
    1. Make a new package in the project and name it `frc.team2186.actionrunner`
    2. Copy and paste the sources into that package
    
### Building
For the more adventurous of you, you can try building this library. ActionRunner uses gradle as it's build system, so the process for building looks something like this.
1. Clone the directory
2. Open a command line or terminal in the cloned directory
3. Run `gradlew build`
4. The built JAR should be in build/libs

### Author
ActionRunner was written by [Kenny Raisbeck](https://github.com/drwaterycat). ActionRunner is governed by the [MIT License](LICENSE). Copyright 2018.