# Made for Let's Robot
# By Monty C

from Adafruit_MotorHAT import Adafruit_MotorHAT, Adafruit_StepperMotor, Adafruit_DCMotor
import time
import atexit
import RPi.GPIO as GPIO
import threading


# Wrapper for step(), so stepping will be easier to manage when multitasking motors
def stepperWrapper (self, stepper, numOfSteps, direction):
    stepper.step(numOfSteps, direction, Adafruit_MotorHAT.INTERLEAVE)


# Class for managing turret
class Turret (): 
	def __init__ (self):
		self.FLYWHEEL_PIN = 24
		self.FIRE_PIN = 23
		self.STEPS = 5

		# self.ammoCounter = AmmoCounter()

		self.initMotors().initBlaster()


	# Init stepper motors
	def initMotors (self):
	 	# new Motor HAT
		self.mh = Adafruit_MotorHAT(addr = 0x60)
		atexit.register(self.disableTurret)

		#create and set stepper motor objects
		self.verticalStepper = self.mh.getStepper(200, 2)
		self.verticalStepper.setSpeed(5)

		self.horizontalStepper = self.mh.getStepper(200, 1)
		self.horizontalStepper.setSpeed(5)

		return self

	# Init GPIO stuff for blaster
	def initBlaster (self):
		#pin for flywheels
		#always have flywheels on. It will be noisy, but there will be no delay when firing since we dont need to keep toggling the flywheels
		GPIO.setmode(GPIO.BCM)
		GPIO.setup(self.FLYWHEEL_PIN, GPIO.OUT)
		GPIO.output(self.FLYWHEEL_PIN, GPIO.LOW)
	    
		#pin for firing
		GPIO.setmode(GPIO.BCM)
		GPIO.setup(self.FIRE_PIN, GPIO.OUT)
		GPIO.output(self.FIRE_PIN, GPIO.HIGH)
	
		return self

	# Functions for aiming/angling/rotating blaster
	# Using threading to be able to control more than 1 motor at the same time
	def rotateUp (self):
		print "rotating up!"

		# rotateUp_Thread = threading.Thread(target = stepperWrapper, args = (self.verticalStepper, self.STEPS, Adafruit_MotorHAT.FORWARD))
		# rotateUp_Thread.start()
		
		self.verticalStepper.step(self.STEPS, Adafruit_MotorHAT.FORWARD, Adafruit_MotorHAT.INTERLEAVE)
		return self

	def rotateDown (self):
		print "rotating down!"

		# rotateDown_Thread = threading.Thread(target = stepperWrapper, args = (self.verticalStepper, self.STEPS, Adafruit_MotorHAT.BACKWARD))
		# rotateDown_Thread.start
		
		self.verticalStepper.step(self.STEPS, Adafruit_MotorHAT.BACKWARD, Adafruit_MotorHAT.INTERLEAVE)
		return self

	def rotateRight (self):
		print "rotating right!"

		# rotateRight_Thread = threading.Thread(target = stepperWrapper, args = (self.horizontalStepper, self.STEPS, Adafruit_MotorHAT.FORWARD))
		# rotateRight_Thread.start()
		
		self.horizontalStepper.step(self.STEPS, Adafruit_MotorHAT.BACKWARD, Adafruit_MotorHAT.INTERLEAVE)
		return self

	def rotateLeft (self):
		print "rotating left!"

		# rotateLeft_Thread = threading.Thread(target = stepperWrapper, args = (self.horizontalStepper, self.STEPS, Adafruit_MotorHAT.BACKWARD))
		# rotateLeft_Thread.start()
		
		self.horizontalStepper.step(self.STEPS, Adafruit_MotorHAT.FORWARD, Adafruit_MotorHAT.INTERLEAVE)
		return self

	#auto disable all motors and relays on shutdown
	def disableTurret (self):
		self.disableStepperMotors()

	# auto-disable motors on shutdown
	def disableStepperMotors(self):
		self.mh.getMotor(1).run(Adafruit_MotorHAT.RELEASE)
		self.mh.getMotor(2).run(Adafruit_MotorHAT.RELEASE)
		self.mh.getMotor(3).run(Adafruit_MotorHAT.RELEASE)
		self.mh.getMotor(4).run(Adafruit_MotorHAT.RELEASE)
		
		return self
		
	#disable blaster. This includes shutting off relay and clearing GPIO
	def disableBlaster(self):
		GPIO.output(self.FIRE_PIN, GPIO.HIGH)
		GPIO.output(self.FLYWHEEL_PIN, GPIO.HIGH)
		
		GPIO.cleanup()
		
		return self

	def shoot(self):
		print "shooting! from nerfBlasterTurret"

		GPIO.output(self.FIRE_PIN, GPIO.LOW)
		time.sleep(.2)
		GPIO.output(self.FIRE_PIN, GPIO.HIGH)
		
		return self
