Documentation of the Frontier Silicon Radio binding for Internet Radios based on the [Frontier Silicon chipset](http://www.frontier-silicon.com/digital-radio-solutions).

## Introduction

This binding has been developed and tested with the [Hama IR110](https://de.hama.com/00054823/hama-internetradio-ir110) and [Medion MD87180](https://www.medion.com/de/service/start/_product.php?msn=50047825&gid=00) internet radios. 
The [MEDION® P85040 (MD 86988)](http://medion.scene7.com/is/image/Medion/50046868_PIC-Q?$m$), [MEDION® P85035 (MD 87090)](https://medion.scene7.com/is/image/Medion/50048568_PIC-Q?$m$), [MEDION® P85105 (MD 87505)](https://www.medion.com/de/service/start/_product.php?msn=50053142&gid=6), [Pinell Supersound II](http://www.pinell.no/en/products/pinell-supersound-ii-white//), [Silvercrest SIRD 14 C1](http://www.lidl.de/de/silvercrest-stereo-internetradio-sird-14-c1/p208310), [Silvercrest SIRD 14 C2](https://www.lidl.de/de/silvercrest-stereo-internetradio-sird-14-c2/p233545), 
[Revo Supersystem](http://revo.co.uk/shop/supersystem/), [Revo Superconnect](http://revo.co.uk/shop/superconnect/), [Ruark R2](http://www.ruarkaudio.com/products/r2-overview), [Technisat DigiRadio 450](https://www.technisat.com/en_XX/DigitRadio-450/352-10996-9589/) and the [Hama DIR3110](https://de.hama.com/00054824/hama-digitalradio-dir3110-internetradio-dab+-fm-app-steuerung) / [Hama DIT2000M](https://ch.hama.com/00054821/hama-hifi-digitaltuner-dit2000m-internetradio-dab+-fm-app-steuerung-multiroom) are also supported. No radio, but also supported: [Silvercrest SMRS 35 A1 (WLAN box)](https://www.lidl.de/de/silvercrest-wlan-lautsprecher-35-w-smrs-35-a1/p243351), and Silvercrest SMRB 40 A1 (WLAN Soundbar) 

[<img src="http://internetradio.medion.com/images/md87180_small.jpg" alt="MEDION LIFE P85044 (MD 87180)" height="160">](http://internetradio.medion.com/)
[<img src="https://de.hama.com/bilder/00054/abb/00054823abb.jpg" alt="Hama Internetradio IR110" height="180">](https://de.hama.com/00054823/hama-internetradio-ir110)
[<img src="http://internetradio.medion.com/images/md86988_small.jpg" alt="MEDION LIFE P85040 (MD 86988)" height="160">](http://internetradio.medion.com/)
[<img src="http://www.pinell.no/sitefiles/site8/shop/pinell-supersound-ii-white3.jpg" alt="Pinell Supersound II" height="100">](http://pinell.no/en/)
[<img src="https://www.lidl.de/media/product/0/2/3/3/5/4/5/silvercrest-stereo-internetradio-sird-14-c2-regular--6.jpg" alt="Silvercrest SIRD 14 C1" height="180">](http://www.lidl.de/de/dab-/-internetradios/c15246)
[<img src="https://assets25.technisat.com/assets/derivates/25/666/149/$v3/DV025_ppic_0000+4963_020300_001.jpg" alt="Technisat DigitRadio 450" height="120">](https://www.technisat.com/en_XX/DigitRadio-450/352-10996-9589/)
[<img src="http://revo.co.uk/assets/2012/11/supersys_wal_blk1-486x395.jpg" alt="Revo Supersystem" height="180">](http://revo.co.uk/shop/supersystem/) [<img src="http://revo.co.uk/assets/2013/10/H2_WNT_BLK_SUPERHERO2-486x395.jpg" alt="Revo Superconnect" height="180">](http://revo.co.uk/shop/superconnect/)

![Ruark R2](http://www.ruarkaudio.com/images/finishes-r2-walnut.jpg)

You can easily check if your IP radio is supported:

1. Figure out the *IP* of your radio (e.g. by looking it up in your router)
2. Figure out the *PIN* that is configured for the radio (somewhere hidden in the radio's on-screen menu); or try the default pin `1234`
3. Go to your web browser and enter: `http://<IP>/fsapi/CREATE_SESSION?pin=<PIN>`
4. If the response is similar to `FS_OK 6836164442`, your radio is most likely compatible with this binding
5. If you radio works with this binding, please add it to the list above by [Editing this page](Frontier-Silicon-Radio-Binding/_edit)!

For installation of the binding, please see Wiki page [[Bindings]].

The Frontier Silicon Radio binding is included since openHAB 1.7.0.


## Binding Configuration

Configuration is done in the openhab.cfg file (in the folder `${openhab_home}/configurations`):

    ############################### Frontier Silicon Radio Binding ###################################
    #
    # Hostname/IP of the radio to control
    #frontiersiliconradio:radio.host=192.168.0.100
    
    # PIN access code of the radio (default: 1234)
    #frontiersiliconradio:radio.pin=1234
    
    # Portnumber of the radio to control (optional, defaults to 80)
    #frontiersiliconradio:radio.port=80
    
    # The number of milliseconds between checks of the radio
    # (optional, defaults to 60 seconds).
    #frontiersiliconradio:refreshInterval=60000
    
    # Cache the state for n minutes so only changes are posted (optional, defaults to 0 = disabled)
    # Example: if period is 60, once per hour all states are posted to the event bus;
    #          changes are always and immediately posted to the event bus.
    # The recommended value is 60 minutes.
    #frontiersiliconradio:cachePeriod=60

Some notes:
* 'radio' is an identifier that can be replaced with your custom identifier, e.g. 'bedroom' or 'radio-kitchen'. This allows to use the binding also for multiple radios.
* The pin may vary from radio to radio, you can typically look it up in the radio settings.
* 'refreshInterval' and 'cachePeriod' are global settings that apply to all radios.


## Item Binding Configuration

There are different types of item bindings, all of them are qualified with the device's identifier used in the `openhab.cfg` file:

    frontiersiliconradio="<identifier>:<property>"

Example for the power state of a device with identifier `radio`:

    Switch RadioPower "Radio Power" { frontiersiliconradio="radio:POWER" }

A list of all properties and their possible item types is given below:

| item | item type | purpose | changeable |
| --- | --- | --- | --- |
| `POWER` | Switch | power state | yes
| `MODE` | Number | radio mode (details about mapping below) | yes
| `VOLUME` | Number | volume | yes
| `MUTE` | Swtich | mute | yes
| `PLAYINFONAME` | String | title of current playback, e.g. radio station | no
| `PLAYINFOTEXT` | String | additional information, e.g. current song | no
| `PRESET` | Number | select preset, e.g. configured radio stations | yes

The *radio mode* property is just a number that specifies the radio mode which may differ for each radio. This is why there is no fixed mapping implemented. For radios listed above, the mapping is as follows (please [add your radio mapping](Frontier-Silicon-Radio-Binding/_edit), if it differs!):

**Please use the second, transposed table below this table, it is easier to extend and maintain**
**This first table should become obsolete**

| radio mode | Hama IR110 | Medion MD 87180/87090  | Medion MD 86988| Pinell Supersound II| Silvercrest SIRD 14 C1 | Revo Superconnect | Hama DIR3110 | Hama DIT2000M | TinyAudio M7+ | Silvercrest SMRS 35 A1 (WLAN box) | Silvercrest SMRB 40 A1 (WLAN Soundbar)
| --- | --- | --- | --- | --- | --- | --- | ---| --- | --- | --- | ---
| 0 | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio | Internet Radio
| 1 | Spotify | Music Player (USB, LAN) | Music Player | Spotify | Spotify | Spotify | Spotify | Spotify | - | TIDAL | TIDAL 
| 2 | Player | DAB Radio | FM Radio | Player | --- | --- | Player | --- | Player | Napster | Napster
| 3 | AUX in | FM Radio | AUX in | DAB Radio | Music Player | Music Player | DAB Radio | Music Player | DAB Radio | Deezer | Deezer
| 4 | - | AUX in | - | FM Radio | DAB Radio | DAB Radio | FM Radio | DAB Radio | FM Radio | Qobuz | Qobuz
| 5 | - | - | - | AUX in | FM Radio | FM Radio | AUX in | FM Radio | - | Spotify | Spotify
| 6 | - | - | - | - | AUX in | AUX in | - | AUX in | - | Local Music | Local Music 
| 7 | - | - | - | - | - | Bluetooth | - | - | - | Music Player | Music Player
| 8 | - | - | - | - | - | - | - | - | - | AUX in | Bluetooth
| 9 | - | - | - | - | - | - | - | - | - | - | AUX in
| 10 | - | - | - | - | - | - | - | - | - | - | Optical in

For ease of reading and extending this table, I start a **transposed version of this table**:


| device\ radio mode | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 
| --- | --- | --- | --- | --- | --- | --- | ---| --- | --- | --- | --- | ---
| Hama IR110 | Internet Radio | Spotify | Player | AUX |  |  | | 
| Hama DIR3110 | Internet Radio | Spotify | Music Player | DAB   | FM  | AUX | 
| Hama DIT2000M | Internet Radio | Spotify | | Music Player | DAB   | FM  | AUX | 
| Medion MD 87180/97090 | Internet Radio | Music Player | DAB | FM | AUX |  | | 
| Medion MD 86988 | Internet Radio | Spotify | Player | AUX |  |  | | 
| Pinell Supersound II| Internet Radio | Spotify | Music Player | DAB   | FM  | AUX |  |
| Silvercrest SIRD 14 C1 | Internet Radio | Spotify | | Music Player | DAB   | FM  | AUX | 
| Silvercrest SIRD 14 C2 | Internet Radio | TIDAL   | Napster | Deezer | Qobuz | Spotify | Local Music | Music Player | DAB | FM | AUX | Network
| Silvercrest SMRS 35 A1 (WLAN box) | Internet Radio | TIDAL   | Napster | Deezer | Qobuz | Spotify | Local Music | Music Player | AUX | 
| Silvercrest SMRB 40 A1 (WLAN soundbar) | Internet Radio | TIDAL   | Napster | Deezer | Qobuz | Spotify | Local Music | Music Player | Bluetooth | AUX | Optical
| Revo Superconnect | Internet Radio | Spotify | | Music Player | DAB   | FM  | AUX | Bluetooth |
| TinyAudio M7+ | Internet Radio | | Music Player | DAB   | FM  |  | 

If your radio is not listed, try [http://\<IP\>/fsapi/LIST_GET_NEXT/netRemote.sys.caps.validModes/-1?pin=1234&maxItems=65536](http://\<IP\>/fsapi/LIST_GET_NEXT/netRemote.sys.caps.validModes/-1?pin=1234&maxItems=65536)



## Example

Items:

    Switch RadioPower         "Radio Power" (gRadio) { frontiersiliconradio="radio:POWER" }
    Number RadioMode          "Radio Mode [%d]" (gRadio) { frontiersiliconradio="radio:MODE" }
    Switch RadioMute          "Radio Mute" (gRadio) { frontiersiliconradio="radio:MUTE" } 
    Number RadioVolDimmer     "Radio Volume [%d %%]" (gRadio) { frontiersiliconradio="radio:VOLUME" } 
    String RadioPlayInfoName  "Play Info Name [%s]" (gRadio) { frontiersiliconradio="radio:PLAYINFONAME" }
    String RadioPlayInfoText  "Play Info Text [%s]" (gRadio) { frontiersiliconradio="radio:PLAYINFOTEXT" }
    Number RadioPreset        "Preset" (gRadio) { frontiersiliconradio="radio:PRESET" }

Sitemap (presets and mode are radio-specific, this is why their mapping is specified here):

    Frame label="Radio Control" {
        Switch     item=RadioPower
        Selection  visibility=[RadioPower==ON] item=RadioPreset mappings=[0="1Live", 1="WDR2", 2="SWR3"]
        Selection  visibility=[RadioPower==ON] item=RadioMode   mappings=[0="Internet Radio", 1="Spotify", 2="Player", 3="AUX IN"]
        Slider     visibility=[RadioPower==ON] item=RadioVolDimmer
        Switch     visibility=[RadioPower==ON] item=RadioMute
        Text       visibility=[RadioPower==ON] item=RadioPlayInfoName
        Text       visibility=[RadioPower==ON] item=RadioPlayInfoText
    }


## Developer Info

If you would like to extend this binding and contribute further functionality, here is some technical information about it. The communication with the radio is realized via simple HTTP requests. Unfortunately, there is no official documentation of the API for the frontier silicon chipset, but there are some sources around the internet ([flammy/fsapi on github](https://github.com/flammy/fsapi/blob/master/FSAPI.md), for example). Until now, only the following requests are implemented:
* netRemote.sys.power
* netRemote.sys.mode
* netRemote.sys.audio.volume
* netRemote.sys.audio.mute
* netRemote.nav.action.selectPreset
* netRemote.play.info.text
* netRemote.play.info.name

If you like to add further actions, have a look at [FrontierSiliconRadio.java](https://github.com/openhab/openhab/blob/master/bundles/binding/org.openhab.binding.frontiersiliconradio/src/main/java/org/openhab/binding/frontiersiliconradio/internal/FrontierSiliconRadio.java).