# MERN-Stack-Project-Implementation

# MERN Web stack consists of following components:

- MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
- ExpressJS: A server-side Web Application framework for Node.js.
- ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
- Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

# Preparing prerequisites =>
Launch an instance in the cloud using Ubuntu's package manager.
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/mern%20aws.PNG)

STEP 1 - BACKEND CONFIGURATION
Update ubuntu

```
sudo apt update
```

Upgrade ubuntu

```
sudo apt upgrade
```

Let’s get the location of Node.js software from Ubuntu repositories

```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

Install Node.js on the server
Install Node.js with the command below

```
sudo apt-get install -y nodejs
```

Verify the node installation with the command below

```
node -v 
```

Verify the node installation with the command below

```
npm -v
```

Application Code Setup
Create a new directory for your To-Do project:

```
mkdir To-Do
```

Run the command below to verify that the Todo directory is created with ls command

```
ls
```

Now change your current directory to the newly created one:

```
cd Todo
```

You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

```
npm init
```

![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/todo%20npm%20init.PNG)

Run the command ls to confirm that you have package.json file created.
Next, we will Install ExpressJs and create the Routes directory.

# INSTALL EXPRESSJS

To use express, install it using npm

```
npm install express
```

Now create a file index.js with the command below

```
touch index.js
```

Run ls to confirm that your index.js file is successfully created
Install the dotenv module

```
npm install dotenv
```

Open the index.js file with the command below

```
vim index.js
```

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

```
const express = require('express');
require('dotenv').config();
 
const app = express();
 
const port = process.env.PORT || 5000;
 
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
 
app.use((req, res, next) => {
res.send('Welcome to Express');
});
 
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

```

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

```
node index.js
```

If everything goes well, you should see Server running on port 5000 in your terminal.
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/node%20index.js)

Now we need to open this port in EC2 Security Groups. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000.

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000

```
http://<PublicIP-or-PublicDNS>:5000
```
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/port%205000.PNG)

Routes
There are three actions that our To-Do application needs to be able to do:
Create a new task
Display list of all tasks
Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

```
mkdir routes
```

```
cd routes
```

Now, create a file api.js with the command below

```
touch api.js
```

Open the file with the command below

```
vim api.js
```

Copy below code in the file.

```
const express = require ('express');
const router = express.Router();
 
router.get('/todos', (req, res, next) => {
 
});
 
router.post('/todos', (req, res, next) => {
 
});
 
router.delete('/todos/:id', (req, res, next) => {
})
 
module.exports = router;

```

# MODELS
To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
Change directory back Todo folder with cd .. and install Mongoose

```
npm install mongoose
```

Create a new folder models 

```
mkdir models
```

Change directory into the newly created ‘models’ folder with

```
cd models
```

Inside the models folder, create a file and name it todo.js

```
touch todo.js
```

Open the file created with  ``` vim todo.js ``` then paste the code below in the file

```
const Schema = mongoose.Schema;
 
//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})
 
//create model for todo
const Todo = mongoose.model('todo', TodoSchema);
 
module.exports = Todo;
```

Now we need to update our routes from the file ``` api.js ``` in ‘routes’ directory to make use of the new model.
In Routes directory, open ``` api.js ``` with ``` vim api.js ``, delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');
 
router.get('/todos', (req, res, next) => {
 
//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});
 
router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});
 
router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})
 
module.exports = router;
```

# MONGODB DATABASE
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. Follow the sign up process, select AWS as the cloud provider, and choose a region near you.
Complete a get started checklist as shown on the image below
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/Mongo%20account.PNG)
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/Mongo%20Access.PNG)

Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/Mongo%20Network%20Access.PNG)

Create a MongoDB database and collection inside mLab
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/Mongo%20Collections.PNG)

In the ``` index.js ``` file, we specified ``` process.env ``` to access environment variables, but we have not yet created this file. So we need to do that now.
Create a file in your Todo directory and name it ``` .env ```.

```
touch .env
vi .env
```

Add the connection string to access the database in it, just as below:

```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/DB%20%3D%20vim.PNG)

Ensure to update ``` <username> ```, ``` <password> ```, ``` <network-address> ``` and ``` <database> ``` according to your setup
![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/cluster%20connect.PNG)

![](https://github.com/Adedoja/MERN-Stack-Project-Implementation/blob/main/MERN%20STACK/cluster%20connect%20to%20app.PNG)













































































































