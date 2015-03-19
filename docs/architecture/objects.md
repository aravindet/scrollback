Objects in scrollback can be broadly divided into those that represent persistent, stateful entities such as users and rooms, those that represent state transitions (messages), and those that represent queries for data. Messages and queries are passed as `payload` objects while emitting events.

### Users and rooms ###

```javascript
{
  id:      string, /* human readable */
  description:    string,
  type:    string(user|room),
  picture: url:string,
  createTime: timestamp,
  identities: [string], /* alternate 'ids' for lookup */
  timezone: integer /* number of minutes offset from UTC */
  sessions: [string],   /* active session ids */
  params: {
    app1: { /* app-specific configuration */ },
    app2: { /* app-specific configuration */ }
  },
  guides: {
    app1: { /* app-specific client-side configuration */ },
    app2: { /* app-specific client-side configuration */ }
  }
}
```

Users and rooms are connected through permanent 'member' and temporary 'occupant' relationships, which can be used to query the database. For example, a query can be used to get a list of occupants of a given room. When querying through relationships, additional properties representing the relationship may become available.

#### occupantOf-hasOccupant link
```javascript
  startTime: timestamp, /* date and time of joining or entry */
```
#### memberOf-hasMember link
```javascript
  role: string(owner|moderator|follower|visitor|none|gagged|banned),
  startTime: timestamp, /* date and time of joining or entry */

  transitionType: (request|invite|timeout)
  transitionRole: (moderator|follower|owner|gagged|banned|none)
  transitionBy: string, /* name of the user who sent the invite or ban. set to none for request*/
  transitionText: string, /* some text as part of the role transition */
  transitionTime: timestamp, /* when the transition happens */
```

### Texts ###

See the [Text Action](Actions#text)


### Sessions ###

Session objects are created and retrieved by `init` events, and are 
```javascript
  id: string /* session id; format is <gateway>:<gateway-specific-identifier> */
  user: string /* id of the current user */
  origin: {
    gateway: string,
    /* below: gateway-specific information; non exhaustive */
    domain: string,
    path: string,
    client: ip:string /* web, irc gateways */
    server: fqdn:string /* web, irc */
  }
```

Gateway apps often store additional information about each session in their own memory.

### Threads ###

The title will be part of URLs and will be transformed into headings, etc.

```javascript
{
  id: string,
  to: string,
  title: string,
  startTime: timestamp,
  endTime: timestamp
}
```
