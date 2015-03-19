When using the Embed API, parent sites can often use unique identifiers in their own namespace to refer to rooms and threads in Scrollback.

## Rooms ##
Every API call that accepts a room ID can also accept a _namespaced identifier_. For example, if your domain is `example.com` and the unique identifier for the room on your site is `myroom123`, you can pass `example.com:myroom123` as the `room` ID. Note that the `:` character is illegal in normal room IDs — the presence of this character is used as a signal that a namespaced identifier is used instead of an ID. Scrollback will maintain a mapping of this identifier to the actual room ID.

The actual room ID can be read from the navigation state object: `scrollback.navigation().room`, but this is usually unnecessary.

If you use the `createRoom` navigation option and no room exists with that identifier, the user will be prompted to create one in the UI. Note that the user who creates a room will become its owner and the domain added as a trusted domain. Use this only when you want to allow your users to easily create Scrollback rooms that they own.

## Threads ##
Similarly, identifiers that are relevant to your application may be passed to the `thread` property while initializing or navigating. For example, if you have a blog `example.com` and you want to insert a scrollback widget under post ID `1234`, pass the thread property `example.com:1234`. If the thread doesn't exist, it will be created automatically.

The namespaced identifier will be converted to a thread ID by hashing it.