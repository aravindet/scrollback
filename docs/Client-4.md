Plans for the next major iteration of the Scrollback client-side.

Contents
    [Event types](#event-types)
    [Modules](#modules)
    State object schema

## Event types ##

1. The setState event (currently navigate)
2. Action-up and action-dn events; Action do
3. Query events. Queries will only be between the store and the socket, no other client-side module should interact with them.
4. Hook events (Inter-module communication; conf-show, dialog-show etc.)

## Modules ##

As far as their interaction with the client-side goes, modules can be divided into four categories:

### 1. Stores

This category will initially have only one app, the store (a mixture of interface + pieces of localStorage). It maintains all globally accessible data on the client side, and provides it to the other components. The store will be the only module which may: (1) write to the global state object, (2) listen to action-dn events, (3) emit queries.

All other apps communicate with the store via setState (currently navigate) events.

Basically, all the “business logic” resides in the store. We may eventually split the store into modules based by the kind of data handled ­— a messageStore, a sessionStore etc.

#### 2. Trucks

These carry data between the store and “the outside” — the server (socket), device APIs (phonegap), local and session storage (duh), the parent page (embed), the history API, the URL, notifications... basically, anything that is not a component.

Apart from the socket, the trucks should NOT emit or listen to anything other than setState events.

#### 3. Components

These map to pieces of the DOM. Components must be singletons (“chat-area”), not classes (“chat-element”); classes may be contained inside components. Each component module should also package its own DOM templating and CSS rather than dumping everything into client.html or client.less. (Todo: sequencing, build process)

Components should listen to setState events to update themselves, but must not contain any internal global memory (i.e. everything it needs must be in global state object). In other words, a state object should describe the state of every component in the system completely.

Components may emit action-up events, setState events or hook events based on user action.

#### 4. Hooks

Typically the client-side component of modules with server-side as well, they listen to hook events like conf-show.

Hooks may not read or write from the client-side state object: All data they need should be provided within the hook events. They may not listen to setState events, and should not emit them either unless there is a very special requirement.

Basically, hook modules are intended to be numerous and rarely updated, so we must restrict them to a narrow and stable API (the hook events)

## State Object ##

I might be missing a few things still.

```
{
    nav: {
        mode,
        room, thread,
        query, dialog, dialogState,
       
        textRange: {time, above, below},
        threadRange: {time, above, below}
    },
   
    context: {
        embed, platform,
        /* customizations */
    },
   
    user: { /* user props */ },
    session: "",
   
    /* content caches below */
   
    content: {
        roomx: {
            textRanges: [{start, stop}],
            texts: [],
            threadRanges: [{start, stop}],
            threads: []
        },
        roomx_thready: {
            textRanges: [{start, stop}],
            texts: []
        }
    },
   
    entities: {
        userx: { /* user props */, memberOf: [/* links */] },
        roomy: { /* room props */, hasMember: [/* links */] },
        userx_roomy: { present, /* membership props */}
    }
}
```



## Full app list (current) ##

```
App				Events


anti-abuse-client		config-show		-> showing the tabs word ban
				config-save		-> adding the params to room object on room-up
				text-menu		-> showing the hide message

authorizer-client		config-show		-> showing the tabs for follow options
				config-save		-> adding the params to room object on room-up

calls-to-actions		listens on lot of events to show cta. might change

client-entityloader		navigate		-> to load room object into navigate

client-init			navigate, room-dn	-> manages history, currentState and sets classes to body for mode and view
							-> listens to other events like room-dn to keep currentState.room correct

css customization		navigate

email-client			prev-show		-> show the email prefs tab in Account settings
				pref-save		-> add params to user-up on save

embed				navigate		-> minimize and mazimize and domain verifcation
				init-up			-> adds domain info to init-up
				away-dn			-> show dialog that you left the room.

facebook, google, persona	auth-menu

http				usermenu		-> add logout button
				pref-show		-> notifications tab for sound and desktop, pictures tab
				prev-save		-> add params to user-up
				config-show		-> options for seo, description
				config-save		-> adds params and description to the roomobject in room-up

id-generator			all action-up		-> to add identities
interface			all events		-> constructs the libsb object.

irc-client			config-show		-> showing irc tab in config view
				config-save		-> adding identities and params related to irc.
				room-dn			-> room-dn to show notification for irc command.

localStorage			almost all		-> well this complicated. handles queries, stores session id
							occupants, members, texts, threads and whole lot more.

phonegap-client			navigate		-> to few start up thing on boot

pushnotification-client		pref-save		-> adds devices to params
							-> does a few other gcm stuff.

socket				all			-> acts kinda like storage to server queries. also like a gateway to shoot actions to the server


threader-client			config-show		-> showing the tab for disabling automatic threads
				config-save		-> adding the params to room object on room-up


twitter-client			config-show		-> showing the twitter options
				config-save		-> adding the params, identities to room object on room-up
____________________________


ui folder:
app cache. 			None

browsernotify			text-dn			-> title change, sound/ desktop notifications


chat-area, thread-area		text-up, text-dn	-> to texts sent and receieved
				navigate		-> refresh area

conf-area			navigate		-> to emit config-show when configure is clicked

follow-room			init
				join, part
				back			-> all for the follow button on screen and cofigure button info tab.


guest-settings			auth-menu		-> disable sound notifications button

info-area			navigate, room-dn	-> re-render info tab and show offline status


no-room-area			navigate

notify-ticker			navigate, text-dn	-> show notification in the ticker and minimized widget

oauth listener						-> post messages to pop-ups and send init-up


panes				navigate		-> to switch panes

people-area			away
				back
				join
				part			-> refresh people area

				navigate		-> refresh on room change and connectionStatus change


pref-area			navigate		-> emits navigate away from setting for guests
				init-dn			-> renders prefs on start up

room-area			away			-> remove rooms
				back			-> add currently opened window
				init-dn			-> show rooms based on occupants and members
				room-dn			-> to add newly created rooms in the list
				user-menu		-> My rooms.
				text-dn			-> show msg count


search							-> emits navigate.

signup-area			init
				user-dn			-> show modal for signup and hide it after saving the user

user-area						-> emits auth-menu and user-menu

				init-dn
				room-dn
				back-dn
				navigate		-> adds owner class to body on room change
```