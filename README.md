# Displaying different LED patterns with Raspberry Pi.



Connection:

For 4 LEDs
Connect Pin no.7 GPIO 04 to LED1 of LED Module
Connect Pin no.11 GPIO 17 to LED2 of LED Module
Connect Pin no13 GPIO 27 to LED3 of LED Module
Connect Pin no15 GPIO 22 to LED4 of LED Module
Connect Pin no 39 Ground to GND of LED Module

For 8 LEDs
Connect Pin no.7 GPIO 04 to LED1 of LED Module
Connect Pin no.11 GPIO 17 to LED2 of LED Module
Connect Pin no.13 GPIO 27 to LED3 of LED Module
Connect Pin no.15 GPIO 22 to LED4 of LED Module
Connect Pin no. 19 GPIO 10 to LED5 of LED Module
Connect Pin no. 21GPIO 09 to LED6 of LED Module
Connect Pin no.23 GPIO 11 to LED7 of LED Module
Connect Pin no. 29 GPIO 05 to LED8 of LED Module
Connect Pin no 39 Ground to GND of LED Module


Code: 

For 4 LEDs
import RPi.GPIO as io
import time
io.setmode(io.BOARD)
io.setup(7,io.OUT)
io.setup(11,io.OUT)
io.setup(13,io.OUT)
io.setup(15,io.OUT)
while(1):
io.output(7,io.HIGH)
time.sleep(0.5)
io.output(11,io.LOW)
time.sleep(0.1)
io.output(13,io.HIGH)
time.sleep(0.5)
io.output(15,io.LOW)
time.sleep(0.3)
print("LED IS ON!")
io.cleanup()

For 8 LEDs
import RPi.GPIO as io
import time
io.setmode(io.BOARD)
io.setup(7,io.OUT)
io.setup(11,io.OUT)
io.setup(13,io.OUT)
io.setup(15,io.OUT)
io.setup(19,io.OUT)
io.setup(21,io.OUT)
io.setup(23,io.OUT)
io.setup(29,io.OUT)
while(1):
io.output(7,io.HIGH)
time.sleep(0.5)
io.output(11,io.LOW)
time.sleep(0.1)
io.output(13,io.HIGH)
time.sleep(0.5)
io.output(15,io.LOW)
time.sleep(0.3)
io.output(19,io.HIGH)
time.sleep(0.5)
io.output(21,io.LOW)
time.sleep(0.3)
io.output(23,io.HIGH)
time.sleep(0.5)
io.output(29,io.LOW)
time.sleep(0.3)
print("LED IS ON!")
io.cleanup()





--------------------------------------------------------------//





# Displaying Time over 4-Digit 7-Segment Display using Raspberry Pi.

Connection:
Connect Pin no.40 of GPIO 21 to CLK
Connect Pin no.38 of GPIO 20 to DIO 
Connect Pin no.4 of DC Power 5v to VCC
Connect Ground to GND

Code:
from time import sleep
import tm1637
try:
import thread
except ImportError:
import_thread as thread
#Initialize the clock (GND,VCC=5V,D10-38pin & CLK=40 pin)
Display=tm1637.TM1637(CLK=21, D10=20,brightness=1.0)
try:
print(“Starting the clock(press Ctrl+C to stop):”)
Display.StartClock(military_time=True)
print(“Continue Display”)
Display.SetBrightness(1.0)
while(True):
Display.ShowDoublepoint(True)
sleep(1)
Display.ShowDoublepoint(False)
sleep(1)
Display.ShowDoublepoint(True)
sleep(1)
Display.StopClock()
Thread.interrupt_main()
except KeyboardInterrupt:
print(“Closing clock”)
Display.cleanup()




----------------------------------------------------------//




# Interfacing Raspberry Pi with RFID.



Connection:
Connect Card Reader +v to Red Wire of Jack(F-F)
Connect Card Reader GND to Red Wire of Jack(F-F)
Connect Card Reader TX to RXD of USB TO TTL Converter
Connect Card Reader GND to GND of USB TO TTL Converter

Connect Pin no.6 of GND to the negative terminal of LED 1 
Connect Pin no.30 of GND to the negative terminal of Buzzer
Connect Pin no.34 of GND to the negative terminal of LED 2

Connect Pin no.37 of GPIO 26 to the positive terminal of LED 1 
Connect Pin no.31 of GPIO 06 to the positive terminal of Buzzer
Connect Pin no.33 of GPIO 13 to the positive terminal of LED 2

Code:

Import RPi.GPIO as GP
import time
import serial
GP.setmode(GP.BOARD)
LED1=37
LED2=35
buzzer=33

GP.setup(LED1,GP.OUT)
GP.setup(LED2,GP.OUT)
GP.setup(buzzer,GP.OUT) 

GP.setup(LED1,False)
GP.setup(LED2,False)

GP.output(buzzer,True)
time.sleep(0.1)

GP.output(buzzer,False)
time.sleep(0.1)

GP.output(buzzer,True)
time.sleep(0.1)

GP.output(buzzer,False)

def  read_rfid():
ser=serial.Serial(“/dev/ttyUSB0”)
ser.baudrate=9600
data=ser.read(12)
ser.close()

return data
try:
while True:
id=read_rfid()
print(id)
if id == b’1E005C362551’:
print(“Access Granted”)
GP.output(LED1,True)
GP.output(LED2,False)
GP.output(buzzer,False)
time.sleep(2)
else:
print(“Access Denied”)
GP.output(LED1,False)
GP.output(LED2,True)
GP.output(buzzer,False)
time.sleep(2)
GP.output(LED1,False)
GP.output(LED2,False)
GP.output(buzzer,False)
finally:
GP.cleanup()


-------------------------------------------//


# Visitor Monitoring with Raspberry Pi and Pi Camera.

Connect Pin no.35 of GPIO 19 to positive terminal of Push Button
Connect Ground to negative terminal of Push Button
Connect Pi Camera Module to CSI Camera Port

Code: 
import RPi.GPIO as gp
import picamera
import time
m11=17
m12=25
led=5
button=19
HIGH=1
LOW=0

gp.setwarnings(False)
gp.setmode(gp.BCM)
gp.setup(led,gp.OUT)
gp.setup(m11,gp.OUT)
gp.setup(m12,gp.OUT)
gp.setup(button,gp.IN)
gp.output(led,0)
gp.output(m11,0)
gp.output(m12,0)

data=“”
def capture_image():
print(“please wait”)
data=time.strftime(“%d_%b_%Y\%H:%M:%S”)
camera.start_preview()
time.sleep(5)
print(data)
camera.capture(‘/home/pi/Desktop/Visitors/%s.jpg%data)
camera.stop_preview()
print(“image captured successfully”)
time.sleep(2)

def gate():
print(“welcome”)
gp.output(m11,1)
gp.output(m12,0)
time.sleep(1.5)
gp.output(m11,0)
gp.output(m12,0)
time.sleep(3)
gp.output(m11,0)
gp.output(m12,1)
time.sleep(1.5)
gp.output(m11,0)
gp.output(m12,0)
time.sleep(1.5)
gp.output(m11,0)
gp.output(m12,0)
print(“thank you ”)
time.sleep(2)
print(“Visitor monitoring”)
print(“using RPI”)
time.sleep(3)
camera=picamera.PiCamera()
camera.rotation=180
camera.awb_mode=‘auto’
camera.brightness=55
time.sleep(2)

while(1):
print(“PRESS BUTTON”)
gp.output(led,1)
If gp.input(button)==0
gp.output(led,0)
time.sleep(0.5)
capture_image()
gate()
time.sleep(5)
camera.rotation=180
camera.rotation=180






------------------------------------------//





# IOT based Web Controlled Home Automation using Raspberry Pi. 


sudo apt – get update
sudo apt – get upgrade
sudo reboot
Go to chrome- http://sourceforge.net/projects/webiopi/files/WebIOPi-0.7.1.tar.gz then download this file.
Go to Downloads
Then Extract the tar file
sudo apt – get update
sudo apt – get upgrade
sudo reboot
Go to chrome - http://sourceforge.net/projects/webiopi/files/WebIOPi-0.7.1.tar.gz then download this file.
Go to Downloads
Then Extract the tar file

tar xvzf WebIOPi-0.7.1.tar.gz
cd WebIOPi-0.7.1
wget https://raw.githubusercontent.com/doublebind/raspi/master/webiopi-pi2bplus.patch
patch -p1 -i webiopi-pi2bplus.patch
sudo ./setup.sh
sudo reboot
sudo webiopi -d -c/etc/webiopi/config 172.16.251.36




---------------------------------------------//



# Controlling Raspberry Pi with Telegram.


Sudo apt-get install python -pip
Sudo pip3  install telepot
Code:
import time, datetime
import telepot
from telepot.loop import MessageLoop
now = datetime.datetime.now()
def action(msg)
    chat_id = msg['chat']['id']
    command = msg['text']
	print ('Received: %s' % command)
	if '/hi' in command:
	    telegram_bot.sendMessage (chat_id, str("Hi! CircuitDigest"))
	elif '/time' in command:
	    telegram_bot.sendMessage (chat_id, str(now.hour)+str(":")+str(now.minute))
	elif '/logo' in command:
	    telegram_bot.sendPhoto (chat_id, photo = "https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2F1000logos.net%2F.jpg")
	elif '/file' in command:
	    telegram_bot.sendDocument (chat_id, document=open('/home/pi/Desktop/Prac7.prac7.py'))		
telegram_bot = telepot.Bot('5629622062:AAFn0vIE4qLgJ7K99s_qYfvmj1G8CuCsxMQ')
print (telegram_bot.getMe())
MessageLoop(telegram, action).run_as_thread()
print ('Up and Running....')
while 1:
    time.sleep(10)



---------------------------------------------//



# the end
