# Raspberry-PI-Extention-Board

This expansion card adds a 4G/3G/2G modem to the popular Raspberry Pi mini PC and makes it possible to communicate wirelessly via the cellular network. The expansion card is simply plugged into the GPIOs of the Raspberry Pi and you can instantly communicate wirelessly with your device anywhere in the world and send and receive data in real-time. 

Datasheet: http://www.roundsolutions.com/media/pdf/EXT-RPI-HAUPT_Datasheet_RaspberryPi_Extension_EN_v1.06.pdf

## Installation

Plug Raspberry Pi extension card on top the 40 Pina connector of the Raspberry Pi. Now your extension card will be powered by your Raspberry Pi and is connected via serial. Special drivers are not needed, because extension card is uses the standard tty drivers.
We recommend using a 5V power supply equal or more than 2,5A, especially for Raspberry Pi 3, to avoid problem caused by power. 
It is necessary to switch off or to reconfigure your Bluetooth port at Raspberry Pi 3, as it is using ttyAMA0. Several Forums and online tutorials are available online to show how this is done.
One possibility to switch off Bluetooth is to change config.txt with following entries:

dtoverlay=pi3-disable-bt
systemctl disable hciuart

To power on extension card the GPIO20 pin on the Raspberry Pi should be switched to high. 
One simple way to do it is by using the Python Shell. Here’s how it is done:

	import RPi.GPIO as GPIO
	import serial
	import time

	def SendATCommand(arg):
		port.write(arg)
		time.sleep(0.2)
		readbbytes = port.inWaiting()
		ATAnswer = port.read(readbbytes)
		print ATAnswer

	GPIO.setmode(GPIO.BCM)
	GPIO.setwarnings(False)
	try:
		GPIO.setup(20,GPIO.OUT)
	except:
		pass	
	GPIO.output(20, True)
	time.sleep(8) # initialisation time to turn on, before you can send AT commands
	port = serial.Serial("/dev/ttyAMA0", baudrate=115200, timeout=0)

	SendATCommand("AT\r")
	SendATCommand("AT+CMEE=2\r")
	SendATCommand("AT+CPIN?\r")
	SendATCommand("AT\r")

	print 'Script is complete'

You can check your serial port with dmesg command to find correct name of your serial port:

	$ dmesg | grep tty

Output ports for example:

	ttyAMA0
	ttyS0
	ttyUSB0
	ttyUSB1
	ttyUSB2
	ttyUSB3
	ttyUSB4

If you connect your AarLogic Raspberry PI Extension Card also with an USB cable or swivel, you can use also ttyUSB3 as connection, which is recommended if you need faster connection for 4G and 3G modules.
To send AT commands to module you can use any kind of terminal software like "minicom"

How to start: http://www.roundsolutions.com/media/pdf/EXT-RPI-HAUPT_AarLogic%20Raspberry%20PI%20extension%20card%20ManualRev%2006.pdf

## More Features

Furthermore, an optional GNSS module can be integrated for monitoring and positioning with the Raspberry Pi: you can choose between GPS, Glonass and Galileo. The expansion has a SIM card holder and is ready for immediate use. If necessary, you can add the following extra options:
-Integrated Sim Card
-IoT Cloud

Find out more: http://www.roundsolutions.com/en/raspberry-pi-cellular-adapter/

More free downloads available at: http://www.roundsolutions.com/en/products/internet-of-things-devices/all-iot-devices/10644/aarlogic-raspberry-pi-extension-board-cellular-4gnb1m13g2ggps 
