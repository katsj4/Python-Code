
/* Components required

    ESP32 card
    L298N module
    hc-06 bluetooth module
    power supply module
    connecting wires

    car chassis

    two wheels

    two motors

    roulette

    9V battery
    une roulette
    une batterie de 9V

Car assembly:

    Connect pin N °5 of the ESP32 board to pin IN1 of the L298N module.

    Connect pin N ° 4 of the ESP32 board to pin IN2 of the L298N module.

    Connect pin No. 23 of the ESP32 board to pin IN3 of the L298N module.

    Connect pin N ° 22 of the ESP32 board to pin IN4 of the L298N module.

    Connect the GND pin of the ESP32 board to the GND pin of the L298N module.

    Connect the VIN pin of the ESP32 board to the (+) terminal of the power supply module

    Connect the GND pin of the ESP32 board to the (-) terminal of the power supply module

    Connect the 12V pin of the L298N module to the (+) terminal of the power supply module

    Connect the TX pin of the HC-06 module to the RX pin of the ESP32

    Connect the RX pin of the HC-06 module to the TX pin of the ESP32

    Connect the VCC pin of the HC-06 module to the (+) terminal of the power supply module

    Connect the GND pin of the HC-06 module to the (-) terminal of the power supply module */

# This is the program that allows the computer to connect and send data to the HC-06 bluetooth module.
import serial
import time
from pynput import keyboard

evenement=""
print("Start")
port="COM20" #This will be different for various devices and on windows it will probably be a COM port.
bluetooth=serial.Serial(port, 9600)#Start communications with the bluetooth unit
print("Connected")
bluetooth.flushInput() #This gives the bluetooth a little kick
result = str(0)

def on_key_release(key):
    global result
    #print('released %s' % key)
    #result ='released %s' % key+'\n'
    if result !='stop': #or result == str(2)+'\n' or result == str(3)+'\n' or result == str(4)+'\n':
       result = 'stop'
       print(result)
       result_bytes = result.encode('utf_8')
       bluetooth.write(result_bytes)

def on_key_pressed(key):
    global result       
    #print('pressed %s' % key)
    result1 ='%s' % key
    if result == 'stop':
        if result1=='Key.up' :
           result = 'forward'
           print(result)
        if result1=='Key.down' :
           result = 'backwards'
           print(result)
        if result1=='Key.left' :
           result = 'left'
           print(result)
        if result1=='Key.right' :
           result = 'right'
           print(result)         
        result_bytes = result.encode('utf_8')
        bluetooth.write(result_bytes)

with keyboard.Listener(on_release = on_key_release,on_press=on_key_pressed) as listener:
    listener.join()
