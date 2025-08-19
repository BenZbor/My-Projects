# Foam Ball Shooter

This was the project I worked on during the summer of 2025. I wanted to design something with my 3D printer on my own. I decided to create something to shoot ping pong balls or similar, and I expanded this idea into an automatic foam ball launcher.


## Design Process
Before designing anything, I wanted to decide on two main parts of the mechanism: how the balls would be fed into the flywheels, and whether to use one or two motors for the flywheels. I decided to use flywheels (instead of something pneumatic) because it is much simpler, especially when making parts with a 3D printer. Ultimately, I decided to use one motor for both flywheels. This is because I already had one high-power motor, and even if using two identical motors, there would be a slight difference in speed, which is difficult to tune due to being much higher power than the ESP32. Having only one motor also makes the assembly much lighter. For the feeding mechanism, I decided to use a gravity-based system, do to its simplicity. The balls would simply be dropped into the flywheels in a controlled manner (using a servo), instead of more complicated options like a solenoid or belt to move them into the flywheels.

After deciding on how the core part of the shooter would work, I started designing the main feeder/barrel part of the launcher. I designed the parts to be light, strong and while trying to keep the required supports when printing to a minimum. 
