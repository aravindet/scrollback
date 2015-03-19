###Twitter App
It searches for most recent tweets and emit text.

Twitter params object structure:
```javascript
room: {
      params: {
           twitter: {
                   username: {string},
                   tags: {string},//space separated
                   token: {string},//twitter secret token of user
                   tokenSecret: {string},// twitter user secret
                   profile: { screen_name: {string}, user_id: {string}}
           }
      }
}
```

Twitter app adds **twitter** label to text action.