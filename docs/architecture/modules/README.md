### Scrollback Modules

Scrollback contains independent modules that talk to each other. They are based on the [Unix philosophy "Write programs that do one thing and do it well"](http://en.wikipedia.org/wiki/Unix_philosophy#Doug_McIlroy_on_Unix_programming). Each module specialises in one functionality say like storage or authentication or search and so on.

Modules follow the [pub-sub](http://en.wikipedia.org/wiki/Publish–subscribe_pattern
) (short for publisher-subscriber) pattern. These modules have n:n mapping that is, any module can pub-sub to any module.

The following are brief descriptions about the modules in use:


### Client-only

### Server-only

### Common

### Analytics

Uses the google analytics API to store analytics data.
- Listens to: all messages and queries

### Anti-Abuse

Uses heuristics to determine if a message might be abusive.
- Listens to: text
- Throws: ERR_ABUSE

### Anti-Flood

Guards against denial-of-service attacks
- Listens to: all actions and queries
- Throws: ERR_FLOOD

### Authorizer

Ensures that the action sender has the appropriate permissions to take the specific action.
- Listens to: all messages, queries
- Throws: ERR_NOT_ALLOWED

### Browserid-Auth

Handles the server side validation for user authentication using Mozilla Persona.

- Listens to: init
- Emits: getUsers
- Injects: user
- Throws: ERR_BAD_CREDENTIALS

### Censor
### Entity Loader

Loads the user and room entities for actions

- Listens to: all actions
- Emits: users, rooms queries
- Injects: user, room

### Email

Sends out daily digests, welcome emails and "you've been mentioned" emails.
- Listens to: text, back, away messages

### Facebook

Handles server-side validation for facebook authentication.
- Listens to: init
- Emits: getUsers
- Injects: user
- Throws: ERR_BAD_CREDENTIALS

### Github
### Google
### HTTP

Handles the websockets endpoint and http requests, maintains sessions, sends outgoing messages.
- Listens to: all messages
- Emits: all messages and queries, custom http/config query.

### IRC

Handles the connections to IRC servers, sends outgoing messages.
- Listens to: all messages
- Emits: most messages; rooms query (on init),room event

### Search
### Twitter

Watches topics on twitter and emits messages when an interesting tweet happens.
- Listens to: all messages
- Emits: text messages; rooms query (on init)

### Threader

Using an external Java process, it detects conversations and threads messages accordingly.
- Listens to: text
- Injects: threads

### Translation


### Validator

Ensures that requests are well-formed and guards against code injection attacks.

- Listens to: all actions and queries
- Injects: any missing property
- Throws: ERR_BAD_ACTION, ERR_BAD_QUERY

### Redis Storage

Stores a cache of rooms and users.
- Listens to: all messages and queries

### Storage

- Stores messages, room and users etc. Handle queries.
- Listens to: all actions and queries

### LevelDB Storage (soon-to-be deprecated!)

Stores messages, maintains rooms and users, handles queries.
- Listens to: all messages and queries
- Throws: ERR_DATABASE (for queries only)
- Deprecated: See [storage](Storage App)

### Custom Emitter

Raises custom events, e.g. 'custom-emitter/sign-up' for apps that are only interested in 'user' properties that are sign ups, or for backward compatibility reasons.

- Listens to: all messages
- Emits: several messages