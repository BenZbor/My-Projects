# Foam Ball Shooter

This was the project I worked on during the summer of 2025. I wanted to design something with my 3D printer on my own, using an ESP32. I decided to create something to shoot ping pong balls or similar, and I expanded this idea into an automatic foam ball launcher.<br/><br/>

The launcher works by using a motor to drive a pair of 3D printed TPU flywheels, and the foam balls are gravity-fed into these flywheels, launching the ball. The rate of fire is controlled with a mini servo motor, which prevents/allows the balls to fall into the flywheels.<br/><br/>

Below is a summary of how I designed and refined the design, along with images of the CAD and the finished launcher. I also have future plans for the project at the bottom of this document. More images are viewable [here](/Image_Gallery.md). I have also attached CAD files in the CAD Files folder, with a .stl of the assembly and SOLIDWORKS files of each part.

![Image of the foam ball launcher CAD assembly](/Images/Foam-Ball-Launcher-CAD.png)
![Image of the built launcher](/Images/Launcher.jpg)

## Mechanical Design
Before designing anything, I wanted to decide on two main parts of the mechanism: how the balls would be fed into the flywheels, and whether to use one or two motors for the flywheels. I decided to use flywheels (instead of something pneumatic) because it is much simpler, especially when making parts with a 3D printer. Ultimately, I decided to use one motor for both flywheels. This is because I already had one high-power motor, and even if using two identical motors, there would be a slight difference in speed, which is difficult to tune due to being much higher power than the ESP32. Having only one motor also makes the assembly much lighter. For the feeding mechanism, I decided to use a gravity-based system, due to its simplicity. The balls would be dropped into the flywheels in a controlled manner (using a servo), rather than a more complicated option like a solenoid or belt to move them into the flywheels.<br/><br/>

By choosing only to use one motor, I had to figure out how to move the rotation from the motor itself to the 2 driveshafts the flywheels would be attached to. I quickly decided to place the motor in the middle of the assembly, since it is the majority of the weight of the part, and I wanted to ensure it was well balanced, since I was planning to have the barrel tilt and rotate. I narrowed my options to a belt and a gear array. I decided on using belts, since I could buy 3D printer belts, belt gears and idlers. However, standardized gears would not work, and so I would have had to print gears myself. I decided against this because the plastic gears would likely have been much too weak or would have had to be very large to support the high forces of the motor. <br/><br/>

After deciding on how the core part of the shooter would work, I started designing the main feeder/barrel part of the launcher. I designed the parts to be light and strong, while trying to keep the required supports when printing to a minimum. In the end, I decided to make the shooter in one main part, with the motor and belts mounted to a separate, smaller part, as well as a separate magazine part to reduce the print size. This way, the main body would be one strong part, and I could also remove or modify the magazine to reduce its size. I used PETG filament because of its strength and durability, and its ease of printing. <br/><br/>

In the design of the shooter, I wanted to try to minimize any use of hardware (screws, bolts/nuts, etc.) and make the fastening between parts integrated within the design. I used poles protruding from the main body, which inserted into the motor mount assembly to keep it in place. I also added a spot for a zip tie to go around the motor for a more permanent and secure attachment. In most other places, I used friction fits to secure things like bearings and the flywheels.

### Belt Drive Design
![Close-up of the belt drive CAD](/Images/Belt_Drive_CAD.png)
![Belt drive on the actual shooter](/Images/Drive_Belt.jpg)

The most complex part of the shooter to design was the belt drive. The main initial issue with a belt system was that by directly driving both driveshafts, one would spin the wrong way. I solved this by driving a shaft next to one of the driveshafts, and 2 gears to reverse the spinning direction. However, this caused a problem where the smaller shaft (the one with the belt gear and direction reversing gear) would be pulled towards the middle of the belt, creating a lot of friction between the 2 printed gears. I solved this by adding a stabilizing part above it, connecting the shaft to the driveshaft and reducing how much it tilted. When designing the spacing for the belt gears and motors, I tried to design it in such a way that a standard belt loop size would fit; however, I realized I could 'splice' a straight belt, saving the ~$20 it costs for a set of closed-loop belts. This meant I could make it any size I wanted. I initially tried to use this to my advantage by not using a tensioner; however, I quickly realized that the belt was too loose, but too tight with one less tooth in the loop - meaning I needed a tensioner. I designed 2 options, using a screw and a nut to adjust the tightness. I remade the belt and found the ideal tension.<br/><br/>

However, after testing this belt design with a real flywheel, the motor started skipping on the belt. I realized that I either needed to remake the entire belt assembly or fit a tensioner next to the motor, such that the motor would have more belt surface to contact. I settled on modifying the tensioner to be on the outside of the belt, next to the motor shaft. This kept tension and increased the surface area enough to prevent motor skipping.

### Flywheel Design
![Close-up of one of the flywheels in CAD](/Images/Flywheel_CAD.png)

The design of the flywheels was the most tedious, requiring lots of trial and error. I initially used a solid plastic wheel with elastics on the outside to add friction. I thought this would work with the foam balls, since they would deform to create the required force to shoot the ball. However, when testing, I found that this compression had very little stored energy, so the ball would not have much speed exiting the barrel. I tried bigger flywheels to compress the ball more, but the ball did not pass through the flywheels. So, I decided to look into more deformable options. This also had the advantage of being able to shoot harder balls, like ping pong balls or even golf balls if soft enough. I looked into plastic and rubber wheels for suitcases or moving tables, but ultimately settled on buying a roll of TPU filament. This is because TPU is very flexible, and I would be able to print any shape I wanted for the flywheel. I used the TPU as an outside, and the standard plastic (PETG) to secure it to the shaft. I used a few different sizes of flywheel and different amounts of infill to change the stiffness and elasticity of the flywheel. I eventually settled on what I have now, and although it could be better, I feel it is good enough, since it shoots the foam balls with an appreciable speed.<br/><br/>

## Electrical Design

As mentioned earlier, I decided on a single motor for driving the flywheel and a mini servo for controlling when the balls are fired. I decided on these motors because I already had them, so I decided to work with them instead of buying new ones. I also wanted to integrate a tilt and swivel functionality, for which I had multiple high-strength, slow-speed motors. These motors had about 180 degrees of motion, and they had an integrated potentiometer, which can be used to find the position of the motor shaft. <br/>

### Motors

The flywheel motor is a 7.2V motor that was taken from an old cordless drill and draws well over 10A. Because of this, this motor provides more than enough power even at a 1:1 gear ratio, which is fast enough for the flywheels. Because of the high current draw of the motor, standard jumper wires heat up after a short amount of use, and using them even produces an audible speed reduction from the extra resistance. Because of this, I had to make 14-gauge wires specifically for these motors. I purchased 7.4V RC car rechargeable batteries to power the motor, and used a relay to be able to switch the motor on and off using an ESP32 board. <br/><br/>

![Picture of the rotation motor in CAD](/Images/Rotation_Motor_CAD.png)

The motors I used for the swivel functionality were originally part of a car's AC system. The motors consisted of a small 12V DC motor with a large gear ratio outputted to a metal shaft. The shaft was also connected to another gear, which attached to a potentiometer in order to determine the relative position of the motor shaft at any time. This motor/gearbox was a great fit for the swivel design, as it was more than strong enough to move the entire ball shooter, and it allowed me to fine-tune the position with the integrated potentiometer. The only issues were that it was a bit slow when powered by the 7.4V battery, and I couldn't find it on the internet. This meant I had to open it up to tell what was inside and that I had to model it from scratch, which took many iterations to get right, given its strange shape.

### Batteries

I initially wanted to only use a single battery for simplicity and space savings. However, I realized that only using one battery would require me to create voltage steppers and design a PCB. So, for the time being, I am using 2 separate batteries. I need 2 separate batteries because the DC motor runs on 7.2V at a very high current, while my control system (ESP32, explained below) runs on only 5V. Since I already had the 5V battery, it made much more sense to integrate both batteries into the design than to design a custom circuit/PCB to use a single battery. The 5V battery was actually a power bank intended for a phone, with up to 2.1A output - which is more than enough to run the ESP32 and the servo motor. The other battery is a rechargeable 7.4V lithium-ion battery, which is designed for RC cars with high current draw. It also has a separate set of cables for recharging, which allowed me to attach my own connectors to it while using the original charger the batteries came with. 

### Hardware (Microcontroller)

I decided to use an ESP32 to control the launcher over an Arduino Uno, mainly because of future plans I have for this project. I narrowed the options down to these two because they are the ones I currently have. Although the Arduino Uno is more than capable of running the launcher as is, I want to expand its functionality to be mounted on wheels and controllable through an app over Wi-Fi. Because of this, I decided to use an ESP32, which has the processing power and hardware capabilities to do this onboard, unlike the Arduino. The main drawback of the ESP32 was that it only ran at 3.3V, meaning it could not drive the servo. This was solved using a custom board I soldered together in one of my design classes, since it can take a 5V input, and drive small DC motors and the mini servo. It also has lots of useful components for testing, like LEDs, potentiometers, buttons, and switches. The downside of the breakout board is its size, but the benefits far outweigh this. In the future, I may design a smaller board custom to this project alongside the voltage switcher mentioned above.


## Future Improvements

For the future, I have many plans to improve this project. I have already planned and designed many improvements, which I will implement soon. These improvements include complete control from the microcontroller, adding a set of wheels and a swivel mount, making the shooter self-contained, and the ability to remotely control the device. 

![Full CAD of the shooter on the tank](/Images/Full_Tank.png)

### 'Tank' Part

As seen in the image, I plan on converting the ball launcher into its own self-contained foam ball tank. I have already designed all of the printed parts for it, and I am currently figuring out the last few electrical components I need to fit into the tank (relays to control high-voltage motors, transistors to allow the ESP32 to control the relays). The design currently has 4 DC motors for driving the tank, and I am going to use either wheels or tank treads. For the swivelling shooter, I have the 2 motors I mentioned above, with a bracket to attach them. I also made a roof to conceal all the parts, which I designed to be completely optional to help with troubleshooting. I also designed the chassis so that I can easily remove the roof and swap batteries, since this was a big problem in a school design project I spent a lot of time on.

![Inside of the Tank](/Images/Tank_Inside.png)

### Remote Controlling

Alongside the tank improvement, I plan on using the ESP32 to allow remote control of the tank and launcher. I am planning on implementing this using a phone app or a website and communicating with the ESP through Wi-Fi. This will allow me to challenge myself in more complex Arduino code, as well as learn how to make either an app, learn HTML, or similar. The control would have functionality of aiming the swivel, shooting, and moving around. I may also add autonomous routines with sensors, possibly including a camera to show a live view in the app and detect things in its surroundings.
