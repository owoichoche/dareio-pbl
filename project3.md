# Project 3: To-do application on MERN web stack

## Connected to EC2 instance via MobaXterm

Spun a new EC2 instance, downloaded and installed MobaXterm on my PC and connected to the instance via SSH on MobaXterm.

![image](https://user-images.githubusercontent.com/36407447/116005719-28398080-a600-11eb-9de5-80e9015c8fd9.png)

## Installed Node.js on the server

Installed Node.js by using the command - ***sudo apt-get install -y nodejs***

verified the node installation with ***node -v*** and ***npm -v***

![image](https://user-images.githubusercontent.com/36407447/116005748-3e474100-a600-11eb-9377-467dc54c8f65.png)

created a new directory called **Todo**, initialized the project using **npm init** which then created a new file called **package.json** as shown below

![image](https://user-images.githubusercontent.com/36407447/116005762-4bfcc680-a600-11eb-8daa-488e2de1fcd5.png)

## Installed Express JS
- installed Express JS by using the **npm install express** command

- created the **index.js** file

- installed the dotenv module by using the command **npm install dotenv**

- edited the index.js file and specified port 5000 to be used. 

- see results from running the **node index.js** command below:

![image](https://user-images.githubusercontent.com/36407447/116005778-5e770000-a600-11eb-9f5e-51e14f938c69.png)

on running the **http://<PublicIP-or-PublicDNS>:5000** from the browser, gives
  
![image](https://user-images.githubusercontent.com/36407447/116005797-6df64900-a600-11eb-8f91-7bf609fad5ff.png)

![image](https://user-images.githubusercontent.com/36407447/116005807-7484c080-a600-11eb-98d5-9c7784981390.png)

created a directory called **routes**

created a file called **api.js**

installed mongoose by using the **npm install mongoose** command 

created a new directory called **models**

created the file **todo.js** inside the models folder

## MongoDB

Created a MongoDB account and connected to the MongoDB database as shown below:
![image](https://user-images.githubusercontent.com/36407447/116005852-b1e94e00-a600-11eb-863b-4dddbdf5efd0.png)

## Tested Backend Code without Frontend using RESTful API

Downloaded and installed Postman for this purpose. 

Created the POST and GET operations using Postman 

POST
![image](https://user-images.githubusercontent.com/36407447/116005872-cdecef80-a600-11eb-846b-9d66f6c3d1b9.png)

GET
![image](https://user-images.githubusercontent.com/36407447/116005874-d47b6700-a600-11eb-9cd7-9c0291fe0800.png)

## Frontend creation

### Created a user interface for a Web client (browser) to interact with the application via API as follows:

ran the **npx create-react-app client** command in the **Todo** directory to create a new folder called **client**. see confirmation of the creation of the React client below:

![image](https://user-images.githubusercontent.com/36407447/116005890-e5c47380-a600-11eb-98a2-8e92e6e86dd4.png)

## Running the React App

installed **concurrently** which is used to run more than one command simultaneously from the same terminal window by using the following command: ***npm install concurrently --save-dev***

installed **nodemon** which is used to run and monitor the server. this was installed by using the command ***npm install nodemon --save-dev***

**Concurrently and Nodemon**

![image](https://user-images.githubusercontent.com/36407447/116005902-ef4ddb80-a600-11eb-9d8c-03ec49f77170.png)

edited the **package.json** file in the Todo folder

added the key value pair in the **package.json** file "proxy": "http://localhost:5000" so that it will be possible to access the application directly from the browser by simply calling the server url as shown below: 

![image](https://user-images.githubusercontent.com/36407447/116005915-fc6aca80-a600-11eb-817f-2a56857a4806.png)

## Created React components

- created a new folder called components

- in the components folder, created the following files: Input.js, ListTodo.js and Todo.js

- edited the Input.js file

- installed **Axios**, a Promise based HTTP client for the browser and node.js

- edited the ListTodo.js file

- edited the Todo.js file

- edited the App.js file

- finally, edited the index.css file

- and the result gotten is as shown below:
![image](https://user-images.githubusercontent.com/36407447/116005936-13112180-a601-11eb-8562-37172e418b45.png)
