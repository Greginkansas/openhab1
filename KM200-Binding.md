Documentation of the KM200 binding bundle
**Note:** This Binding will be available starting with version 1.9.0 of the OH1 bindings.

## Introduction

For installation of the binding, please see Wiki page [[Bindings]], or you can add [this JAR](https://openhab.ci.cloudbees.com/job/openHAB1-Addons/lastSuccessfulBuild/artifact/bundles/binding/org.openhab.binding.km200/target/org.openhab.binding.km200-1.9.0-SNAPSHOT.jar) to your `addons` folder.

The KM200 Binding is communicating with a [Buderus Logamatic web KM200 / KM100 / KM50](https://www.buderus.de/de/produkte/catalogue/alle-produkte/7719_gateway-logamatic-web-km200-km100-km50).
It is possible to recive and send parameters like string or float values.

**Important**: If the communication is not working and you see in the logfile errors like "illegal key size" then you have to change the [Java Cryptography Extension to the Unlimited Strength Jurisdiction](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html). 
<br><br>
For Linux Systems:<br>
cd $JAVA_HOME/jre/lib/security<br>
Check if these two files local_policy.jar and US_export_policy.jar already exsist. If so, delete them and extract the ones from the zip file to this folder.<br>
Also, if this doesn't work, please try restart openhab service. 
<br><br>
Adapt your openhab.cfg to your configuration:
* IP address of KN200 to connect to
<BR>`
    km200:ip4_address=192.168.XXX.XXX
`
* There a two ways of de-/encryption password handling:

1.  With the finished private key, here is this one required:
<BR>`
    km200:PrivKey=0000FFFFEEEEDDDDCCCCBBBBAAAA999988887777666655554444333322221111
`
<br><br>
Note:<br>
You could generate this key here f.e. https://ssl-account.com/km200.andreashahn.info/
<br>Geräte-Passwort = gateway password <br>
Benutzer-Passwort = user password (the one you set the first time you connected with the EasyControl App to the box)
<br><br>
2.  --OR-- the binding is creating the key from the md5salt, the device and the private password. Here are all three required:
<BR>`
    km200:MD5Salt=111122223333444455556666777788889999aaaabbbbccccddddeeeeffff0000
`<BR>`
    km200:GatewayPassword=AAAABBBBCCCCDDDD
`<BR>`
    km200:PrivatePassword=MYPASSWORD
`

## Generic Item Binding Configuration

In order to bind an item to the KM200 device, you need to provide configuration settings. The easiest way to do so is to add some binding information in your item file (in the folder `configurations/items`). 
There are two different ways to configure the items.
### Direct access wir defined key:

    DateTime  budDate	"Buderus Date Time[%1$tA, %1$td.%1$tm.%1$tY]" {km200="date_time"}
    DateTime  budDate	"Buderus Datum  [%1$td.%1$tm.%1$tY %1$tH:%1$tM]" {km200="date_time"}
    String  budBrand "Brand of the heater [%s]" {km200="sys_brand"}
    String  budType "Type of the heater [%s]" {km200="sys_type"}
    String  budStatus "State of the heater [%s]" {km200="sys_state"}
    String  budFirmware "Firmwareversion [%s]" {km200="ver_firmware"}
    String  budHardware  "Hardwareversion [%s]" {km200="ver_hardware"}

### List of avalible services:

The second way is the definition of user-defined items with direct path to the services. Here you need to know which kind of services is you device supporting. This information you will get from this binding. <BR>

* Set the right configuration to the openhab.cfg file.
* Now you have to start openhab and take a look to the openhab log file. (/var/log/openhub/openhab.log)
* Here you will see after 1-2 Minutes after start a table with all supported services and capabilities. It is looking like this:

List of availible services:

    readable;writeable;recordable;virtual;type;service;value;allowed;min;max
    1;0;1;0;floatValue;/heatSources/nominalDHWPower;15.0;;;
    1;1;1;0;floatValue;/dhwCircuits/dhw1/setTemperature;60.0;;30.0;80.0
    1;0;0;0;switchProgram;/heatingCircuits/hc1/switchPrograms/Nachmittag;;;
    1;0;1;0;floatValue;/heatingCircuits/hc2/pumpModulation;0.0;;0.0;100.0
    1;0;0;0;stringValue;/heatingCircuits/hc4/status;INACTIVE;INACTIVE|ACTIVE;;
    .....

You can copy this table in excel. It is ';' seperated.<BR>
The colums are: <BR>

| Property     |  Description     |
| ------------- |  --------------- |
| readable | Service is readable, if not then you cannot use it |
| writable | It is possible to set values |
| recordable | It is possible to create recordings. Not directly supported yet |
| virtual | This service is only virtual and not existing of the device. The binding is translating the message to the parent |
| type | FloatValue, stringValue, switchProgram (not supported), refEnum ( not relevant), yRecording (not supported) |
| service | This is the path for the configuration |
| value | Value of the service in the time of the init |
| allowed | If existing then only this values are possible |
| min | The min value for a float |
| max | The max value for a float |

You can select this services in the config with the syntax: service:<fullPathToService>  

## Switching Programs

The binding is now supporting the reading and changing of switching programs.
The communication between the binding and the user is done over virtual services. In the service list are now virtual services included. Every switch program service has now five virtual subservices. They are: <BR>

| Subservice|  Description     |
| ------------- |  --------------- |
| weekday | Selection of a weekday, possible: Mo/Tu/We/Th/Fr/Sa/Su |
| nbrCycles | The number of cycles (on+off or day+night, depends on the parent service) on the selected day |
| cycle | Selection of a cycle on the selected day |
| on/day | The selected switch time for the on/day type of the selected cycle |
| off/night | The selected switch time for the off/night type of the selected cycle |

### Add a new switch cycle
With the last commit it's possible to add a switch cycle to the end of the cycle list. 
* Set a weekday. 
* Set the cycle to the one after the number of actual cycles.
* Set the time for the new on/day type, it will by automaticly moved to the first possible time.
* Set the time for the new off/night type, it will by automaticly moved to the first possible time.

### Remove a switch cycle
With the last commit it's possible to remove a switch cycle from the end of the cycle list. 
* Set a weekday. 
* Set the cycle to the last one.
* Set the time for the off/night type to max time (1430 Minutes).
* Set the time for the on/day type to max time (1430 Minutes).
* Some seconds later it will be removed.

##Error Lists
The binding is supporting the parsing of the device error list. This list is usally readable from the /notification service. This has now new subservices

| Subservice|  Description     |
| ------------- |  --------------- |
| nbrErrors | The number of errors |
| error| Selection of the error for generation of the errorString 
| errorString | String with all informations about this error

## Dynamic service path (Parameter replacement)
In some cases the path to the service not static. It depends on something else like another service. One example for this are switching programs. It is nice to see switch points parameters from the current selected program.
This binding is supporting parameter replacements. It meens that's possible to replace a part of the service path with a values from another service. 

| Parameter|  Replacement|
| ------------- |  --------------- |
| `__current__` | `current:<path>`|

## Examples

    String  budState "State of the heating [%s]"  {km200="service:/system/healthStatus"}
    Number	budTemp  "Temperature heating night [%.1f °C]" {km200="service:/heatingCircuits/hc3/temperatureLevels/night"}

For switches you can define which of the allowed values is the one for 'on' and 'off'.

`Switch  budMode  "Mode [%s]" {km200="service:/heatingCircuits/hc3/operationMode on:auto off:night"}`

Switching programs: (you have to select as first the day, second the cycle and then read/write on/day or off/night).

    String actBudDayHC1 "Day HC1 [%s]" {km200="service:/heatingCircuits/hc1/switchPrograms/Eigen1/weekday" }
    Number nbrBudNbrCyclesHC1 "Cycles HC1 [%d]" {km200="service:/heatingCircuits/hc1/switchPrograms/Eigen1/nbrCycles" }
    Number actBudCycleHC1 "Selected cycle HC1 [%d]" {km200="service:/heatingCircuits/hc1/switchPrograms/Eigen1/cycle" }
    Number actBudPosHC1 "Day  HC1  [%d]" {km200="service:/heatingCircuits/hc1/switchPrograms/Eigen1/day" }
    Number actBudNegHC1"Night HC1  [%d]" {km200="service:/heatingCircuits/hc1/switchPrograms/Eigen1/night" }

    Number nbrBudNbrCyclesHCT1Curr "Current HC1 [%d]" {km200="service:/heatingCircuits/hc1/switchPrograms/__current__/nbrCycles current:/heatingCircuits/hc1/actualSwitchingProgram" 

###Example files (German):

Items:<BR>
[Heizung.items](http://www.markinus.de/Heizung.items)<BR>
Cometvisu:<BR>
[visu_config.xml](http://www.markinus.de/visu_config.xml)<BR>

![KM200 Binding with OpenHAB](http://www.markinus.de/Openhab_Buderus.PNG)<BR>

![KM200 Binding with CometVisu](http://www.markinus.de/Cometvisu_Buderus.PNG)<BR>


## Supported item types 

| Item Type|  KM200 service type|
| ------------- |  --------------- |
| Number |  For string, float and switching program (cycle, nbrCycles, on/day, off/night) | 
| String |  For string, float and switching program (weekday)) | 
| DateTime | For string and switching program (on/day, off/night) |
| Switch | For string |


This binding is automaticly blocking the values to the allowed and limiting them to the min and max capabilities.

