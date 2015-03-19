The HTTP App responds to HTTP Reqests and gateways with the Web client through Websockets.

### HTTP Endpoints ###

- /:roomid
   - Redirects to /:roomid?p=:lastpagetime

- /:roomid?p=:pagetime
   - Static: A list of at most 15 threads that started after the given time, with pagination links. Items link to /:roomid/:threadid/:threadlabel.
   - Dynamic: In addition to the room details pane (with the list of threads), and the chat log pane at that timestamp is loaded dynamically. For the last page, the chat pane will start scrolled to the bottom, for other pages it will be at the top. After the chat pane loads, the list of threads will be synced with it and will remain in sync during scroll.

- /:roomid/:threadid/:threadlabel[?p=:nextpagetime]
   - Static: Shows 50 texts, from the beginning of the thread or at the given timestamp, along with pagination links.
   - Room details pane as before, but with a particular thread selected.

- /me
   - No static page
   - Dynamic page displays the list of rooms the current user is a member of, with chat logs of the selected room on the right.


