###Email App
Sends out daily digests, welcome emails and "you've been mentioned" emails.
####Daily Digest and Mention Email
#####Implementation
Email app usage redis to store last 2 days messages and threads information.
Particularly it stores following information.
* Count of messages in each thread.
* Title of each thread.
* Last 3 messages of each Thread.
* Messages where any user has been mentioned.
* Update time of each thread in a sorted set (zadd in redis).
* Set of users who might get email in next tick (based on min time interval between mentioned email).

Each hour email app emits a ```getUsers``` query based on timezone and send emails to users.
It also sends mentions email to users if max waiting time is up.
###Welcome Email
Listen for ```user``` event and sends welcome emails.

