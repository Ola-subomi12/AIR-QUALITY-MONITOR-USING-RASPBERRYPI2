# AIR-QUALITY-MONITOR-USING-RASPBERRYPI2
import time
import Adafruit_DHT
import spidev

# Set up SPI connection for air quality sensor
spi = spidev.SpiDev()
spi.open(0, 0)
spi.max_speed_hz = 1200000

def read_air_quality():
    # Read data from air quality sensor
    spi.xfer([0x01])
    bytes = spi.readbytes(2)
    value = (bytes[0] << 8) | bytes[1]
    return value

def calculate_aqi(value):
    # Calculate Air Quality Index (AQI) based on sensor value
    if value <= 50:
        return "Good"
    elif value <= 100:
        return "Moderate"
    elif value <= 150:
        return "Unhealthy for sensitive groups"
    elif value <= 200:
        return "Unhealthy"
    elif value <= 300:
        return "Very unhealthy"
    else:
        return "Hazardous"

try:
    while True:
        # Read air quality data
        value = read_air_quality()
        aqi = calculate_aqi(value)
        
        # Print air quality data
        print(f"Air Quality Index (AQI): {aqi} ({value})")
        
        # Wait for 1 minute before taking the next reading
        time.sleep(60)
except KeyboardInterrupt:
    spi.close()
