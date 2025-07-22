2025-01-21 17:58

Status: #Leaf

Tags: #API #Database #WebDev #React 

# Optimistic vs Pessimistic Updates
When we update the server in response to user input we perform an *update*, which can be one of two types:
- **Optimistic Update:** We update the UI first and then update the server to persist the changes
	- Pro: Faster response time for user
	- Con: If update fails we need to reverse user input
	- Best for updates that almost always succeed
- **Pessimistic Update:** We update the server and only update the UI once the update is successful
	- Pro: A failed update won't cause issues
	- Con: Slower response time for user
	- Best for updates that commonly fail

## References
