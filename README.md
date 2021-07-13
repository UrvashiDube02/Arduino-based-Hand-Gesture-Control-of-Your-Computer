# Arduino-based-Hand-Gesture-Control-of-Your-Computer

**ABSTRACT**

Human Machine Interface or HMI is a system comprising of hardware and software that helps in communication and exchange of information between the user (human operator) and the 
machine. We normally use LED Indicators, Switches, Touch Screens and LCD Displays as a part of HMI devices. Another way to communicate with machines like Robots or Computers is with the help of Hand Gestures. Instead of using a keyboard, mouse or joystick, we can use our hand gestures to control certain functions of a computer like play/pause a video, move left/right in a photo slide show, scroll up/down in a web page and many more. In this project, I have implemented a simple Arduino based hand gesture control where you 
can control few functions of your web browser like switching between tabs, scrolling up and down in web pages, shift between tasks (applications), play or pause a video and increase or decrease the volume (in VLC Player) with the help of hand gestures.

**PRINCIPLE**

The principle behind the Arduino based Hand Gesture Control of Computer is actually very simple. All you have to do is use two Ultrasonic Sensors with Arduino, place your hand in front of the Ultrasonic Sensor and calculate the distance between the hand and the sensor. Using this information, relevant actions in the computer can be performed. The position of the Ultrasonic Sensors is very important. Place the two Ultrasonic Sensors on the top of a laptop screen at either end. The distance information from Arduino is collected by a Python Program and a special library called PyAutoGUI will convert the data into keyboard click actions.

**COMPONENTS REQUIRED**

1. Arduino UNO x 1 
2. Ultrasonic Sensors x 2 
3. USB Cable (for Arduino)
4. Few Connecting Wires 
5. A Laptop with internet connection
6. Double sided tapes
 
**DESIGN**

The design of the circuit is very simple, but the setup of the components is very important. The Trigger and Echo Pins of the first Ultrasonic Sensor (that is placed on the 
left of the screen) are connected to Pins 11 and 10 of the Arduino. For the second Ultrasonic Sensor, the Trigger and Echo Pins are connected to Pins 6 and 5 of the Arduino.
Now, coming to the placement of the Sensors, place both the Ultrasonic Sensors on top of the Laptop screen, one at the left end and the other at right. You can use double sided tape to hold the sensors onto the screen. Coming to Arduino, place it on the back of the laptop screen. Connect the wires from Arduino to Trigger and Echo Pins of the individual sensors. Now, we are ready for programming the Arduino. 

**IMAGES**

![image](https://user-images.githubusercontent.com/87383888/125502918-2a1edf1f-d9c4-4727-a68c-8e34055dc31f.png)

**ARDUINO PROGRAMMING**

The important part of this project is to write a program for Arduino such that it converts the distances measured by both the sensors into the appropriate commands for controlling certain actions.The hand gestures in front of the Ultrasonic sensors can be calibrated so that they can perform five different tasks on your computer. 

Before taking a look at the gestures, let us first see the tasks that I can accomplish.

1. Switch to Next Tab in a Web Browser
2. Switch to PreviousTab in a Web Browser
3. Scroll Down in a Web Page
4. Scroll Up in a Web Page
5. Switch between two Tasks (Chrome and VLC Player)
6. Play/Pause Video in VLC Player
7. Increase Volume
8. Decrease Volume


**Arduino Code**

const int trigPin1 = 11;

const int echoPin1 = 10; 

const int trigPin2 = 6; 

const int echoPin2 = 5; 

////////////////////////////////// variables used for distance calculation 

long duration; 

int distance1, distance2; 

float r;

unsigned long temp=0; int temp1=0;

int l=0;

////////////////////////////////

void find_distance (void);WOODGROVE

BANK 13

void find_distance (void) 

{ 

digitalWrite(trigPin1, LOW); 

delayMicroseconds(2); 

digitalWrite(trigPin1, HIGH); 

delayMicroseconds(10); 

digitalWrite(trigPin1, LOW); 

duration = pulseIn(echoPin1, HIGH, 5000);

r = 3.4 * duration / 2; 

distance1 = r / 100.00;

digitalWrite(trigPin2, LOW);

delayMicroseconds(2); 

digitalWrite(trigPin2, HIGH);

delayMicroseconds(10); 

digitalWrite(trigPin2, LOW);

duration = pulseIn(echoPin2, HIGH, 5000);

r = 3.4 * duration / 2; 

distance2 = r / 100.00; 

delay(100); }WOODGROVE

BANK 14

void setup() 

{ 

Serial.begin(9600); 

pinMode(trigPin1, OUTPUT);

pinMode(echoPin1, INPUT);

pinMode(trigPin2, OUTPUT); 

pinMode(echoPin2, INPUT); 

delay (1000); }

void loop()

{ 

find_distance();

if(distance2<=35 && distance2>=15) {

temp=millis();

while(millis()<=(temp+300)) 

find_distance();

if(distance2<=35 && distance2>=15) {WOODGROVE

BANK 15

temp=distance2;

while(distance2<=50 || distance2==0) {

find_distance();

if((temp+6)<distance2) {

Serial.println("down"); }

else if((temp-6)>distance2)

{ Serial.println("up");

} }}

else }

Serial.println("next"); }}WOODGROVE

BANK 16

else if(distance1<=35 && distance1>=15) {

temp=millis();

while(millis()<=(temp+300)) {

find_distance();

if(distance2<=35 && distance2>=15) {

Serial.println("change"); 

l=1;

break; }}

if(l==0) 

{ Serial.println("previous");

while(distance1<=35 && distance1>=15)

find_distance(); }

l=0; }}

**HAND GESTURES**

1. Gesture 1: Place your hand in front of the Right Ultrasonic Sensor at a distance (between 15CM to 35CM) for a small duration and move your hand away from the sensor. This gesture will Scroll Down the Web Page or Decrease the Volume.

2. Gesture 2: Place your hand in front of the Right Ultrasonic Sensor at a distance (between 15CM to 35CM) for a small duration and move your hand towards the sensor. This gesture will Scroll up the Web Page or Increase the Volume.

3. Gesture 3: Swipe your hand in front of the Right Ultrasonic Sensor. This gesture will move to the Next Tab.

4. Gesture 4: Swipe your hand in front of the Left Ultrasonic Sensor. This gesture will move to the Previous Tab or Play/Pause the Video.

5. Gesture 5: Swipe your hand across both the sensors (Left Sensor first). This action will Switch 
between Tasks.

**PYTHON PROGRAMMING**

Writing Python Program for Arduino based Hand Gesture Control is very simple. You just need to read the Serial data from Arduino and invoke certain keyboard key presses. In order to achieve this, you have to install a special Python Module called PyAutoGUI.

**Installing PyAutoGUI**

• The following steps will guide you through the installation of PyAutoGUI on Windows Computers. The module PyAutoGUI will help you to programmatically control the mouse and keyboard.
• With the help of PyAutoGUI, I can write a Python Program to mimic the actions of mouse like left click, right click, scroll, etc. and keyboard like keypress, enter text, multiple key press, etc. without physically doing them. Let us install PyAutoGUI.
• If you remember in the previous project, where I controlled an LED on Arduino using Python, I have installed Python in the directory “C:\Python27”. 
• Open Command Prompt with Administrator privileges and change to the directory where you have installed Python (in my case, it is C:\Python27). 
• If you have installed the latest version of Python, then pip (a tool for installing packages in Python) will already be installed. To check if pip is installed or not, type   pip-V.
• You should upgrade to the latest package of pip using the following command. If pip is already in its latest version, then ignore this step.Type python -m pip install -U pip
• After upgrading pip, you can proceed to install PyAutoGUI. In order to install PyAutoGUI, type python -m pip install pyautogui.

**PYTHON CODE**

If everything goes well till now, you can proceed to write the Python Code. If you observe the Arduino Code given above, the Arduino sends out five different texts or commands through Serial Port upon detecting appropriate hand gestures. These commands are
1. Next
2. Previous
3. Down
4. Up
5. Change

Using these commands along with few functions in PyAutoGUI (like hotkey, scroll, keyDown, press and keyUp), you can write a simple Python Code that will execute the following tasks of keyboard and mouse.
1. Data = “next” – – > Action = Ctrl+PgDn
2. Data = “previous” – – > Action = Ctrl+PgUp
3. Data = “down” – – > Action = Down Arrow
4. Data = “up” – – > Action = Up Arrow
5. Data = “change” – – > Action = Alt+Tab


**Python Code**

import serial 

Import pyautogui 

Arduino_Serial = serial.Serial('com12',9600) 

while 1:

incoming_data = str (Arduino_Serial.readline()) 

print incoming_data 

if 'next' in incoming_data: 

pyautogui.hotkey('ctrl', 'pgdn')

if 'previous' in incoming_data: 

pyautogui.hotkey('ctrl', 'pgup') 

if 'down' in incoming_data: 

pyautogui.press('down') 

pyautogui.scroll(-100) WOODGROVE

BANK 21

if 'up' in incoming_data:

#pyautogui.press('up') 

pyautogui.scroll(100) 

if 'change' in incoming_data: 

pyautogui.keyDown('alt')

pyautogui.press('tab') 

pyautogui.keyUp('alt') 

incoming_data = "";

**APPLICATIONS**

In this project, we have implemented Arduino based Hand Gesture Control of Your Computer, where few hand gestures made in front of the computer will perform certain tasks in the computer without using mouse or keyboard. 

This type of hand gesture control of computers can be used for –
1. VR (Virtual Reality),
2. AR (Augmented Reality)
3. 3D Design
4. Reading Sign Language, etc

**OUTPUT OF THE PROJECT**

• The link to the video of the project is provided below:

https://youtu.be/jzDRe90CCWU

• The video shows the following functions:
1. Scroll up and down
2. Switch to next page 
3. Play 
4. Volume increase decrease
