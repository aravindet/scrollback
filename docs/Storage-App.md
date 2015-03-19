#Storage

Storage module is used to store messages, rooms and users information. It is currently implemented using postgres. 
A query representation(query transform) is used to represent select, insert, update and upsert queries.
## Query Representation
Each action and query is transformed to a new query that is used to generate sql query.
```javascript 
{
   type: insert | update | upsert | select,
   source: 'string', // table name or source of data
   sources: 'array', // if multiple sources
   lock: 'string', // used to lock upsert query.
   filters: [
      [name(string or array), operator, value (string or array))],
      .....
   ],
   update: [[column name, operation, value],
   ...... 
   ], // only for update query
   iterate: {
	keys: [], start: [], reverse: boolean, limit: number
    }
}
```
For more information on query representation see [action-transform.js](https://github.com/scrollback/scrollback/blob/master/storage/action-transform.js) and [query-transform.js](https://github.com/scrollback/scrollback/blob/master/storage/query-transform.js). 

##Action Transform
Action transform is used to transform actions (```text```, ```room```, ``` user```, ```join, part, admit, expel```) to a new query representation that is used to generate sql queries.

See [action-transform.js](https://github.com/scrollback/scrollback/blob/master/storage/action-transform.js)
##Query Transform
Query transform is used to transform queries (```getEntities, getUsers, getRooms```, ``` getTexts, getThreads```).

See [query-transform.js](https://github.com/scrollback/scrollback/blob/master/storage/query-transform.js)
##Result Transform
Result transform is used to transform result of queries from database to ```results``` array.

See [result-transform.js](https://github.com/scrollback/scrollback/blob/master/storage/result-transform.js)


[postgres.js](https://github.com/scrollback/scrollback/blob/master/storage/postgres.js) is used to put and get data from postgres database.
