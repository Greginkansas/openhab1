# Lg TV Binding

## Working models:
* Every LG TV Model with Netcast 3.0 and Netcast 4.0 (Model years 2012 & 2013)

## Binding ConfigurationĀ¶

```
#First of all you need to introduce your LGTVs in the openhab.cfg file (in the folder '${openhab_home}/configurations'). 
#--------- LgTV  Binding --------------
# Host of the first lgtv device to control 
#lgtv:<lgtvId1>.host=
# Port of the lgtv to control (optional, defaults to 60128)
#lgtv:<lgtvId1>.port=
# Host of the second lgtv device to control 
#lgtv:<lgtvId2>.host=
# Port of the lgtv to control (optional, defaults to 60128)
#lgtv:<lgtvId2>.port=

#The lgtv:<lgtvId1>.host value is the ip address of the lgtv . 
#The lgtv:<lgtvId1>.port value is TCP port address. 
#The lgtv:<lgtvId1>.serverport is TCP port address of the openhab system to receive lgtv status messages (only first occurance is used for all tvs) usually 8080
#The lgtv:<lgtvId1>.xmldatafiles location to put xml files with the information of availiable channels and apps (optional)
#The lgtv:<lgtvId1>.checkalive check if tv is availiable every 60secs
#The lgtv:<lgtvid1>.pairkey - the pairkey / if it's wrong the device shows the right pairkey at every connection attempt 
```


### Example, how to configure your lgtv device: 

```
lgtv:wohnzimmer.host=192.168.77.15
lgtv:wohnzimmer.port=8080
lgtv:wohnzimmer.pairkey=
lgtv:wohnzimmer.serverport=8080
lgtv:wohnzimmer.xmldatafiles=./
lgtv:wohnzimmer.checkalive=60
```



### Item Binding ConfigurationĀ¶

In order to bind an item to the device, you need to provide configuration settings. The easiest way to do so is to add some binding information in your item file (in the folder configurations/items`). The syntax of the binding configuration strings accepted is the following: 
lgtv="<openHAB-command>:<device-id>:<device-command>[,<openHAB-command>:<device-id>:<device-command>][,...]"
where parts in brackets signify an optional information. 

The openHAB-command corresponds OpenHAB command. 

The device-id corresponds device which is introduced in openhab.cfg. 

The device-command corresponds lgtv commands. See complete list below. 

### Examples, how to configure your items: 

```
Switch LgTvPower                        "Power Command"                 (GF_Living)     {lgtv="ON:wohnzimmer:POWER"}
Switch LgTvMute                         "Mute"                          (GF_Living)     {lgtv="ON:wohnzimmer:KEY_MUTE, OFF:wohnzimmer:KEY_MUTE"}
Number LgTvVolume                       "lgVolume [%.1f]"               (GF_Living)     {lgtv="INIT:wohnzimmer:VOLUME_CURRENT, INCREASE:wohnzimmer:VOLUME_UP, DECREASE:wohnzimmer:VOLUME_DOWN, *:wohnzimmer:VOLUME_SET"}
Number LgTvChannel                      "Channel [%.1f]"                (GF_Living)     {lgtv="INIT:wohnzimmer:CHANNEL_CURRENTNUMBER, INCREASE:wohnzimmer:CHANNEL_UP, DECREASE:wohnzimmer:CHANNEL_DOWN, *:wohnzimmer:CHANNEL_SET" }
String LgTvChannelName                  "Channelname [%s]"              (GF_Living)     {lgtv="INIT:wohnzimmer:CHANNEL_CURRENTNAME"}
String LgTvGetChannels                  "getchannels [%s]"              (GF_Living)     {lgtv="INIT:wohnzimmer:GET_CHANNELS, ON:wohnzimmer:GET_CHANNELS"}
String LgTvGetApps                      "getapps [%s]"                  (GF_Living)     {lgtv="INIT:wohnzimmer:GET_APPS, ON:wohnzimmer:GET_APPS"}
String LgTvAppExecute                   "excuteapp"                     (GF_Living)     {lgtv="*:wohnzimmer:APP_EXECUTE"}
String LgTvAppTerminate                 "terminateapp"                  (GF_Living)     {lgtv="*:wohnzimmer:APP_TERMINATE"}
Switch LgTvConnStatus                   "connstatus "                   (GF_Living)     {lgtv="*:wohnzimmer:CONNECTION_STATUS"} 
```

### Sitemap Example:

```
   Frame label="Tv"
                        {
                                Switch item=LgTvConnStatus
                                Switch item=LgTvPower mappings=[ON="Power Off"]
                                Setpoint item=LgTvChannel minValue=0 maxValue=300 step=1.0
                                Setpoint item=LgTvVolume minValue=0 maxValue=100 step=1.0
                                Switch item=LgTvMute mappings=[ON="Mute", OFF="Mute"]
                                Text item=LgTvChannelName
                                Selection item=LgTvChannel mappings=[1="ORF1",2="ORF2"]
                                Switch item=LgTvAppExecute label="AppExecute" mappings=[Skype=Skype, Spotify=Spotify]
                                Switch item=LgTvAppTerminate label="AppTerminate" mappings=[Skype=Skype, Spotify=Spotify]
                        }
```



### Predefined Commands:

Command	Description
- POWER	POWER
- N0	Number 0
- N1	Number 1
- N2	Number 2
- N3	Number 3
- N4	Number 4
- N5	Number 5
- N6	Number 6
- N7	Number 7
- N8	Number 8
- N9	Number 9
- KEY_UP	UP key among remote Controllerās 4 direction keys
- KEY_DOWN	DOWN key among remote Controllerās 4 direction keys
- KEY_LEFT	LEFT key among remote Controllerās 4 direction keys
- KEY_RIGHT	RIGHT key among remote Controllerās 4 direction keys
- KEY_OK	OK
- KEY_HOME	Home menu
- KEY_MENU	Menu key (same with Home menu key)
- KEY_BACK	Previous key (Back)
- VOLUME_UP	Volume up
- VOLUME_DOWN	Volume down
- KEY_MUTE	Mute (toggle)
- CHANNEL_UP	Channel UP (+)
- CHANNEL_DOWN	Channel DOWN (-)
- KEY_BLUE	Blue key of data broadcast
- KEY_GREEN	Green key of data broadcast
- KEY_RED	Red key of data broadcast
- KEY_YELLOW	Yellow key of data broadcast
- KEY_PLAY	Play
- KEY_PAUSE	Pause
- KEY_STOP	Stop
- KEY_FF	Fast forward (FF)
- KEY_REW	Rewind (REW)
- KEY_SF	Skip Forward
- KEY_SB	Skip Backward
- KEY_RECORD	Record
- KEY_RECORDLIST	Recording list
- KEY_REPEAT	Repeat
- KEY_LIVETV	Live TV
- KEY_EPG	EPG
- KEY_CURRENTPROG	Current program information
- KEY_ASPECT	Aspect ratio
- KEY_EXTERNALINPUT	External input
- KEY_PIP	PIP secondary video
- KEY_SUBTITLE	Show / Change subtitle
- KEY_PROGRAMLIST	Program list
- KEY_TELETEXT	Tele Text
- KEY_MARK	Mark
- KEY_3DVIDEO	3D Video
- KEY_3DLR	3D L/R
- KEY_DASH	Dash (-)
- KEY_PREVCHANNEL	Previous channel (Flash back)
- KEY_FAVORITE	Favorite channel
- KEY_QUICKMENU	Quick menu
- KEY_TEXTOPTION	Text Option
- KEY_AUDIODESCR	Audio Description
- KEY_NETCAST	NetCast key (same with Home menu)
- KEY_ENGERGYSAVE	Energy saving
- KEY_AVMODE	A/V mode
- KEY_SIMPLINK	SIMPLINK
- KEY_EXIT	Exit
- KEY_RESERVAT	Reservation programs list
- PIP_CHANNEL_UP	PIP channel UP
- PIP_CHANNEL_DOWN	PIP channel DOWN
- KEY_SWITCHPSEC	Switching between primary/secondary video
- KEY_MYAPPS	My Apps
- REQUESTPAIRKEY	MF: Request Pair Key
- SENDPAIRKEY	MF: Send Pair Key
- CHANNEL_SET	MF: Set Channel
- VOLUME_CURRENT	MF: Get Current Volume
- VOLUME_ISMUTED	MF: Get Is Muted
- CHANNEL_CURRENTNUMBER	MF: Get Current Channel Number
- CHANNEL_CURRENTNAME	MF: Get Current Channel Name
- CHANNEL_CURRENTPROG	MF: Get Current Program Name
- GET_CHANNELS	MF: Get all Channels
- GET_APPS	MF: Get all Apps
- APP_EXECUTE	MF: Execute Application
- APP_TERMINATE	MF: Terminate Application
- CONNECTION_STATUS	MF: Get Binding Connection Status 

For those who try to POWER ON - LgTV shuts down the network port so only power off is availiable ;-(

## BETA FUNCTION: Usage of LgTv as Openhab "screen"

The initial idea behind the binding was the following usecase: In the morning (office days) a start of the tv should automatically start a browser showing latest news, traffic information and the status of the openhab sites (eg heating, water and power consumtion of the house ...) - controlled via an openhab rule. 

As the binding is able to start an internet browser on the tv a functionality to automaticly change websites in the screen browser is embedded. 

the functionality is realized via a small embedded webserver in the binding which shows a minimalistic (not visible) website (a frame). this website shows the given url described in an item and refreshes it if the configured item changes the value to a new website. 

to automatically enable the feature you should set the bindings location as default page in the browser - the location is: http://<ipadressofopenhab>:<portofopenhab>/udap/api/event?devicename=<deviceid>. 
example: http://opnehabserver:8080/udap/api/event?devicename=wohnzimmer

try the sample by creating the following items in lgtv.items in the appropriate folder

###extend the item file eg. lgtv.items
```
String LgTvAppExecute                   "excuteapp"                     (GF_Living)     {lgtv="*:wohnzimmer:APP_EXECUTE"}
String LgTvBrowserRemote                "wohnzimmer url"                (GF_Living)     {lgtv="*:wohnzimmer:BROWSER_URL"}
Number LgTvInfoMode                     "infomode [%.1f]"               (GF_Living)
```

Within a rules file you can then open a browser and set a location 

```
LgTvAppExecute.sendCommand("Internet")
postUpdate(LgTvBrowserRemote,"http://www.profil.at")
```

With the following example rule you can automate this process and changes websites based on a timeshedule. 

###create a rules file lginfoscreen.rules
```
import org.openhab.core.library.types.*
import java.lang.Math

var Number lgruninsec=0
var String site=""
var Number currentpage=0

rule "infosystemstart"
when
 System started
then
        println ("infosystemstart")
        lgruninsec = 0
        currentpage = 0
end



rule "infostart"
when
        Item LgTvInfoMode received update
then

        if (LgTvInfoMode.state!=0)
        {
                if (lgruninsec==0)
                {
                        LgTvAppExecute.sendCommand("Internet")
                        lgruninsec = lgruninsec+1
                        currentpage=1
                }
                currentpage=LgTvInfoMode.state
                if (currentpage==1) site="http://www.google.com"
                if (currentpage==2) site="http://www.diepresse.at"
                if (currentpage==3) site="http://www.spiegel.de"
                if (currentpage==4) site="http://openhabserver:8080/openhab.app?sitemap=demo"
                postUpdate(LgTvBrowserRemote,site)
        }else
        {
                LgTvAppTerminate.sendCommand("Internet")
                lgruninsec=0
        }
end
rule "infolaeuft"
when
        Time cron "0,15,30,45 * * * * ?"
then
        var int s1=0
        var int s2=0

        if (LgTvInfoMode.state>0)
        {
                lgruninsec = lgruninsec + 1
                currentpage=LgTvInfoMode.state
                if (lgruninsec > 4)
                {
                        currentpage = currentpage+1
                        lgruninsec=1
                }
                logInfo("lginfomode","infolauft: lgruninsec="+lgruninsec+" currentpage="+currentpage)
                if (currentpage>4) currentpage=1
                postUpdate(LgTvInfoMode,currentpage)
        }
end
```