These options can be specified as properties of the embed config object (the one that is defined as `window.scrollback = { … };` in your embed code. The following properties are available:

### form ###

Can be "toast" or "canvas". Toasts are inserted with fixed positioning at the bottom of your page (similar to Gmail or Facebook chat), while "canvas" inserts an iframe into a div you specify.

If you use the form "canvas", you should have an element with the id `"scrollback-container"` on the page, and an iframe will be inserted into it.

If you use the form "toast", you can reposition the widget by adding CSS to the `"scrollback-stream"` class. Note that you may need to override the default CSS values using `!important`.

### minimize ###

Boolean. Specifies whether scrollback should start minimized (only has effect if `form` is `toast`).

### room ###

Specifies a room to display in the embed. If the room does not exist or if it is not permitted to be embedded on this domain, appropriate error messages will be shown.

### thread ###

Specifies a thread to display in the embed. This can be a website-specific identifier, and it will be hashed with the room name as salt to obtain a globally unique thread id. If no such thread exists, one will be created automatically.

### nick and jws ###
See [[Single Sign-On]]. Note that jws is planned but not currently implemented.

### createRoomLike ###

Create a new room (with id and name from the room property) and copy the description, picture and settings from the room specified here. This only works if this domain has been whitelisted to create rooms by Scrollback, and no room with that id currently exists.