Actions represent state transitions; The text action is also an entity that can be modified and queried. All actions have some common fields.

Before reaching the “Entity Loader” app:
```javascript
{
  id:   string, // guid
  type: string(text|back|away|join|part|admit|expel|room|user|edit|init),
  to:   id, /* always "me" for user and init actions */
  time: timestamp,
  session: string, /* the session id */
  resource: string, /* the resource (gateway specific identifier) */
  origin: { gateway: string, /* gateway-specific properties */ }
}
```

After “Entity Loader”:
```javascript
{
  id:   string, // guid
  type: string(text|back|away|join|part|admit|expel|room|user|edit|init),
  from: id,
  to:   id, /* always "me" for user and init actions */
  user: User, /* if room is also loaded, should have role properties. */
  room: Room, /* not defined for user and init actions */
  time: timestamp,
  session: string, /* the session id */
  resource: string /* the resource (gateway specific identifier) */
  origin: { gateway: string, /* gateway-specific properties */ }
}
```

### Action types ###

Actions serve as event payloads, i.e. a 'text' event is emitted with a 'text' action as payload, a 'back' event with a 'back' event etc. On the client, the 'text' event can be the payload for both the 'text-up' and 'text-down' events, 'back' for 'back-up' and 'back-down' events, etc.

 - **text**: A piece of communication from a user to the room, both action and object
 - **back**/**away**: The sender changes their own presence status in the room
 - **join**/**part**: The sender changes their own affiliation to the room
 - **admit**/**expel**: The sender changes another user's affiliation to the room
 - **room**/**user**: The sender creates or modifies a room/user account
 - **edit**: The sender modifies an existing text object
 - **init**: The sender initializes a new session or changes an existing one

All actions except `init` and `user` are sent to a room and may be forwarded to all its occupants and followers. Init and user actions are sent to the special endpoint 'me', and represent changes to the current user's session or account.

Depending on the action type, other fields may be present.

### Text
```javascript
  text:   string,
  mentions: [string],
  threads: [{id: string, title: string, score: number}],
  labels: {<string>: number},
  cookies: [string], /* usernames that have given a cookie to this message */
  updateTime: timestamp /* time of the last update */
  editInverse: [ {text: string, threads: [], labels: {}, cookie: boolean, from: string, time: timestamp } ]
```

Edit inverse is an array of "undo" operations that reverses the edits that have been made on this text. Undo objects have the same schema as Edit actions, excluding several fields. The latest edit will be the last element of the array.

### Back/Away
```javascript
  status: (online | offline | inactive)
  text: string /* optional */
```

### Join/Part
```javascript
  role: (owner|moderator|follower|none) /* optional; only part can specify owner/moderator */
  transitionRole: (owner|moderator|follower) /* set by authorizer if immediate transition is not allowed */
  transitionType: (request)
  text: string /* optional */
```

### Admit/Expel
```javascript
  ref:  id, /* The id of the user being admitted or expelled */
  victim: User, /* the user object referred by ref. */
  role: (owner|moderator|follower|none|gagged|banned) /* optional */
  transitionRole: (follower|moderator|owner)
  transitionType: (invite|timeout)
  transitionTime: timestamp
  text: string /* optional */
```

### Room/User
```javascript
  old: Room/User /* the old room or user, loaded by entity-loader; The 'room' or 'user' property will contain new data. */
```

### Edit
```javascript
  ref:    id,
  old:    Text /* the original text, loaded by entity loader. */
  text:   string,
  threads: [{id: string, title: string, score: number}],
  labels: {<string>: number},
  cookie: boolean
```

Text changes the content of the underlying text object; Threads can be used to add, edit or remove a thread candidate (to delete, set score to null). Labels can be used to add, edit or remove a label (e.g. abusive, hidden, sticky, announcement), and Cookie can be used to give or withdraw a cookie (gamification).

### Init

```javascript
  suggestedNick: string, /* a suggested nickname for this user */
  old: User, /* the guest user; 'user' will be populated with the logged in user's info */
  occupantof: [Room],
  memberof: [Room],
  auth: { app: { /* app-specific identity information */ } },
  origin: { /* session origin object; see Session object for schema */ }
```
An init action is emitted by interactive 'gateway' apps, to signal the creation or update of a user [session](Objects#sessions). Other apps, such as  storage apps, are responsible for adding the other properties.
