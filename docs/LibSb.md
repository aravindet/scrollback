LibSb is a chaining Event Emitter with some additional methods to trigger internal events that are handled by built-in client-side apps.

## Apps ##

### Client-only ###

- **localStorage**: Stores information about the current user, their memberships and presence, and the rooms, texts and labels they have seen. Acts as the "storage" app on the client side. Uses LocalStorage.

- **socket**: Establishes and maintains socket connections with the server, takes care of transparently routing actions to the server and triggering callbacks with returned data. Uses sockjs.

- **interface**: Provides the public JavaScript interface into libsb. It provides methods that clients can call (in preference to calling `.on()` and `.emit()`.

- **admin**: Handles administration options like banning people and hiding messages.

### Client-and-server ###
All server-side apps that support per-room configuration or per-user preferences provide configuration UI and client-side code to drive interactivity.

- **persona-login**: Handles login.

## Properties

Properties of the libsb object provide access to most frequently used data in a synchronous manner.

- **user**: the current user
- **rooms**: an array of rooms that are open in this page.
- **occupantOf**: an array of rooms the current user is present in. This includes everything in rooms as well as rooms that may be open on other tabs
- **memberOf**: an array of rooms the current user is a member of. The ‘role’ property may be ‘member’, ‘owner’, etc.
- **isConnected**: connected to the server or not.

## Methods ##

These methods are provided by the interface app, and are the primary means for consumers to interact with libsb.

- **connect(host, options)**: Establishes a socket connection. Host defaults to https://scrollback.io. Options currently supports only one property, ‘suggestedNick’.

- **disconnect()**: Disconnects.

- **getTexts(query, cb)**: Encapsulates a getTexts [[query|Queries]]. May be served by the cache. Query specifies a time and the number of messages before and after; if the time is null, the current time will be used.

- **getLabels(query, cb)**: Query specifies a time and number of labels before and after, or a search string ‘q’. Returns labels matching the criteria.

- **getOccupants(rid, cb)**: Returns occupants of the given room.

- **getMembers(rid, cb)**: Returns members of the room.

- **getRooms(query, cb)**: Searches for rooms according to provided criteria.

- **enter(rid, cb), leave(rid, cb)**: Enters or leaves a room. Emits away or back actions and loads and caches room info.

- **join(rid, cb), part(rid, cb)**: Joins or parts a room. Emits join or part actions and loads and caches room info.

- **say(rid, text, threadId, cb)**: Sends a text to a room. threadId is optional.

- **roomConfigForm(rid, cb)**: The callback receives a DOM &lt;form> element

- **userPreferForm(uid, cb)**: The callback receives a DOM &lt;form> element

## Events

All queries on the server side are available on the client as well; all actions are replaced with -up and -dn variants. See [[Events]] for details. In addition, the following events are used in client-side code.

- **ready**: All libsb scripts have initialized.
- **connected**: Emitted by socket after connection is established
- **disconnected**: Emitted by socket on intentional or accidental disconnection.
- **auth-menu**: Emitted by interface to request all apps providing login to advertize this capability.
- **user-menu**: Emitted by interface; apps providing user-specific actions respond to this.
- **text-menu**: Emitted by interface; call for text-specific actions.

LibSb consumers can listen to these and use the events to trigger navigation.
- **config-show**: Emitted by the UI to display the room configuration. The payload is a tabs object. Each app adds it's room level settings to the payload as a new tab. 

The app-wise payload has the structure as demonstated by the sample irc client app below: 
```javascript
tabs.irc = {
    html: div, // div contains the jQuery elements provided by the client app to be added to the settings DOM.
    text: "IRC Settings", 
    prio: 100 // the app settings are displayed in the decreasing order of priority.  
}
```
- **pref-show**: Similar to the config-show event above, the pref-show event is emitted by the UI in order to fetch the user level settings from the listener client apps. It has a payload structure similar to config-show. 

- **config-save**: Emitted by the UI, when a room configuration form was saved.
- **config-cancel**: Emitted by the interface, when a room configuration form was cancelled.

- **pref-save**: Emitted by the UI, when a user preferences form was saved.
- **pref-cancel**: Emitted by the interface, when a user preferences form was cancelled

Events emitted when the properties of libsb (user, rooms, occupantOf, memberOf) are updated.
- **user-update**
- **rooms-update**
- **occupantof-update**
- **memberof-update**

UI Events
- **navigate**: Emitted by the user interface. The payload has the following properties:
```javascript
{
	room: 'roomid',
	mode: (normal|search|conf|prefs|home),
	tab: (info|people|threads|local|global|<conf/pref tabs>),
	thread: 'selected_thread',
	query: 'search_query',
	text: 'selected text id',
	time: 'current scroll time'
	source: 'user interface component where the event originated'

	old: {previous state object},
}
```