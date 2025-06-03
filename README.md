# AIR-QUALITY-MONITOR-USING-RASPBERRYPI2
import RPi.GPIO as GPIO
import time

# Setup GPIO pins
SENSOR_PIN = 17  # MQ-3 DOUT
BUZZER_PIN = 18  # Buzzer
GPIO.setmode(GPIO.BCM)
GPIO.setup(SENSOR_PIN, GPIO.IN)  # Input for sensor
GPIO.setup(BUZZER_PIN, GPIO.OUT)  # Output for buzzer

try:
    while True:
        # Read digital output from MQ-3
        if GPIO.input(SENSOR_PIN) == GPIO.LOW:
            # DOUT LOW means high CO2 (threshold exceeded)
            GPIO.output(BUZZER_PIN, GPIO.HIGH)
            print("Alert: High CO2 levels detected!")
        else:
            # DOUT HIGH means normal CO2
            GPIO.output(BUZZER_PIN, GPIO.LOW)
            print("CO2 levels normal")

        time.sleep(1)  # Check every second

except KeyboardInterrupt:
    print("Program stopped")
finally:
    GPIO.cleanup()
