A token is a digital token that indicates that the user has been authenticated

ex: like a security on the gate of an building which when you want to get in building the security wants an ID card from you and when you show the ID card and you get approved  he gives you a paper(token) which you can stay in building and until you have that paper you can walk around the building

### What JWT is

#### Industry standard for tokens (RFC 7519)
	which means JWT contains all the necessary information and there is no need 
	for server to read the informations

example:
	`{`
	  `"userId": "123",`
	  `"role": "admin",`
	  `"exp": 1719830400`
	`}`

the server understand by reading this token(**without searching the database**):
	1- Who is the user?
	2- What is the role?
	3- How long is the token valid?

#### Self-contained and can contain 
	. Credentails(help us to know who is the user without searching the database)
		1- UserId
		2- UserName
			
	. Claims
		The information that the token claims about the user 
			ex
				1-"role": "admin" => this user is admin
				2- exp: 1719830400 => This token is valid to date
				
	. Other Information
		Token can contains anything which is useful for the server
			ex: 
				"lan": "fa",
				"theme": "dark",
				teamId: 5

### JWT Structure
[[jwt-structure.png]]
	
	1- Header: The token type, algorithm, cryptoghraphy
	2- Paload: User information(role, userId, ...)
	3- Signature: Digital signature which That cheating on the token is hard


### Token Authentication
[[token-auth.png]]


### Benefits of JWT
1- No session to manage - JWT are self contained tokens
	all the necessary information are save on token and there is no need to database call
	
2-Portable - A single token can be used with multiple backends
	Token is only string which we can use everywhere
		you can use the JWT which you get from one server for the different servers and there is no need to login again for another servers
		
3- No Cookies required - mobile friendly
	You can save token on local storage or session storage

4- High Performance
	When the token generates there is no need to call database for any request
		(Who is this user?)

	only need to check JWT signature which is fast 
	