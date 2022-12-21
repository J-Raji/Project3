# MERN STACK Implementation

## Update and Upgrade new Instance
`sudo apt update`
`sudo apt upgrade`
![Image of Update and Upgrade](upd-upg.PNG)

## NODE.JS install
`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`
![Image of NODE JS install ](nodejs.PNG)

## NODE.JS update
`sudo apt-get install -y nodejs`
![Image of Node JS update](nodjsup.PNG)

## Application of code-initial 
`mkdir Todo`
`ls`
`cd Todo`
`npm init`
-Click Enter and some other times
-Type Yes

![Image of application of code-npm initialization](initnpm.PNG)

## Express Js install
`npm install express`
![Image of express install](expressinst.PNG)

## create file index
`touch index.js`

## Install dotenv
`npm install dotenv`
![Image of dotenv](dotenv.PNG)

# Edit index file
`vim index.js`

>const express = require('express');
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

### to save
-type :w ,ENTER
-and :qa ,ENTER to exit vim

## Check port in terminal
-Open Terminal
-`cd Todo`
-`node index.js`
![Image of view port in terminal](chkport-term.PNG)

## Add inbound rule for
-Custom TCP
-Port 5000
-Description "Port 5000 for Project MERN "To-do" application deployment"
![Image of view Port 5000 active](port5000.PNG)

## View in browse
[http://3.92.213.228:5000]
![Image of view express](viewexp.PNG)

## Routes
There are three actions that our To-Do application needs to be able to do:
1. Create a new task
2. Display list of all tasks
3. Delete a completed task

## Blocker unable to connect to instance
- Created another inbound rule for ssh port 80
- Set inbound for HTTP on port 80
-set for inbound for ssh on port 22
![Image of Failed to connect to instance on terminal](blockerinb.PNG)

## create route direction and set to it
`mkdir routes`
`cd routes`
`touch api.js`
![Image of mkdir and cd routes](mkdir-cdR.PNG)

## Insert into api.js
`vim api.js`
>const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;


** save with ** 
:w ENTER and
:qa ENTER

![Image of Insert in api.js](insertapi.PNG)

## Create Models Directory
`cd Todo`

## Install Mongoose
`npm install mongoose`
![Image of Mongoose installed](mongoose.PNG)

## Create Models Directory
`mkdir models`
`mkdir models && cd models && touch todo.js`
`vim todo.js`

## insert into todo.js
>const mongoose = require('mongoose');
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

module.exports = Todo;`

**save with**
:w ENTER 
:qa ENTER

## Change directory from Todo to Routes
` cd ..`
`cd routes`
`vim api.js
- Delete contents with :%d ENTER
-update with

>const express = require ('express');
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


![Image of Modified api.js file](mod-apijs.PNG)

## MOBAXTERM New session
- Download and Install MobaXterm Home edition
- New SSH session
- Copy IP address
- Username
- Specify Primary Key (pick from download folder)

![Image of MobaXterm session established](mobaXterm.PNG)

## Backend Configuration
`sudo apt update`
`sudo apt upgrade`
![Image of running apt update and upgrade in MobaXterm](mod-apt.PNG)

## Location of Node.JS
`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

## Created Cluster
- Network configuration
- Database Access

## POSTMAN
-Installed
-Setup completed
-Api connection established with GitHub

## Blocker Error CONNECTREFUSED with POST request
-Unable to resolve

## FRONTEND CREATION

## Install React
`cd Todo`
` npx create-react-app client`
![Image of React Client](client.PNG)
## Install Concurrently
`npm install concurrently --save-dev`

## Install node monitor
`npm install nodemon --save-dev`
![Image of running react and node monitor](react-nodmon.PNG)

## Modify package.json
`vim package.json`
- type i ENTER
- select image script part
- right click to paste copied scripts to be replaced

>"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
  ---

  ## Configure Proxy
  `cd client`
  `vi package.json`
  -Insert
  `"key": {"Mykey3"},`
  `"proxy":{"5000"},`
-Run
`npm run dev`

## Creating React Components
`cd client`
`cd src`
`mkdir components`
`cd components`
`touch Input.js ListTodo.js Todo.js`
-Open Input.js file
`vi Input.js`
>import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
--

## Install Axios
`cd ..`
`cd ..`
`npm install axios`
![Image of Axios](axios.PNG)

## FRONTEND CONTD
`cd src/components`
-Open 
`vi ListTodo.js`
>import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
---
-open Todo.js
`vi Todo.js`
>import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
---
## Modify React.js
`cd ..`
`vi App.js`
-Copy and Paste below it
>import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
---
## Modify App.js
-Move to src directory
`cd..`
`vi App.js`
-copy and paste below the codes
>import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
---
-open App.css
`vi App.css`
-paste
..App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
---
-open index.css
`vim index.css`
-copy and paste below
>body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
---
-Open Todo directory
`cd ../..`
-run
`