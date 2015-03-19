These are the rules that are enforced in the authorizer app.

### join/part ###
	owner -> *
		allowed
	
	moderator -> :role
		allowed if role != owner
		
	none -> follower
		allowed if room.params.authorizer.openFollow
		else amend to action.request = follower
	
	* -> :role
		allowed if user.invitedRole = role


### admit/expel ###
	* -> :role
		if user is a moderator, any role except owner and moderator
		if user is an owner, all roles are allowed
		if user is a follower && room is openFollow && role = follower
		
		if role is something other than none, banned or gagged:
			allow if victim.requestedRole = role
			else amend to action.invitedRole = role

		in all other cases, deny.

Modification: if until is specified, then add a property fallbackRole = victim.role;

### away/back ###
	allow if levelWeights[room.params.readLevel] <= levelWeights[user.role]
	else deny

### text ###
	allow if levelWeights[room.params.writeLevel] <= levelWeights[user.role]
	else deny

### edit ###
	allow if user.role is moderator or owner
	allow if user is registered AND old.from = old.editInverse[:last].from = from
	
### room ###
	allow if user.role is owner OR old is null

### user ###
	allow if old is null or old.id is from

### getTexts, getThreads ###
	allow if room..readLevel <= user.role, deny otherwise

### init, getRooms, getUsers, getSessions ###
	no authorization rules.



Default Room Permissions
------------------------
- writeLevel: 0 (none)
- readLevel: -1 (gagged)
- openFollow: true
