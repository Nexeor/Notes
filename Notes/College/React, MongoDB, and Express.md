**Tech Stack:**
- React: GUI framework to build the frontend UI
- Express Framework: Allows us to build a backend web server on top of Node
	- ```npx express-generator .```
- Node: JS runtime environment that allows us to run on a machine rather than a browser
	- Mongoose: JS library for MongoDB interaction
		- ```npm install mongoose```
	- CORS (Cross-Origin research sharing): Node module that allows for web apps to make requests to resources on different origins
		- Allows React to make requests to Express API (which is hosted on a different origin)
		- ```npm install axios```
		- ```npm install cors```
	- Axios: JS library that allows Node to make HTTP requests to express 
- MondoDB: Database used to store app data

Setting up Project: 
1) Create root directory
2) Install mongoose, express, cors, and axios:
	1) ```npm install mongoose```
	2) ```npm install express```
	3) ```npm install cors```
	4) ```npm install axios```
3) Create "front" and "back" directories
4) In "front" create the react app: ```npx create-react-app .```
5) In "back" create the express app: ```npx express-generator .```
