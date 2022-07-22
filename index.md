
 <img style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://raw.githubusercontent.com/bradyott/BradyBSEPortfolio/gh-pages/IMG_1456.jpg" width="305" height="318">
 

# Gesture Controlled Robot Car

Users can control all movements of this robot simply by turning their hand. The robot utilizes an accelerometer and two Bluetooth HC-05 modules to connect the Arduino Micro on the hand controller to the Arduino Uno on the robot itself. Furthermore, an Ultrasonic sensor and LCD screen connect to the hand controller to create an interactive game based on how far away the robot is from a given object. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Brady O. | Council Rock North | Mechanical Engineering | Incoming Junior

<img style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://raw.githubusercontent.com/bradyott/BradyBSEPortfolio/main/IMG_1278.jpg" width="900" height="650">

# Modifications
I added a couple of modifications to my robot since I was done with the project early and had ideas for some improvements. The first major modification that I made was to make the hand controller more portable by wiring a 9V battery to the Arduino Micro. This way, I could stop relying on the computer as my source of power and instead take the hand controller and robot anywhere. The second modification that I made was an interactive game using an Ultrasonic sensor and an LCD screen. First, I coded the LCD screen to print a randomly generated number between 1 and 15, called the objective. Then, I wired a button to the hand controller to send a signal to the Ultrasonic sensor. The player's goal was to press the button when they thought they were close to the randomly generated objective. If they were within 2.5 centimeters, they would be awarded a point. I enjoyed this part of the project the most since I was able to express my creativity and make something completely original. 

<p align="center">
<iframe width="1000" height="500" src="https://www.youtube.com/embed/bhcBi9ve2_k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

# Demo Night
<p align="center">
<iframe width="1000" height="500" src="https://www.youtube.com/embed/fabll6Bzw_Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

# Final Milestone
My final milestone was the connection of both Arduinos via Bluetooth HC-05 modules. The first step was wiring the modules to their respective Arduinos. This was unlike the simple wiring of the accelerometer, since the modules required the help of resistors to split the current from 5V to 3.3V. Once that was complete, I had to set each HC-05 module into AT Command modeand then pair them by using the Serial Monitor to obtain their Bluetooth addresses. This was definitely the hardest part of the project, since I had absolutely no experience. Furthermore, the addition of the Bluetooth required me to modify the code for the Arduino Uno, since some of previous aspects of the code slowed down the Bluetooth too much and had to be deleted.

<p align="center">
<iframe width="1000" height="500" src="https://www.youtube.com/embed/gayrmzNoFYY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
</p>

# Second Milestone

My second milestone was the implementation of the accelerometer. After successfully wiring the accelerometer the the Arduino Micro, I was back in Arduino code trying to make sense of the huge amounts of coordinate data that the accelerometer was producing. I had to hold the accelerometer in several different orientations and record the X, Y, and Z data for each orientation. That way, I could write code in the Arduino Micro to recognize a new orientation and assign a number to it. Once I got the Bluetooth HC-05 modules paired, I could then send that number to the Arduino Uno to call a certain function, either forward, backward, left, or right. This part took a lot of trial and error since the accelerometer was very sensitive, but I soon learned that the trick was to make the ranges of X, Y, and Z coordinates for each position much greater than what you had recorded. This part was also especially meticulous because it involved large amounts of data collection, and it made me realize that I enjoy the coding and assembly of the robot much more than the collection of the data.

<p align="center">
<iframe width="1000" height="500" src="https://www.youtube.com/embed/4yhy7YYJi4c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

# First Milestone
  

My first milestone was the completion of my robot's driving mechanism. I assembled the frame of the robot with its DC motors, battery pack, and wheels, and then I went on to wire the motors through the H-Bridge to the Arduino Uno. When all of the assembly was done, I plugged my Arduino Uno into my computer and started coding. I only had coding experience in C Sharp prior to this, but Arduino wasn't much different and I quickly learned how the outputs, inputs, serial moniter, and pin system worked. Finally, I created functions to move the motors either forward, backward, left, or right and I controlled these functions by typing WASD onto my keyboard via the Arduino Serial Monitor. This part of the project reaffirmed my love for coding and reminded me of how satisfying it can be to get code running properly. 

<p align="center">
<iframe width="1000" height="500" src="https://www.youtube.com/embed/plT0T65Whm8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>
# Schematics
<img style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://cdn.discordapp.com/attachments/991733151166627870/998973852631711905/gesture_bb.png" width="900" height="650">
# Final Code
<pre>
<font color="#95a5a6">&#47;*This is the code for the Arduino Micro, which manages the</font>
<font color="#95a5a6"> * accelerometer and the LCD screen. In this code, the Micro sends </font>
<font color="#95a5a6"> * information about the position of the accelerometer to the Arduino </font>
<font color="#95a5a6"> * Uno via Bluetooth. It also recieves information about the Ultrasonic</font>
<font color="#95a5a6"> * Sensor from the Arduino Uno and prints it using the LCD screen.</font>
<font color="#95a5a6"> *&#47;</font>

<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><font color="#d35400">Wire</font><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>
<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><font color="#000000">MPU6050</font><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>
<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><b><font color="#d35400">LiquidCrystal_I2C</font></b><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>
<b><font color="#d35400">LiquidCrystal_I2C</font></b> <font color="#000000">lcd</font><font color="#000000">(</font><font color="#000000">0x27</font><font color="#434f54">,</font><font color="#000000">20</font><font color="#434f54">,</font><font color="#000000">4</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">MPU6050</font> <font color="#000000">mpu</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">250</font><font color="#000000">;</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in1</font> <font color="#434f54">=</font> <font color="#000000">8</font><font color="#000000">;</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in2</font> <font color="#434f54">=</font> <font color="#000000">9</font><font color="#000000">;</font>
<font color="#00979c">String</font> <font color="#000000">distance</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;&#34;</font><font color="#000000">;</font>
<font color="#00979c">float</font> <font color="#000000">d</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">score</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
<font color="#00979c">long</font> <font color="#000000">randomnumber</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">yesno</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">previoustate</font> <font color="#434f54">=</font> <font color="#000000">1</font><font color="#000000">;</font>
<font color="#00979c">void</font> <font color="#5e6d03">setup</font><font color="#000000">(</font><font color="#000000">)</font> 
<font color="#000000">{</font>
 &nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">38400</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">38400</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#5e6d03">while</font><font color="#000000">(</font><font color="#434f54">!</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">MPU6050_SCALE_2000DPS</font><font color="#434f54">,</font> <font color="#000000">MPU6050_RANGE_2G</font><font color="#000000">)</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;Could not find a valid MPU6050 sensor, check wiring!&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">delay</font><font color="#000000">(</font><font color="#000000">500</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;
 &nbsp;<font color="#000000">checkSettings</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">init</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 &nbsp;
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">backlight</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">INPUT_PULLUP</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">INPUT_PULLUP</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#00979c">String</font> <font color="#000000">starting</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;Press button on&#34;</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">starting</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#00979c">String</font> <font color="#000000">start</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;the right!&#34;</font><font color="#000000">;</font>
<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">start</font><font color="#000000">)</font><font color="#000000">;</font>

 &nbsp;
<font color="#000000">}</font>


<font color="#00979c">void</font> <font color="#000000">checkSettings</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; * Sleep Mode: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getSleepEnabled</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#434f54">?</font> <font color="#005c5f">&#34;Enabled&#34;</font> <font color="#434f54">:</font> <font color="#005c5f">&#34;Disabled&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; * Clock Source: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">switch</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getClockSource</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_KEEP_RESET</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;Stops the clock and keeps the timing generator in reset&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_EXTERNAL_19MHZ</font><font color="#434f54">:</font> <b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;PLL with external 19.2MHz reference&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_EXTERNAL_32KHZ</font><font color="#434f54">:</font> <b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;PLL with external 32.768kHz reference&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_PLL_ZGYRO</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;PLL with Z axis gyroscope reference&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_PLL_YGYRO</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;PLL with Y axis gyroscope reference&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_PLL_XGYRO</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;PLL with X axis gyroscope reference&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_CLOCK_INTERNAL_8MHZ</font><font color="#434f54">:</font> &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;Internal 8MHz oscillator&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; * Accelerometer: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">switch</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getRange</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_RANGE_16G</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;+&#47;- 16 g&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_RANGE_8G</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;+&#47;- 8 g&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_RANGE_4G</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;+&#47;- 4 g&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#000000">MPU6050_RANGE_2G</font><font color="#434f54">:</font> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#005c5f">&#34;+&#47;- 2 g&#34;</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font> &nbsp;

 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; * Accelerometer offsets: &#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getAccelOffsetX</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; &#47; &#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getAccelOffsetY</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34; &#47; &#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">getAccelOffsetZ</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">println</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#5e6d03">while</font><font color="#000000">(</font><font color="#000000">yesno</font> <font color="#434f54">!=</font> <font color="#000000">1</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#d35400">digitalRead</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#000000">)</font> <font color="#434f54">==</font> <font color="#00979c">LOW</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;<font color="#000000">randomnumber</font> <font color="#434f54">=</font> <font color="#d35400">random</font><font color="#000000">(</font><font color="#000000">15</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;&nbsp;<font color="#00979c">String</font> <font color="#000000">wor</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;Objective:&#34;</font> <font color="#434f54">+</font> <font color="#00979c">String</font><font color="#000000">(</font><font color="#000000">randomnumber</font><font color="#000000">)</font> <font color="#434f54">+</font> <font color="#005c5f">&#34;cm&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">wor</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#000000">Vector</font> <font color="#000000">rawAccel</font> <font color="#434f54">=</font> <font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">readRawAccel</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">Vector</font> <font color="#000000">normAccel</font> <font color="#434f54">=</font> <font color="#000000">mpu</font><font color="#434f54">.</font><font color="#000000">readNormalizeAccel</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">delay</font><font color="#000000">(</font><font color="#000000">10</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">2.75</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">2.85</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&gt;</font> <font color="#000000">6.0</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">11.5</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;5&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;5&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">150</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">2.75</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&gt;</font> <font color="#000000">6.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">12.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">3.1</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">5.30</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;2&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;2&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">150</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font><font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">2.75</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&lt;</font><font color="#434f54">-</font><font color="#000000">3.6</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">10.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">2.1</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">4.3</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;3&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;3&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">150</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font><font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&gt;</font> <font color="#000000">2.7</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">13.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&gt;</font><font color="#434f54">-</font><font color="#000000">5.85</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">5.9</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.3</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;4&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;4&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">150</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font><font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&lt;</font> <font color="#434f54">-</font><font color="#000000">2.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">XAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">12.5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&gt;</font><font color="#434f54">-</font><font color="#000000">4.85</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">YAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">5</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&lt;</font> <font color="#000000">4.9</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">normAccel</font><font color="#434f54">.</font><font color="#000000">ZAxis</font> <font color="#434f54">&gt;</font> <font color="#434f54">-</font><font color="#000000">5.35</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;1&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">&#34;1&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">150</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

 &nbsp;
<font color="#00979c">int</font> <font color="#000000">state</font> <font color="#434f54">=</font> <font color="#d35400">digitalRead</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">state</font> <font color="#434f54">==</font> <font color="#000000">0</font> <font color="#434f54">&amp;&amp;</font> <font color="#000000">previoustate</font> <font color="#434f54">==</font> <font color="#000000">1</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">write</font><font color="#000000">(</font><font color="#005c5f">&#34;6&#34;</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">delay</font><font color="#000000">(</font><font color="#000000">10</font><font color="#000000">)</font><font color="#000000">;</font>

<font color="#000000">}</font>
<font color="#000000">previoustate</font> <font color="#434f54">=</font> <font color="#000000">state</font><font color="#000000">;</font>

<font color="#5e6d03">if</font><font color="#000000">(</font><b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">available</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#00979c">char</font> <font color="#000000">n</font> <font color="#434f54">=</font> <b><font color="#d35400">Serial1</font></b><font color="#434f54">.</font><font color="#d35400">read</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#d35400">isDigit</font><font color="#000000">(</font><font color="#000000">n</font><font color="#000000">)</font><font color="#434f54">||</font> <font color="#000000">n</font> <font color="#434f54">==</font> <font color="#00979c">&#39;.&#39;</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">distance</font> <font color="#434f54">+=</font> <font color="#000000">n</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">n</font> <font color="#434f54">==</font> <font color="#00979c">&#39;*&#39;</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">d</font> <font color="#434f54">=</font> <font color="#000000">distance</font><font color="#434f54">.</font><font color="#000000">toFloat</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">distance</font> <font color="#434f54">=</font> <font color="#005c5f">&#34; &#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">printLCD</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

 &nbsp;
<font color="#000000">}</font>

<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">score</font> <font color="#434f54">&gt;=</font> <font color="#000000">5</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;
 &nbsp;&nbsp;<font color="#00979c">String</font> <font color="#000000">ending</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;You Win!&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">ending</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#00979c">String</font> <font color="#000000">endingg</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;Reset to play&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">endingg</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#d35400">delay</font><font color="#000000">(</font><font color="#000000">9000</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#00979c">int</font> <font color="#000000">yesno</font> <font color="#434f54">=</font> <font color="#000000">1</font><font color="#000000">;</font> &nbsp;
<font color="#000000">}</font>
 &nbsp;
 &nbsp;<font color="#000000">}</font>
<font color="#000000">}</font>



<font color="#00979c">void</font> <font color="#000000">printLCD</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;
 &nbsp;&nbsp;<font color="#00979c">String</font> <font color="#000000">ds</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;Distance:&#34;</font> <font color="#434f54">+</font> <font color="#00979c">String</font><font color="#000000">(</font><font color="#000000">d</font><font color="#434f54">,</font><font color="#000000">2</font><font color="#000000">)</font> <font color="#434f54">+</font> <font color="#005c5f">&#34;cm&#34;</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">ds</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#00979c">float</font> <font color="#000000">num</font> <font color="#434f54">=</font> <font color="#d35400">abs</font><font color="#000000">(</font><font color="#000000">d</font> <font color="#434f54">-</font> <font color="#000000">randomnumber</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">num</font> <font color="#434f54">&lt;=</font> <font color="#000000">3.5</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">score</font><font color="#434f54">++</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#00979c">String</font> <font color="#000000">sco</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;Your score:&#34;</font> <font color="#434f54">+</font> <font color="#00979c">String</font><font color="#000000">(</font><font color="#000000">score</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">sco</font><font color="#000000">)</font><font color="#000000">;</font>

 &nbsp;
<font color="#000000">}</font>


</pre>

<pre>
<font color="#95a5a6">&#47;*This is the code for the Arduino Uno, which recieves data from the </font>
<font color="#95a5a6"> * Arduino Micro and relays it via the H Bridge to the DC Motors. It also </font>
<font color="#95a5a6"> * recieves data from the Ultrasonic Sensor and then forwards that data</font>
<font color="#95a5a6"> * via Bluetooth the the Arduino Micro. This Uno is located on top of the robot.</font>
<font color="#95a5a6"> * </font>
<font color="#95a5a6"> *&#47;</font>

<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><b><font color="#d35400">SoftwareSerial</font></b><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>

<font color="#5e6d03">#define</font> <font color="#000000">tx</font> <font color="#000000">2</font>
<font color="#5e6d03">#define</font> <font color="#000000">rx</font> <font color="#000000">3</font>

<b><font color="#d35400">SoftwareSerial</font></b> <font color="#000000">configBt</font><font color="#000000">(</font><font color="#000000">rx</font><font color="#434f54">,</font> <font color="#000000">tx</font><font color="#000000">)</font><font color="#000000">;</font>

<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in1</font> <font color="#434f54">=</font> <font color="#000000">12</font><font color="#000000">;</font> 
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in2</font> <font color="#434f54">=</font> <font color="#000000">11</font><font color="#000000">;</font> &nbsp;
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in3</font> <font color="#434f54">=</font> <font color="#000000">4</font><font color="#000000">;</font> &nbsp;
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in4</font> <font color="#434f54">=</font> <font color="#000000">5</font><font color="#000000">;</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in5</font> <font color="#434f54">=</font> <font color="#000000">9</font><font color="#000000">;</font> <font color="#434f54">&#47;&#47; this is enable A</font>
<font color="#00979c">const</font> <font color="#00979c">int</font> <font color="#000000">in6</font> <font color="#434f54">=</font> <font color="#000000">10</font><font color="#000000">;</font> <font color="#434f54">&#47;&#47; this is enable B</font>
<font color="#00979c">int</font> <font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">250</font><font color="#000000">;</font>
<font color="#00979c">char</font> <font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39; &#39;</font><font color="#000000">;</font>
<font color="#5e6d03">#define</font> <font color="#000000">echoPin</font> <font color="#000000">7</font>
<font color="#5e6d03">#define</font> <font color="#000000">trigPin</font> <font color="#000000">6</font>
<font color="#00979c">long</font> <font color="#000000">duration</font><font color="#000000">;</font>
<font color="#00979c">float</font> <font color="#000000">distance</font><font color="#000000">;</font>
<font color="#00979c">char</font> <font color="#000000">state</font><font color="#000000">;</font>
<font color="#00979c">void</font> <font color="#5e6d03">setup</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;<font color="#434f54">&#47;&#47; put your setup code here, to run once:</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">trigPin</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">echoPin</font><font color="#434f54">,</font> <font color="#00979c">INPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">38400</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">configBt</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">38400</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">tx</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">rx</font><font color="#434f54">,</font> <font color="#00979c">INPUT</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#000000">{</font>


<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">configBt</font><font color="#434f54">.</font><font color="#d35400">available</font><font color="#000000">(</font><font color="#000000">)</font><font color="#434f54">&gt;</font><font color="#000000">0</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#00979c">char</font> <font color="#000000">a</font> <font color="#434f54">=</font> <font color="#000000">(</font><font color="#00979c">char</font><font color="#000000">)</font><font color="#000000">configBt</font><font color="#434f54">.</font><font color="#d35400">read</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">a</font> <font color="#434f54">!=</font> <font color="#000000">state</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">a</font> <font color="#434f54">==</font> <font color="#00979c">&#39;6&#39;</font><font color="#000000">)</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#00979c">String</font> <font color="#000000">d</font> <font color="#434f54">=</font><font color="#00979c">String</font><font color="#000000">(</font> <font color="#000000">getDistance</font><font color="#000000">(</font><font color="#000000">)</font><font color="#434f54">,</font><font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">configBt</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">d</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">configBt</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#00979c">&#39;*&#39;</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#000000">a</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">c</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#000000">}</font>
<font color="#5e6d03">switch</font> <font color="#000000">(</font><font color="#000000">c</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;1&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;<font color="#000000">forward</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;4&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">backward</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;2&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">right</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;3&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">left</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;o&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">speedup</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;p&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">speeddown</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;6&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">case</font> <font color="#00979c">&#39;h&#39;</font><font color="#434f54">:</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">break</font><font color="#000000">;</font> &nbsp;&nbsp;
 &nbsp;<font color="#5e6d03">default</font><font color="#434f54">:</font>
 &nbsp;<font color="#000000">brake</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;
<font color="#000000">}</font>

 
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#000000">forward</font> <font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>

 
<font color="#00979c">int</font> <font color="#000000">variable</font> <font color="#434f54">=</font> <font color="#000000">s</font><font color="#000000">;</font>
<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">100</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>

<font color="#5e6d03">while</font> <font color="#000000">(</font><font color="#000000">s</font> <font color="#434f54">&lt;</font> <font color="#000000">variable</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">speedup</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">state</font> <font color="#434f54">=</font> <font color="#00979c">&#39;1&#39;</font><font color="#000000">;</font>
<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">variable</font><font color="#000000">;</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#000000">backward</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>

<font color="#00979c">int</font> <font color="#000000">variable</font> <font color="#434f54">=</font> <font color="#000000">s</font><font color="#000000">;</font>
<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">100</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font> 

<font color="#5e6d03">while</font> <font color="#000000">(</font><font color="#000000">s</font> <font color="#434f54">&lt;</font> <font color="#000000">variable</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">speedup</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">variable</font><font color="#000000">;</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">state</font> <font color="#434f54">=</font> <font color="#00979c">&#39;4&#39;</font><font color="#000000">;</font>
<font color="#000000">}</font>


<font color="#00979c">void</font> <font color="#000000">right</font> <font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font> 
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">state</font> <font color="#434f54">=</font> <font color="#00979c">&#39;2&#39;</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#000000">left</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font> 
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">state</font> <font color="#434f54">=</font> <font color="#00979c">&#39;3&#39;</font><font color="#000000">;</font>


<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#000000">brake</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font> <font color="#00979c">int</font> <font color="#000000">previous</font> <font color="#434f54">=</font> <font color="#000000">s</font><font color="#000000">;</font>
<font color="#5e6d03">while</font> <font color="#000000">(</font><font color="#000000">s</font> <font color="#434f54">&gt;</font><font color="#000000">0</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#000000">speeddown</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in2</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in4</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in3</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">in1</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>

<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">previous</font><font color="#000000">;</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">state</font> <font color="#434f54">=</font> <font color="#00979c">&#39;5&#39;</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#000000">speedup</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">s</font> <font color="#434f54">==</font> <font color="#000000">250</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">250</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">+=</font> <font color="#000000">10</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
<font color="#000000">c</font> <font color="#434f54">=</font> <font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
<font color="#000000">}</font>
<font color="#00979c">void</font> <font color="#000000">speeddown</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in6</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">analogWrite</font><font color="#000000">(</font><font color="#000000">in5</font><font color="#434f54">,</font> <font color="#000000">s</font><font color="#000000">)</font><font color="#000000">;</font> 
<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">s</font> <font color="#434f54">==</font> <font color="#000000">0</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">s</font> <font color="#434f54">-=</font> <font color="#000000">10</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#000000">c</font><font color="#434f54">=</font><font color="#00979c">&#39;h&#39;</font><font color="#000000">;</font>
 &nbsp;
<font color="#000000">}</font>
<font color="#00979c">float</font> <font color="#000000">getDistance</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 <font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">trigPin</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">delayMicroseconds</font><font color="#000000">(</font><font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font> 
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">trigPin</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">delayMicroseconds</font><font color="#000000">(</font><font color="#000000">10</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">trigPin</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">duration</font> <font color="#434f54">=</font> <font color="#d35400">pulseIn</font><font color="#000000">(</font><font color="#000000">echoPin</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">distance</font> <font color="#434f54">=</font> <font color="#000000">duration</font> <font color="#434f54">*</font> <font color="#000000">0.034</font> <font color="#434f54">&#47;</font> <font color="#000000">2</font><font color="#000000">;</font>
<font color="#5e6d03">return</font> <font color="#000000">distance</font><font color="#000000">;</font>
<font color="#000000">}</font>


</pre>

# Bill of Materials

| **Part** | **Quantity** | **Description** | **Reference Designator** | **Cost** |
|:--:|:--:|:--:|:--:|:--:|
| Robot Kit | 1 | Chassis, Battery Pack, DC Motors, Wheels, etc. | M1, M2 | $19.99 |
| Bluetooth Module | 2 | Serial Communication | HC-05 | $19.98 |
| Accelerometer	 | 1 | Measure Linear Acceleration Along Axis | ADXL345 | $13.59 |
| Arduino Uno	 | 1 | Microcontroller Board | R3 | $23.00 |
| Arduino Micro	 | 1 | Microcontroller Board | R3 | $20.70 |
| Arduino Uno	 | 1 | Microcontroller Board | R3 | $23.00 |
| Jumper Wires| 1 | Connection Wires| N/A | $6.98 |
| Dual H Bridge	| 1 | Control Direction of DC Motors| L298N | $6.99 |
| Breadboard	| 1 | Build Circuits Without Soldering | N/A| $11.98 |
| Resistors	| 1 | Control Flow of Electrical Current | N/A| $10.99 |
| Liquid Crystal Display	| 1 | Display Characters | IIC/I2C/TWI | $7.59 |
| Ultrasonic Sensor	| 1 | Detect Distance | HC-SR04 | $1.80 |
| Soldering Iron	| 1 | Connects Wires | N/A | $15.99 |
| Screwdriver Kit	| 1 | Tightens Screws | N/A | $6.99 |

