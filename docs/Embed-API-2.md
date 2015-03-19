**This specification is a work in progress. Some of it has not been implemented yet and features may be changed or removed completely without warning.**

## Goals ##
* Support single-page apps which need to reinitialize Scrollback without reloading the page
* Provide APIs for the parent page to interact with the Scrollback widget

## Embed the Scrollback API##
```html
<script>document.write('<script src="http'+
(location.protocol=='https:'?'s':'')+
'://scrollback.io/sb.js"><\/script>');</script>
```
The embed script above loads the Scrollback API onto the page and creates a global function `scrollback`. Call this with some options to create a widget.

## Creating a widget ##
This shows all the options. Note that many options clash, so this example is somewhat contrived. In practice you will only be using a subset of these.
```html
<script>
	var widget = scrollback ({
		/** widget position **/
		form: "toast",
		minimize: true,
		container: 'scrollback-container', // for form=canvas

		/** widget options **/
		showrooms: false,   // or an array of rooms
		showthreads: false,
		showinfo: false,
		showpeople: false,
		showsearch: false,
		alertSound: false,
		alertTitle: false,

		/** initial navigation state (see page) **/
		room: "hasgeek",    // see identifier normalization
		thread: "jsfoo-express-jade-nodejs-workshop",
		time: 1402877428,
		tab: 'people',
		mode: 'settings',
		view: 'meta',
		dialog: 'signin',

		/** navigation options **/
		createRoom: false,

		/** single sign-on **/
		nick: "jace",
		jws: "eyJ…CJ9.eyJ…Mn0.vuq…jR0" // See single sign-on
	});
</script>
```
All the properties of the settings object are optional. The returned `widget` object can be used to make other API calls.

Some of the options above are explained in more detail in [[Navigation State]], [[Identifier Normalization]], [[Single Sign-On]]

## Widget API ##
The Widget API provides fine grained control but should be unnecessary for most use cases. It provides four primary groups of functionality: navigation, single sign on, following rooms and widget control.

This API is not yet designed to provide access to the content of rooms.

### widget#navigation([navState: object, [callback: function]]) ###
Gets and sets the navigation state object containing the current room, thread, timestamp, dialog, tab or view.
- `widget.navigation()` returns the state object.
- `widget.navigation(state, callback)` sets the state and fires the callback (if provided) when the widget has completed navigating.
- Security: Room consent required while calling with no arguments.

### widget#following([room: string,][value: boolean, [callback: function]]) ###
Gets and sets whether the current user is following the current or specified room.
- `widget.following()` returns true if the current user is following the current room, false otherwise.
- `widget.following('hasgeek')` returns if the current user is following the hasgeek room.
- `widget.following(true, callback)` makes the current user follow the current room and triggers the callback when confirmation is received from the server.
- `widget.following('hasgeek', false)` makes the current user unfollow the current room.
- Security: Room and User consent required.

### widget#signin(authTokens: object or false, [callback: function]) ###
Perform signin. To sign out, call with false.
- `widget.signin({nick: 'jace'}, callback)` attempts to change the guest nickname to jace, and calls back when done.
- `widget.signing({jws: '…', nick: 'jace'})` attempts to sign in using the email address in the jws, and suggests the nickname ‘jace’ if the user is new. The user is always given an option to change this before his/her account is created.
- Security: User consent (not blacklisted) required for jws login.

### widget#options([options: object]) ###
Gets and changes widget settings, like showing and hiding components or changing notification settings.
- `widget.options()` returns an object with all the options with their current values.
- `widget.options(changes)` updates the options that are specified.
- Security: None

## Security Model ##
- The origin of the calling code is verified using origin-restricted inter-iframe messaging.
- **Room**: Rooms can specify a whitelist of origins. Only API calls from whitelisted origins are allowed to perform sensitive actions affecting these rooms. Room configurations can control whether some actions (e.g. viewing messages) should be treated as sensitive.
- **User**: Users can whitelist or blacklist origins to "sign in" for them. Only non-blacklisted origins can perform single sign-in as the user. Sign-in from non-whitelisted origins will cause a notice to be sent to the user. Sensitive actions affecting a user is only allowed from the origin that signed them in — i.e. you must provide a valid JWS token while creating a widget before calling any sensitive action on it.

If a room or user is created through an API call from a particular origin, that origin is automatically whitelisted for that entity.