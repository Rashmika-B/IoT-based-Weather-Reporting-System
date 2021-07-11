import sys
from urllib.request import urlopen
import RPi.GPIO as GPIO
import bmpsensor
from time import sleep
import requests

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
channel=26
GPIO.setup(channel,GPIO.IN)
GPIO.setup(21,GPIO.OUT)   
GPIO.setup(21,GPIO.OUT, initial=GPIO.LOW)
pin1=23
pin2=24
GPIO.setup(pin1,GPIO.OUT, initial=GPIO.LOW)
GPIO.setup(pin2,GPIO.OUT, initial=GPIO.LOW)
myAPI='F6CJSTUTJ2S1WE9Q'
baseURL = 'https://api.thingspeak.com/update?api_key=%s' %myAPI
def bmp_data():
    temp,press,alt= bmpsensor.readBmp180()
    return temp,press,alt


def main():
    for x in range(5):
        temp,press,alt=bmp_data()
        print('WEATHER MONITORING SYSTEM - PROJECT 1')
        print('-------------------------------------------------------')
        print('-------------------------------------------------------')
        print("Temperature: %.2f C" % temp)
        print('------------------------------')
        print("Pressure:    %.2f hPa" % (press / 100.0))
        print('-------------------------------')
        if GPIO.input(channel):
            w=0;
            conn = urlopen(baseURL + '&field1=%s&field2=%s&field4=%s' % (temp,press,w))
            print('No Rain')
            print('-------------------------------')
            print(conn.read())
            conn.close()
            GPIO.output(21,0)
            sleep(1)
        else:
            w=1;
            conn = urlopen(baseURL + '&field1=%s&field2=%s&field4=%s' % (temp,press,w))
            print('Raining!')
            print('-------------------------------')
            print(conn.read())
            conn.close()
            GPIO.output(21,1)
            sleep(1)
            GPIO.output(21,0)
            sleep(1)
            GPIO.output(21,1)
        readapikey='VEGUER776SBMMPN8'
        channel_id=1369893
        baseURL1= 'https://api.thingspeak.com/channels/1369893/fields/1.json?api_key=VEGUER776SBMMPN8&results=1'
        get_data= requests.get(baseURL1).json()
        print('WEATHER MONITORING SYSTEM - ACTUATION:')
        print('-------------------------------------------------------')
        print('Average Temperature: '+get_data['feeds'][0]['field1'])
        if float(get_data['feeds'][0]['field1'])>28:
            print("It's a VERY hot day")
            GPIO.output(pin1, 1)
            GPIO.output(pin2, 0)
        else:
            print("It's a Normal day")
            GPIO.output(pin2, 1)
            GPIO.output(pin1, 0)
        baseURL2= 'https://api.thingspeak.com/channels/1369893/fields/2.json?api_key=VEGUER776SBMMPN8&results=1'
        get_data2= requests.get(baseURL2).json()
        print('-------------------------------')
        print('Average Pressure:'+get_data2['feeds'][0]['field2'])
        if float(get_data2['feeds'][0]['field2'])<970:
            print("It's a windy day")
            GPIO.output(21, 1)
        sleep(30)
        print('-------------------------------------------------------')
        print('-------------------------------------------------------')
        GPIO.output(pin1, 0)
        GPIO.output(pin2, 0)
        GPIO.output(21,0)

if __name__=="__main__":
    main()


