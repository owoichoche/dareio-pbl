# Project 4: MEAN stack deployment to Ubuntu website in AWS

In this tutorial, I will be deploying the MEAN stack to Ubuntu website in AWS.

The steps below show how I achieved this and I believe that you will find this useful

## Assumptions
- I assume that you already have an EC2 instance running in AWS.

- Connect to the EC2 instance via any SSH client or directly from your browser by following the steps outlined in the AWS documentation found here - https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-2-connect-to-instance.html

- Continue with the steps below

## STEP 1 - Install Node JS

- First, update Ubuntu: ``sudo apt update``

- Next, Upgrade Ubuntu: ``sudo apt upgrade``

- Finally, install NodeJS:  ``sudo apt install -y nodejs``

## STEP 2 - Install MongoDB
For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

Enter the following commands:

``sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6``

``echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list``

### Install MongoDB

Install MongoDB, Start the Server and verify that the service is up and running

![image](https://user-images.githubusercontent.com/36407447/118979238-2ac59680-b970-11eb-8ffe-06c5e44f157c.png)

### Install Node package manager.

``sudo apt install -y npm``

### Install *body-parser* package

The **body-parser** package is needed to help us process JSON files passed in requests to the server.

``sudo npm install body-parser``

### Create a folder named *Books*

``mkdir Books && cd Books``

### In the Books directory, Initialize npm project

``npm init``

### Add a file to it named server.js

![image](https://user-images.githubusercontent.com/36407447/118979853-d40c8c80-b970-11eb-9b6f-d8953f98c910.png)

## STEP 3 - Install Express and set up routes to the server

``sudo npm install express mongoose``

### In ‘Books’ folder, create a folder named apps

``mkdir apps && cd apps``

### Create a file named ``routes.js``

``vi routes.js``

### Copy and paste the code below into routes.js

``var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};``

### In the ‘apps’ folder, create a folder named ``models``

``mkdir models && cd models``

### Create a file named ``book.js``

``vi book.js``

### Copy and paste the code below into ‘book.js’

``var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);``

## STEP 4 - Access the routes with AnguarJS

### Change the directory back to ‘Books’

``cd ../..``

### Create a folder named public

``mkdir public && cd public``

### Add a file named ``script.js``

``vi script.js``

### Copy and paste the Code below (controller configuration defined) into the script.js file.

``var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});``

### In ‘public’ folder, create a file named ``index.html``

``touch index.html``

### Change the directory back up to ‘Books’

cd ..

### Start the server by running this command and you get the output as shown below:

``node server.js``

![image](https://user-images.githubusercontent.com/36407447/118987530-403ebe80-b978-11eb-9100-c36932146b55.png)

