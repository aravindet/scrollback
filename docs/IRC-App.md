IRC app consist of 2 different processes (client and server) which communicate through TCP sockets. 
##IRC##
Communicate to IRCClient through Object exchanging.

IRC params object structure:
```javascript
{
    params: { 
       irc: {
            server: {string},
            channel: {string},
            enabled: {boolean},
            pending: {boolean}//true if room verification is pending
        }
    }
}
```
##IRCClient##
Listen to request from IRC.
Provide following functions
- **connectBot(room, options, callback)**
- **partBot(roomId, callback)**
- **partUser(roomId, nick)**
- **connectUser(roomId, nick, options, callback)**
- **say(message)**

     Message Object: 
     ```javascript 
     {
         from: {string},
         to: {string},
         text: {string}
     }
     ```
- **rename(oldNick, newNick)** : rename old nick to new Nick on all IRC servers.
- **newNick(roomId, nick, sbNick)** : if suggested nick is changed to sbNick.
- **getCurrentState(callback)** : return with current state of server
      
      State Object: 
      ```javascript
      {  
         room: {object},//rooms Object
         servChanProp: {object},//users and room
         servNick: {object}//map of servNick ---> sbNick
      }
      ``` 
- **getBotNick(roomId, callback)** : returns bot nick as an object i.e data.nick  
- **isUserConnected(sbNick, callback)** : returns true or false
- **disconnectUser(sbNick)** : disconnect user from all servers.

To call any function remotely client have to pass an object of following type:

### Request Object ###
```javascript
 {
       uid:{string},//a unique ID to match reply.
       type: {function name},
       /*
        function parameters with same parameters name(important)
        */
  }
```
### Response Object ###
```javascript
{
    type: 'callback'//if reply of request,
    uid: {string},//uid of reply 
    data: {object}//specific reply of request as an object.   
}
```
### Messages ###
IRC activity is reflected by sending Objects of different types: 
```javascript
{
     type: {back | away | message | ircError | init}
     /*
     parameters based on type of message.
     */
}
```