# Victron MPPT ve.direct to MQTT


Fork of https://github.com/KinDR007/Victron-MPPT-ve.direct-to-MQTT.

Read data from #victron mppt charger and transport to #mqtt server with esp32 

Main changes to original repository:
- platform is esp32
- using platfomio
- using https://github.com/cterwilliger/VeDirectFrameHandler from Chris Terwilliger  to read Ve.direct data
- using HardwareSerial instead of SoftwareSerial
- publish only changed key/value pairs
- publish all key/value pairs uninterpreted (this could be done later in Home Assistant or Node Red)
- reconnect on wifi lost



Home Assistant dashboard
![alt text](https://github.com/KinDR007/Victron-MPPT-ve.direct-to-MQTT/blob/master/HA.png?raw=true)


![alt text](https://github.com/KinDR007/Victron-MPPT-ve.direct-to-MQTT/blob/master/MQTTExplorerVictronToMQTT.png?raw=true)


Copy config_example.h to config.h and set params in config.h
```
const char* ssid = "SSID";
const char* password = "hidden_password_to_my_wifi";  //password to wifi
const char* mqtt_server = "192.168.0.10";             //ip to mqtt server
const char* mqtt_user = "";                           //mqtt user name
const char* mqtt_pass = "";                           //mqtt password

#define rxPin  D1                            // RX using Software Serial so we can use the hardware UART to check the ouput
#define txPin  D2                            // TX Not used

#define OTA_HOSTNAME                    "VictronMPPT"
#define MQTT_ROOT                       "Victron"
```


Home Assitant example config

```
       #. Victron
     - platform: mqtt
       name: "Victron - Battery voltage"
       state_topic: "Victron/Battery voltage, V"
       unit_of_measurement: "V"
       icon: mdi:mdi-battery
       unique_id: sensor.text.victron.battery.voltage
       
     - platform: mqtt
       name: "Victron - Battery current"
       state_topic: "Victron/Battery current, I"
       unit_of_measurement: "I"
       icon: mdi:mdi-current-dc
       unique_id: sensor.text.victron.panel.current
       
     - platform: mqtt
       name: "Victron PV power"
       state_topic: "Victron/Panel power, W"
       unit_of_measurement: "W"
       icon: mdi:gauge
       unique_id: sensor.text.victron.panel.power
       
     - platform: mqtt
       name: "Victron Panel voltage"
       state_topic: "Victron/Panel voltage"
       unit_of_measurement: "V"  
       unique_id: sensor.text.victron.panel.voltage
       
     - platform: mqtt
       name: "Victron Tracker operation"
       state_topic: "Victron/Tracker operation"
       unique_id: sensor.text.victron.tracker.operation
       icon: mdi:mdi-solar-power

     - platform: mqtt
       name: "Victron Yield today"
       state_topic: "Victron/Yield today, kWh"
       unique_id: sensor.text.victron.yield.today
       icon: mdi:gauge  
       unit_of_measurement: "Kw/h"
       
     - platform: mqtt
       name: "Victron - Maximum power today"
       state_topic: "Victron/Maximum power today, W"
       unique_id: sensor.text.victron.maximum.power.today
       unit_of_measurement: "W"
       icon: mdi:gauge       
       
     - platform: mqtt
       name: "Victron/Yield total, kWh"
       state_topic: "Victron/Yield total, kWh"
       unique_id: sensor.text.victron.yield.total
       icon: mdi:gauge  
       unit_of_measurement: "Kw/h"
       
     - platform: mqtt
       name: "Victron Charge state"
       state_topic: "Victron/Charge state"
       unique_id: sensor.text.victron.charge.state
       icon: mdi:mdi-solar-power
```

#NodeMCU pinout and wirring
![alt text](https://github.com/KinDR007/Victron-MPPT-ve.direct-to-MQTT/blob/master/nodemcu.png?raw=true)

> **Warning**
>
> Be aware of 5V vs 3V!
>
> MPPTs deliver 5V TX signals, therefore you need a voltage divider or level shipfter for MCU's RX pins.
>
> Check Victron's manuals if you are in doubt.
