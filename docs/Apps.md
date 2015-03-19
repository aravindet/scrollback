This is a non-exhaustive list; more apps are constantly being added. This list is in the order of priority.

## Current apps ##

### anti-flood ###
Guards against denial-of-service attacks
- Listens to: all actions and queries
- Throws: ERR_FLOOD

### validator ###
Ensures that requests are well-formed and guards against code injection attacks.
- Listens to: all actions and queries
- Injects: any missing property
- Throws: ERR_BAD_ACTION, ERR_BAD_QUERY

### entity-loader
Loads the user and room entities for actions
- Listens to: all actions
- Emits: users, rooms queries
- Injects: user, room

### [authorizer](Authorizer App) ###
Ensures that the action sender has the appropriate permissions to take the specific action.
- Listens to: all messages, queries
- Throws: ERR_NOT_ALLOWED

### browserid-auth ###
Handles the server side validation for user authentication using Mozilla Persona.
- Listens to: init
- Emits: getUsers
- Injects: user
- Throws: ERR_BAD_CREDENTIALS

### facebook-auth ###
Handles server-side validation for facebook authentication.
- Listens to: init
- Emits: getUsers
- Injects: user
- Throws: ERR_BAD_CREDENTIALS

### anti-abuse ###
Uses heuristics to determine if a message might be abusive.
- Listens to: text
- Throws: ERR_ABUSE

### threader ###
Using an external Java process, it detects conversations and threads messages accordingly.
- Listens to: text
- Injects: threads

### [http](HTTP App) ###
Handles the websockets endpoint and http requests, maintains sessions, sends outgoing messages.
- Listens to: all messages
- Emits: all messages and queries, custom http/config query.

### [irc](IRC App) ###
Handles the connections to IRC servers, sends outgoing messages.
- Listens to: all messages
- Emits: most messages; rooms query (on init),room event

### [email](Email App) ###
Sends out daily digests, welcome emails and "you've been mentioned" emails.
- Listens to: text, back, away messages

### [twitter](Twitter App) ###
Watches topics on twitter and emits messages when an interesting tweet happens.
- Listens to: all messages
- Emits: text messages; rooms query (on init)

### admin-notifier ###
Sends emails to the scrollback team when interesting things happen (e.g. new deployments).
- Listens to: back, room messages

### redis-storage ###
Stores a cache of rooms and users.
- Listens to: all messages and queries

### [storage] (Storage App) ###
- Stores messages, room and users etc. Handle queries.
- Listens to: all actions and queries

### leveldb-storage ###
Stores messages, maintains rooms and users, handles queries.
- Listens to: all messages and queries
- Throws: ERR_DATABASE (for queries only)
- Deprecated: See [storage](Storage App)

### analytics ###
Uses the google analytics API to store analytics data.
- Listens to: all messages and queries

### custom-emitter ###
Raises custom events, e.g. 'custom-emitter/sign-up' for apps that are only interested in 'user' properties that are sign ups, or for backward compatibility reasons.
- Listens to: all messages
- Emits: several messages

## Deprecated apps ##

- mysql storage apps
- the *ban plugins -> anti-abuse
- the *validation plugins -> validator