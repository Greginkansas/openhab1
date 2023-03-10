# ImperiHome Binding

**Please Note:** 
This binding is in beta stage; currently available for download via the [OpenHAB-Forum](https://groups.google.com/d/msg/openhab/TWrvCd1fens/mO83ymI772sJ).

 * [[Purpose|ImperiHome-Binding#purpose]]
 * [[Installation|ImperiHome-Binding#installation]]
 * [[Configuration|ImperiHome-Binding#configuration]]
  * [[Items|ImperiHome-Binding#items]]
    * [[Binding Format|ImperiHome-Binding#binding-format]]
    * [[Examples|ImperiHome-Binding#examples]]

## Purpose

The binding will enable you to use the comercial [ImperiHome](http://www.imperihome.com/) UI.

![](http://www.imperihome.com/wp-content/main-screens.png)

## Installation
Copy the .jar files provided into the ./addons directory of your OH installation.

[[Page Top|ImperiHome-Binding#imperihome-binding]]

## ImperiHome Configuration

After OpenHAB configuration is complete, on your ImperiHome device add a new system.  Type should be ImperiHome Standard System.  Local API base url should be of the form http://(your openhab server ip address):(openhab port)/imperihab

## OpenHAB Configuration
The binding provides the API for ImperiHome to load the devices from openHAB and to control it.

### Items
#### Binding Format
```
{imperihab="room:[#room],label:[#label],type:[#type],watts:[#wattsitem],unit:[#unit],persist:[#persist],invert:[#invert]"}
```
<table>
  <tr><td>#room</td><td>the room you want the item to appear under in ImperiHome</td></tr>
  <tr><td>#label</td><td>Optional, the name of the item to appear in ImperiHome, if not specified the name will be the item name replacing _ with " ", so Outside_Porch_Light would become "Outside Porch Light" automatically</td></tr>
  <tr><td>#type</td><td>Optional, only needed if the type of the device cannot be guessed from the item.  It first tries to find the type based on the values it support OpenClose, OnOff, Percentage etc and there's some best guesses for the item names e.g. if item name contains "Humidity" it thinks its a humidity sensor).  If none of these work, or if you want to override the type it guesses, you can specify it.  Using the device types for imperihome: 
<table>
  <tr><th>Device type string</th><th>Description</th></tr>
  <tr><td>DevCamera</td><td>MJPEG IP Camera</td></tr>
  <tr><td>DevCO2</td><td>CO2 sensor</td></tr>
  <tr><td>DevCO2Alert</td><td>CO2 Alert sensor</td></tr>
  <tr><td>DevDimmer</td><td>Dimmable light</td></tr>
  <tr><td>DevDoor</td><td>Door / window security sensor</td></tr>
  <tr><td>DevElectricity</td><td>Electricity consumption sensor</td></tr>
  <tr><td>DevFlood</td><td>Flood security sensor</td></tr>
  <tr><td>DevGenericSensor</td><td>Generic sensor (any value)</td></tr>
  <tr><td>DevHygrometry</td><td>Hygro sensor</td></tr>
  <tr><td>DevLock</td><td>Door lock</td></tr>
  <tr><td>DevLuminosity</td><td>Luminance sensor</td></tr>
  <tr><td>DevMotion</td><td>Motion security sensor</td></tr>
  <tr><td>DevMultiSwitch</td><td>Multiple choice actuator</td></tr>
  <tr><td>DevNoise</td><td>Noise sensor</td></tr>
  <tr><td>DevPressure</td><td>Pressure sensor</td></tr>
  <tr><td>DevRain</td><td>Rain sensor</td></tr>
  <tr><td>DevScene</td><td>Scene (launchable)</td></tr>
  <tr><td>DevShutter</td><td>Shutter actuator</td></tr>
  <tr><td>DevSmoke</td><td>Smoke security sensor</td></tr>
  <tr><td>DevSwitch</td><td>Standard on/off switch</td></tr>
  <tr><td>DevTemperature</td><td>Temperature sensor</td></tr>
  <tr><td>DevThermostat</td><td>Thermostat (extra parameters required)</td></tr>
  <tr><td>DevUV</td><td>UV sensor</td></tr>
  <tr><td>DevWind</td><td>Wind sensor</td></tr>
</table>
</td></tr>
  <tr><td>#wattsitem</td><td>Optional, this lets you specify another item to be the "energy" value for an item, e.g. for a z-wave power outlet Switch Item, you can link this to the Number item that has the power reading.  Then in imperihome it will show the power usage for that switch.</td></tr>
  <tr><td>#persist</td><td>Optional, this lets you specify another persist store to use instead of the default configured in openHAB.</td></tr>
  <tr><td>#unit</td><td>Optional lets you specify the units for a numeric value, for Fahrenheit temperatures use "F", eg unit:F </td></tr>
  <tr><td>#stopable</td><td>Optional, if present on a Shutter, this will show a "Stop" button for that shutter in imperiHome</td></tr>
  <tr><td>#invert</td><td>Optional (true|false), this lets you invert an on/off, open/closed state of an item.  This is helpful if ImperiHome is showing the wrong state for a door.  E.g. a garage door appears open when closed etc. </td></tr>
</table>

[[Page Top|ImperiHome-Binding#imperihome-binding]]

## Temperatures with Humidity
To combine a temperature with a humidity value (so they appear as a single item in imperiHome).  You can specify a "hygroId" to the temperature binding.  For example
```
Number	nOffice_Temperature { zwave="3:command=SENSOR_MULTILEVEL,sensor_type=1", imperihab="room:Office,label:Office Temperature,hygroId:nOffice_Humidity" }
Number	nOffice_Humidity { zwave="3:command=SENSOR_MULTILEVEL,sensor_type=5" }
```
In this case you do not specify a imperihab binding for the humidity just for the temperature.

## Thermostats

DevThermostat should be attached to the item that defines the thermostat's set point.  In addition, DevThermostat item needs several extra parameters to function.  _All of them are required._  Finally, rules to convert between the strings that Imperihab uses for the thermostat's modes and whatever command(s) you need to send to your thermostat.

<table>
  <tr><th>Name</th><th>Type</th><th>Description</th></tr>
  <tr><td>curmodeid<td>String<td>Current thermostat mode</tr>
  <tr><td>currentTempId<td>Item<td>Current temperature</tr>
  <tr><td>minVal<td>Number<td>Minimum allowable value for thermostat set point</tr>
  <tr><td>maxVal<td>Number<td>Maximum allowable value for thermostat set point</tr>
  <tr><td>availableModes<td>String<td>dash separated modes.  eg:  Off-Auto-Cool-Heat</tr>
</table>

####  DevThermostat example

```
Number LIVINGROOM_THERMOSTAT_MODE "Livingroom Thermostat Mode: [MAP(zwave_thermostat_mode.map):%s]"  {zwave="40:0:command=THERMOSTAT_MODE"}
Number LIVINGROOM_THERMOSTAT_TEMPERATURE "Livingroom Thermostat Temperature: [%.1f F]" <temperature> {zwave="40:0:command=SENSOR_MULTILEVEL,sensor_type=1"}
Number LIVINGROOM_THERMOSTAT_HEAT_SETPOINT "Livingroom Thermostat Heating Set Point [%.1f F]" <heating2_small> (Group_Thermostat, Group_Persistence)  {zwave="40:0:command=THERMOSTAT_SETPOINT,setpoint_type=1,setpoint_scale=1", imperihab="room:Livingroom,label:Thermostat (heat),unit:F,curmodeid:IMPERIHAB_THERMOSTAT_MODE,currentTempId:LIVINGROOM_THERMOSTAT_TEMPERATURE,minVal:60,maxVal:90,availableModes:Off-Heat-Cool"}
String IMPERIHAB_THERMOSTAT_MODE "Imperihab thermostat mode" 
```

####  DevThermostat example rules

```
rule "Thermostat mode changed -- tell Imperihab"
when
  Item LIVINGROOM_THERMOSTAT_MODE received command
then
	switch receivedCommand
		{
			case 0: {
				logInfo("openhab","Thermostat mode set to 0 (Off).  Informing Imperihab.")
				postUpdate(IMPERIHAB_THERMOSTAT_MODE,"Off")
					}  
			case 1: {
				logInfo("openhab","Thermostat mode set to 1 (Heat).  Informing Imperihab.")
				postUpdate(IMPERIHAB_THERMOSTAT_MODE,"Heat")
					}
			case 2: {
				logInfo("openhab","Thermostat mode set to 2 (Cool).  Informing Imperihab.")
				postUpdate(IMPERIHAB_THERMOSTAT_MODE,"Cool")
					}
		}
end

rule "Imperihab changed thermostat mode -- change thermostat"
when
  Item IMPERIHAB_THERMOSTAT_MODE received command
then
	switch receivedCommand
		{
		case "Off":
			{
				logInfo("openhab","Imperihab switched thermostat state to Off")
				sendCommand(LIVINGROOM_THERMOSTAT_MODE,0)
			}
		case "Heat":
			{
				logInfo("openhab","Iperihab switched thermostat state to Heat")				
				sendCommand(LIVINGROOM_THERMOSTAT_MODE,1)
			}
		case "Cool":
			{
				logInfo("openhab","Iperihab switched thermostat state to Cool")				
				sendCommand(LIVINGROOM_THERMOSTAT_MODE,2)
			}
		}
end

```



## Examples
```
Number zWaveSensor23_1 "L1 [%.1f W]" <energy> (gZWaveNode23, gPower) {zwave="23:1:command=METER", imperihab="room:Keller,label:Verbrauch L1,type:DevElectricity,watts:zWaveSensor23_1"}
Dimmer zWaveLightOGBedroom "Licht [%d %%]" <light> (gZWaveNode20, gLights, gHomeOGBedroom) {zwave="20:0:command=SWITCH_MULTILEVEL", imperihab="room:Schlafzimmer,label:Licht,type:DevDimmer,watts:zWaveLightOGBedroom"}
Rollershutter zWaveShutterEGLivingroomLeft "Rollladen" <rollershutter> (gZWaveNode16, gHomeShuttersEG, gHomeEGLivingRoom) {zwave="16:0:command=SWITCH_MULTILEVEL", imperihab="room:Wohnraum,label:Rollladen,type:DevShutter,watts:zWaveShutterEGLivingroomLeft"}
Number zWaveSensor2Temperatur "Temperatur EG [%.1f ??C]" <temperature> (gZWaveNode2, gTemperature, gHomeEGTV) {zwave="2:3:command=SENSOR_MULTILEVEL, sensor_type=1, sensor_scale=0", imperihab="room:TV,label:Temperatur,type:DevTemperature,watts:zWaveSensor2Temperatur"} 
Number	nOffice_Temperature { zwave="3:command=SENSOR_MULTILEVEL,sensor_type=1", imperihab="room:Office,label:Office Temperature,hygroId:nOffice_Humidity" }
Number	nOffice_Humidity { zwave="3:command=SENSOR_MULTILEVEL,sensor_type=5" }
```
[[Page Top|ImperiHome-Binding#imperihome-binding]]
