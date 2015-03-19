Common fields in all queries include:

```javascript
{
  id: string, /* a unique query ID */
  type: (getTexts|getThreads|getSessions|getRooms|getUsers),
  session: string, /* the session ID */
  resource: string, /* the resource (gateway specific identifier) */
  user: User /* loaded by the entity loader; if room is also loaded, should have role properties. */
  room: Room /* optional; loaded if a to property exists */
}
```

Other than that, specific query may have one of these properties:

### getTexts ###

```javascript
{
  to: id,
  thread: string,
  label: string,
  time: timestamp,
  updateTime: timestamp,
  before: integer,
  after: integer
}
```
Returns an array of Text Messages, sorted by time. before or after can not be specified in same query.

### getThreads ###

```javascript
{
  to: id,
  q: string, /* a search string */
  time: timestamp,
  updateTime: timestamp,
  before: integer,
  after: integer,

}
```
Returns an array of Threads, sorted by time. before or after can not be specified in same query.
### getRooms/getUsers/getEntities ###

```javascript
{
  ref: string | array,//'me' for current active user
  identity: string, /* a full identity string or just the 'app' part */

  memberOf: string, /* users only */
  occupantOf: string, /* users only */
  hasMember: string, /* rooms only */
  hasOccupant: string, /* rooms only */
  timezone: {gte: number, lte: number}, /*users only*/
  role: string     /* only for memberOf/hasMember queries; default: all except 'banned' */
  startTime: timestamp, /* for memberOf, occupantOf, hasMember, hasOccupant only */

  createTime: timestamp,
  before: integer, /* only queries with time or created */
  after: integer,
  q: /* a search string */
}
```
Returns an array of Rooms or Users. Example: `core.emit('getRooms', {}, handleResponse);`

