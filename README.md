# node-red-pool-panel-flow
This is a flow for integration with Pool Panel App, to control smart Pool Heating Pumps, using Node-RED and MQTT, to be used in your home automation platform (Ex. HA/OpenHAB)

### Configuration
Update line 36 with you own credentials:

 ``"to": "email={USER}%40{DOMAIN}&password={PASSWORD}",``

Replace:
- {USER} = Ex. if your account is ``user@gmail.com`` in the Pool Panel app, so ``user`` goes here.
- {DOMAIN} = Ex. if your account is ``user@gmail.com`` in the Pool Panel app, so ``gmail.com`` goes here.
- {PASSWORD} = Your actual password used in the Pool Panel app

Update line 1277 and 1278 with your MQTT server:

``"broker": "{MQTT-Server-Address}",``
``"port": "{MQTT-Server-Port}",``

Replace:
- {MQTT-Server-Address} = IP or hostname/dns of your MQTT Server.
- {MQTT-Server-Port} = 1883 or port number of your MQTT Server.

### Usage
The flow is configured to resolve authentication every 24 hours or when the token needs to be renewed, and get all status automatically every 60 secs.

Status information is published to MQTT on ``tele/pool-panel/info`` topic, and the message is a JSON with the following properties (Names are case sensitive):

| Property Name | Values | Description |
| ------------- | ------ | ----------- |
| POWER | ON,OFF | Power Status of the heat pump |
| MODE | 1 = Cool, 2 = Heat, 4 = Auto | Operation mode | 
| HEATTEMP | 16-34 | Target temperature selected for heat mode |
| AUTOTEMP | 16-34 | Target temperature selected for auto mode |
| COOLTEMP | 16-34 | Target temperature selected for cool mode |
| FUNCTION | 1 = Boost, 2 = Silence, 4 = Smart | Function selection |
| BOOST | 1 = On, 0 = Off | Boost function (Enabled/Disabled) |
| SILENCE | 1 = On, 0 = Off | Silence function (Enabled/Disabled) |
| COMPRESOR | 0-100 | Compressor operation power level |
| AMBIENTTEMP | -10-50 | Actual ambience temperature |
| INLETTEMP | -10-40 | Actual temperature of water comming from pool to heat pump |
| OUTPUTTEMP | -10-40 | Temperature exiting from heat pump to pool |
| MALFUNTION | Text | Error information from heat pump controller | 

Commands are subscribed on MQTT with the topic ``cmnd/pool-panel/{PROPERTY}`` in text format, using the following commands (names are case sensitive):

| Property name | Values | Descripction |
| ------------- | ------ | ------------ |
| POWER | ON or OFF | Turn On or Off |
| function | 1 or 2 or 4 | Function selection, 1 = Boost, 2 = Silence, 4 = Smart |
| mode | 1 or 2 or 4 | Mode selection, 1 = Cool, 2 = Heat, 4 = Auto |
| heattemp | 16-34 | Set target temperature for Heat mode |
| cooltemp | 16-34 | Set target temperature for Cool mode |
| autotemp | 16-34 | Set target temperature for Auto mode |




