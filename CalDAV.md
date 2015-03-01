This binding can be used to connect through the CalDAV Interface to calendars.

First of all you need to add the org.openhab.io.caldav-version.jar to the addons folder.

openhab.cfg
* `caldavio:<calendar-id>:url=`
* `caldavio:<calendar-id>:username=`
* `caldavio:<calendar-id>:password=`
* `caldavio:<calendar-id>:reloadInterval=<minutes>`
* `caldavio:<calendar-id>:preloadTime=<minutes>`

***

# CalDAV Command
Binding file: org.openhab.binding.caldav-version.jar

Used to execute commands through an event, triggered at the start or the end of an event.
Syntax is `<BEGIN|END>:<Item-Name>:<Command>`.

Additionaly you can define an item to listen to upcoming changes of an item (which will be triggered through an event). Two types are available the command which will be set and the trigger time.
Syntax is `caldavCommand="itemName:<Item-Name to listen to> type:<VALUE|DATE>"`
Furthermore a switch can be defined to disable the automatic execution (through calendar) of an item. 
Syntax is `caldavCommand="itemName:<Item-Name to listen to> type:<DISABLE>"`

openhab.cfg
`caldavCommand:readCalendars=<calendar-id>` (multiple calendars can be seperated by commas)

## Description of type
* VALUE: the value which will send to the command (can be of any type, depends on command in event and accepted commands of item)
* DATE: the time on which the event occurs (item type: DateTime)
* DISABLE: can turn off the automatic execution of the given item (item type: Switch)

***

# CalDAV Personal
Binding file: org.openhab.binding.caldav-presence-version.jar

* Used to detect presence through calendar events.
* Used to show upcoming/active events in openhab.

### openhab.cfg
* `caldavPersonal:usedCalendars=<calendar-id>` (multiple calendars can be seperated by commas)
* `caldavPersonal:homeIdentifiers=<values seperated by commans>` (if one of these identifiers can be found inside the place of the event, this event will not be used for presence)

### items
* `caldavPersonal="calendar:<calendar-id> type:<UPCOMING|ACTIVE|EVENT> eventNr:<event-nr, first one is 1> value:<NAME|DESCRIPTION|PLACE|START|END|TIME>"`
* `caldavPersonal="calendar:<calendar-id> type:PRESENCE" (type must be Switch)`

## Description of type
* UPCOMING: the next upcoming events, not the active ones
* ACTIVE: events which are currently on (internally used for presence detection)
* EVENT: all events, active as well as upcoming

## Description of value
* NAME: Name of the event (itemtype: String)
* DESCRIPTION: Event content (itemtype: String)
* PLACE: Place of event (itemtype: String)
* START: start time (itemtype: DateTime)
* END: end time (itemtype: DateTime)
* TIME: start/end time (itemtype: String)