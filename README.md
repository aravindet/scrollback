## Sindhu's fork of Scrollback

![](http://gifrific.com/wp-content/uploads/2012/11/Dachshund-Playing-With-Toy-Penguin.gif)

This repo's master branch contains commits on top of existing [Scrollback](http://github.com/scrollback/scrollback) master. Some of these commits may not have made it upstream but __ideally__ this is how I would like Scrollback to be. Branches on this repo are a work-in-progress (wip) and may disappear after they have been accepted upstream.

## Scrollback, where communities hang out

Scrollback provides a free-to-use service at [scrollback.io](http://scrollback.io). If you’re planning run a community chat room, you should [try it](https://scrollback.io/me) out now!

You can even [embed Scrollback room](https://github.com/scrollback/scrollback/wiki/Basic-Usage#embed-scrollback-room) on your webpage/blog.

## Install

To run Scrollback locally on your computer, run `wget https://raw.githubusercontent.com/scrollback/scrollback/master/tools/install.sh -O install.sh && chmod +x install.sh && ./install.sh`.

This is a Bash 4.x only script that installs missing system and application dependencies from your default package manager, creates the required databases, builds client-side JS and style and starts Scrollback for you at `http://localhost:7528`.

The install script requires __root__ priviledges for these cases:

1. If you are missing system depdencies (nodejs, npm, git, redis and postgres) 
1. If you have a Debian system, you are almost most certainly would be missing Postgres 9.4 and so we would need to add Postgres' source to your `/etc/apt/sources.list.d/` dir because Postgres 9.4 is a strict dependency, without which Scrollback won't be able to run.
2. If your `/usr/local/bin/` and `/usr/local/lib/` dirs are not writable, we won't able to install nodejs modules that require to be on system paths (gulp and bower).

### Configure

If you have or want custom server and/or client configuration, you can override the defaults by creating `server-config.js` and/or `client-config.js`. For sample config files take a look at [`server-config-sample.js`](https://github.com/scrollback/scrollback/blob/master/server-config.sample.js) and [`client-config-sample.js`](https://github.com/scrollback/scrollback/blob/master/client-config.sample.js).

## Contribute to Scrollback

Want to hack on Scrollback? Awesome! We maintain a [wiki](https://github.com/scrollback/scrollback/wiki) that explains Scrollback architecture. It's a work-in-progress so please let us know if there are gaps in the content. Better yet, feel free to send us a Pull Request to make it better.

## License

Scrollback is licensed under GNU Affero General Public License. For more information see [http://www.gnu.org/licenses/agpl-3.0.html](http://www.gnu.org/licenses/agpl-3.0.html)