io7uLux01
=====
<img width="665" height="243" alt="Screenshot 2025-10-01 at 8 42 28 PM" src="https://github.com/user-attachments/assets/b3a795d5-fb84-46ff-bd34-8ea8b22d5a3d" />


This is a sample nodered flow project with the io7 IOT Platform. 

It takes the lux information from an io7 device and builds a dashboard. The following is the io7 device with io7fupython library.

```python
from IO7FuPython import ConfiguredDevice
import json
import time
import uComMgr32
from machine import Pin, ADC

lux = ADC(Pin(1))
lux.atten(ADC.ATTN_11DB)

def handleCommand(topic, msg):
    global lastPub
    jo = json.loads(str(msg,'utf8'))
    if ("lux" in jo['d']):
        if jo['d']['lux'] is 'current':
            lastPub = - device.meta['pubInterval']

nic = uComMgr32.startWiFi('io7lux')
device = ConfiguredDevice()
device.setUserCommand(handleCommand)

device.connect()

lastPub = time.ticks_ms() - device.meta['pubInterval']

while True:
    # default is JSON format with QoS 0
    if not device.loop():
        break
    if (time.ticks_ms() - device.meta['pubInterval']) > lastPub:
        lastPub = time.ticks_ms()
        device.publishEvent('status', json.dumps({'d':{'lux': lux.read()}}))

```
### Breadboard

<img width="526" alt="Screenshot 2024-04-08 at 6 32 14 PM" src="https://github.com/iotlab101/io7lux01/assets/13171662/645113b6-1e1f-4a2b-88a8-56f071b8e94c">
