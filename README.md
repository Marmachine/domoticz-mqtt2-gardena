# domoticz-mqtt2-gardena
Python script to status, battery and connectivity data from Gardena using MQTT to Domoticz (or any other MQTT) 
Tested this on Gardena Sileno City, but should work on all Gardena/Husqvarna mowers

# Configuration for Gardena/Husqvarna mower data 
* [Setup Domoticz with MQTT server](https://www.domoticz.com/wiki/MQTT)
* [Install Python3 on your Domoticz server](https://www.domoticz.com/wiki/Using_Python_plugins)
  * Ensure you have Websocket and Paho packages installed (if not you'll come accross some errors when running the script!)
  * _Errors when running the script_
  * ModuleNotFoundError: No module named 'websocket'
    **Solution: sudo pip3 install websocket**
  * ModuleNotFoundError: No module named 'paho'
    **Solution: pip3 install paho-mqtt**
  * Note: Since we're running Python 3, use pip3 (not pip)
* [Create account for Gardena API and generate API Key](https://developer.husqvarnagroup.cloud/)
* Create 3 dummy devices in Domoticz and note down the IDX's:
  * Battery: Percentage (DOMOTICZ_MOWER_RFLINK_IDX)
  * Status: Text (DOMOTICZ_MOWER_STATUS_IDX)
  * Connectivity: Percentage (DOMOTICZ_MOWER_RFLINK_IDX)
* Edit the gardena.py and edit the variables listed, make sure the MQTT is pointing to the right IP (default: Localhost) and port (default: 1883). 
* Run the script:  ``` python3 gardena.py```
* Check if the dummy devices receive the correct values, 
  * if not, some hints; 
  * make sure your variables are all set correctly.
  * remove # before # websocket.enableTrace(True) and run the script again to see the request and response results to get indication of your problem

# Configuration for Gardena mower control
* [Setup Domoticz with MQTT server](https://www.domoticz.com/wiki/MQTT)
* [Install Python3 on your Domoticz server](https://www.domoticz.com/wiki/Using_Python_plugins)
* Create folder 'gardena' in ``` /home/pi/domoticz/scripts ```
* Copy ``` mower_control.sh ``` and ``` mower_control.py ```in the created folder
* Create 3 dummy devices in Domoticz:
    *   Single button: Start mowing
    *   Single button: Park until next operation
    *   Single button: Park until further notice
* Edit the button action in Domoticz with On/Off action  ```  script://gardena/mower_control.sh "INSERT ACTION KEY HERE" ``` 
    * Fill in the action key based on button (for example: ```  script://gardena/mower_control.sh "START_DONT_OVERRIDE" ```):    
        * START_SECONDS_TO_OVERRIDE - Manual operation, use 'seconds' attribute to define duration.
        * START_DONT_OVERRIDE - Automatic operation.
        * PARK_UNTIL_NEXT_TASK - Cancel the current operation and return to charging station.
        * PARK_UNTIL_FURTHER_NOTICE - Cancel the current operation, return to charging station, ignore schedule.

# Auto startup
To run the script in the background by default, install it using [PM2](https://pm2.keymetrics.io/).

