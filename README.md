# Practice-Server-and-ExpressBlogger
### In-Class - Day 1

- Introduce Express Generator
	- Create a new github repo called ExpressBlogger, clone the repo to your computer and add the link to populi
		- Add a Readme = checked
		- .gitignore template = node
		- License = none
		- _Clone_
			- Click green Code button
			- Copy link
			- git clone <link>
			- cd into repo
	- Install and run express-generator
		- npx express-generator -e
		- _Note_: You can ignore all warnings
	- Install node_module packages
		- npm i
- Introduce .gitignore
- Introduce Package.json start scripts
	- npm start
	
- Break + Class ToDo:
	- Read through and review new app.js

- Create a new postman collection "ExpressBlogger"
- Create two postman requests in the ExpressBlogger collection:
	- One for the default route in index.js "/" called "index.js default"
	- One for the default route in users.js "/users" called "users.js default"
- Explain route name concatenation

- Create a blogs route file
	- Create a new file called ./routes/blogs.js
	- Copy/Paste users.js code into ./routes/blogs.js
	- Require blogs.js in app.js
		- var blogsRouter = require('./routes/blogs');
	- Add the new blogs router in app.js
		- app.use('/blogs', blogsRouter);
	- Modify /blogs to send sample blogs message
		- /* GET blogs default */
		router.get('/', function(req, res, next) {
			res.json({
				success: true,
				route: "blogs",
				message: "hello from the blogs default route"
			});
		});
	- Post sampleBlogs in slack
	- Add sampleBlogs to the top of the ./routes/blogs.js file

### Requirements - Day 1

- Create two GET routes in ./routes/blogs.js
	- GET /blogs/all that will send the entire list of sample blogs in the HTTP response
		- _Remember_: Since route names concatenate with the blogs router code we added to app.js, we only need to name our new route "/all" in blogs.js. This is because all of the routes we will create in blogs.js will concatenate their route name with "/blogs" from the app.use('/blogs', blogsRouter); line of code in app.js. 
		- _Note_: It is good practice to send success: true in the response JSON
			- res.json({
					success: true,
					blogs: sampleBlogs
				});
	- GET /blogs/single/:blogTitleToGet that will send a single blog from the sample blogs in the HTTP response based upon the blog title passed into the url
- Create one DELETE route with a single route param blogTitleToDelete
	- DELETE /blogs/single/:blogTitleToDelete that will delete a single blog in the sample blogs based upon the blog title passed into the url
	- _Note_: Even though we are not sending any blog data back with this request, we still need to respond to the request. So we will res.json an object containing success:true.
- Create Postman requests for all of these routes and test to see that they work
	
### In-Class - Day 2

- Introduce nodemon, dev dependencies
	- Install nodemon
		- 'npm i --save-dev nodemon'
	- Change the default start script to use nodemon instead of node
		- "scripts": {
				"start": "nodemon ./bin/www"
			}

- Break

- Server-side validation
	- Create /validation folder
	- Create /validation/users.js file
	- Create a new function called validateUserData with a single parameter userData 
		- In validateUserData, console.log userData
		- Add code so that if userData is undefined, validateUserData returns {isValid: false}; otherwise, it returns {isValid: true}
	- Add validateUserData to the module.exports and import (via require) validateUserData into /routes/users.js
		- Default vs Named import
	- In /routes/users.js
		- Create a new global variable userList initialized to an empty array
		- Create a new POST route '/create-one'
		- In the /users/create-one route:
			- Create new variables to get the new user fields from the req.body
				- email: {String}
				- firstName: {String}
				- lastName: {String}
				- age: {Number}
				- favoriteFoods: {String[]}
			- Create a new variable userData that has all the above fields as properties along with the following generated properties:
				- fullName (firstName + lastName): {String}
				- createdAt: {Date}
				- lastModified: {Date}
			- Add a new variable that calls validateUserData with the userData passed in as an argument
				- const userDataCheck = validateUserData(userData)
			- Add code so that if userDataCheck.isValid is false, we respond with success: false; otherwise, we respond with success: true by default
				- _Note_: Right now, both the validation function and the http response are sparse in functionality. We will first get the route working at a minimum level and then come back to implement more robust functionality.
			- Add code to push userData into the userList array AFTER userData has been validated
	- Create a Postman request for the route and test to see that it is working
	- Now, let's build out the validateUserData function. Implement the following:
		- Add validation to check the following conditions:
			- Email, firstName and lastName are required and must be strings
			- If age is defined, it must be a number
			- If favoriteFoods is defined and is an array, all entries must be strings
		- Update the validation conditions to return a message string along with the isValid: false result indicating what specific validation failed
	- Update the code in /users/create to send the validation message if userData is invalid in the http response

### Requirements - Day 2

- Create a new file /validation/blogs.js. In /validation/blogs.js:
	- Create a basic validator function for blogData and add that function to the module.exports
- In /routes/blogs.js:
	- Import (require) the blogData validator function into /routes/blogs.js
	- Create one POST route /blogs/create-one to create a new blog post
		- _Note_: Do not forget to generated createdAt and lastModified in the new blog post
	- Create one PUT route /blogs/update-one/:blogTitle to update a blog post
	- Both of the above routes should run validations on the incoming blog post body data BEFORE either creating a new blog post or updating a blog post. If the blog data is invalid, then a message should be sent in the http response indicating which validation failed and why
- Build out the blogData validator function to check for the following conditions
	- Title, text and author are required fields and they should be strings
	- Title and author should be no longer than 40 characters in length (letters + whitespace)
	- _Stretch Goal_: 
		- If category is defined and has a length greater than 0:
			- There can be no more than 10 entries for category 
			- All the entries must be strings
			- All categories must be in the following list of strings:
				- "Lorem"
				- "ipsum"
				- "dolor"
				- "sit"
				- "amet"	

### In-Class - Day 3 - Morning

- Review Day 2 assignment
- Implement Day 2 array validation with for loop
	- If favoriteFoods is defined and is an array
		- All entries must be strings
		
- Implement Day 2 Array Validation with .forEach()
- Implement Day 2 array validation with .filter()

### Requirements - Day 3 - Morning

- Implement Day 2 stretch goal with .forEach() or .filter() where appropriate
	- _Suggestion_: Create individual checks for each condition that you need to account for
- _Stretch Goal_:
	- Using query params in the GET /all and GET /single routes, implement the ability to only return certain fields on the blogPost
		- _Hint_: Array.map() can be used to edit all entries in an array in a single call

### In-Class - Day 3 - Afternoon

- Introduce express route error handling
	- Add try/catch blocks to all functions
- Refactor user post route to handle errors and test with throw Error

### Requirements - Day 3 - Afternoon

- Refactor all routes with error handling
- Test that all routes properly handle errors by throwing a test error
- _Stretch Goal_:
	- Add status codes to all res.json() method calls appropriate for the success or error being sent in the response
		- E.G. A successful response would have a status code of 200, an internal server error would respond with 500
		- _Suggestion_: I would google 'ExpressJS set status code'


Message lecture-notes








Shift + Enter to add a new line
