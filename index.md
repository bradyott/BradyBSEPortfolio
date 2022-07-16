# Gesture Controlled Robot Car
This will serve as a brief description of your project. Limit this to three sentences because it can become overly long at that point. This copy should draw the user in and make she/him want to read more.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Brady O. | Council Rock North | Mechanical Engineering | Incoming Junior

![Headstone Image](https://github.com/BlueStampEng/BSE_Template_Portfolio/blob/4655d8c4b2f1d0fa5912511d0b39542520b9f88e/branding/BlueStamp-Engineering-Logo-White.png)
  
# Final Milestone
My final milestone was connecting the Arduino Micro which worked with the accelerometer to the Arduino Uno, which worked with the DC motors. I did this by wiring HC-05 Bluetooth modules to both the Arduino Micro and the Arduino Uno. The wiring of these was particularly trickey because certain pins on the Bluetooth module required resisttors to change their current from 5V to 3.3V. Another tricky aspect was the debugging of the Arduino Uno code, since I learned that some of previous aspects of the code slowed down the Bluetooth too much and had to be deleted.

<iframe width="560" height="315" src="https://www.youtube.com/embed/gayrmzNoFYY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 

# Second Milestone
My second milestone setting up the accelerometer. I had to hold the accelerometer in several different orientations and record the X, Y, and Z data for each orientation. That way, I could write code in the Arduino Micro to recognize a new orientation and assign a number to this orientation to be sent to my Arduino Uno on the robot. This part took a lot of trial and error since the accelerometer was very sensitive, but I soon learned that the trick was to make the ranges of X, Y, and Z coordinates for each position much greater than what you had recorded.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4yhy7YYJi4c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
# First Milestone
  

My first milestone was assembling the frame of the robot by screwing in the DC motors and attaching the wheels. I then wired the motors and battery pack to the Arduino Uno using the H-Bridge. When that was all wired, I assigned the outputs for all of the pins that my motors were using in Arduino code. I wrote a simple motor testing code by assigning all of the pins to a HIGH output to see if the motors would run. Once I was confident that everything was wired properly, I created different functions to go forward, backward, right, and left. I controlled these functions by typing WASD onto my keyboard into the Arduino Serial Monitor.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/plT0T65Whm8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
