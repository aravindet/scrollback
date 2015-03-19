Both the server and client are built around 'core' event emitters. They expose two functions:

### core.on(eventType, listener, priority)

`eventType` is a string, `listener` is a callback function and `priority` can be a number or the name of a 'priority bucket'. Listeners are invoked by inceasing priority; priority buckets are strings carrying pre-defined values for each event type.

### core.emit(eventType, payload, callback) ###

`payload` is an object containing data describing the event and `callback` will be invoked when all the listeners have completed or if one of the listeners cancelled the event. `callback` receives two arguments, `err` and `data`, in the normal Node.js pattern.

### Server event types ###

Events can be broadly classified into those that write data, known as [[Actions]], and those that read data, known as [[Queries]]. See those pages for a full list.

In addition, some apps may emit "Custom events" that other apps may listen to; for example, "custom-emitter/sign-up" is raised when a new user account is created.

One important custom events is [[http-init Event]] which is raised by the http app soon after all the apps are initialized. Other apps may listen to this to to register url handlers. For example, twiiter app may use this to register url handler for login.

### Client event types ###

The client core will have the same [[Queries]] as the server side. Each server-side action corresponds to two [[Actions]] on the client, one each for the up and down directions (to and from the server). For example, 'text-up' and 'text-down' events. The payload for both is a text message.

There are also several custom events on the client side, corresponding to the [[client-side apps|Client]].

#### socket app ####
- **socket/connect**: A socket connection is established. The payload is empty.
- **socket/disconnect**: The connection was lost. The payload is empty.

#### ui app ####
- **ui/ready**: UI rendering (initial or after a navigation event) is completed; the payload has one property, 'view', which is a string that may be one of 'chat', 'roomconfig', 'roominfo', 'roomlist', 'userinfo', 'userlist' or 'userconfig'.
- **ui/navigate**: User began navigation (the current view is being unloaded). The from and to views are in the payload.
- **ui/menu**: UI

### How it works ###

When an event is emitted, the highest priority listener is invoked with two arguments: the `payload` passed to `emit()` and a function `done`. The listener may carry out asynchronous functions and modify the payload object itself before calling `done` to signal completion.

The `done` function can be invoked with zero, one or two arguments:
1. **done()** - continue processing the next listener. If all listeners are completed, return the payload to the `emit` callback.
2. **done(error)** - cancel the event and return the error to the `emit` callback immediately.
3. **done(true, results)** - called by query listeners, this signals that the query results have been retrieved. It cancels downstream listeners and returns the results to the `emit` callback immediately, passing 'null' as the error argument.
