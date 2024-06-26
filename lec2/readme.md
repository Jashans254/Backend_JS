# Backend Code in Production 

## ✅check Requirements 
Open Terminal and type 
- `node -v`   _Prefered 18 or higher_
- `npm - v`

## Understanding Working 

     Computer / Mobile -> request -> [Express] -> Server 

     Computer / Mobile <- [Express] <- Response Server 

- #### Task :- 
  - Go to any url say [https://www.postman.com/](https://www.postman.com/) ; what happens here is 
  - server **Listens** to the request , and server decides **response** 
  - All the listening work is done by package $Express$ 

  - Route terms :- 
      - visiting [https://www.postman.com/](https://www.postman.com/) means you are visting home route ; represented by `/`
      - visting [https://identity.getpostman.com/signup](https://identity.getpostman.com/signup)means you are visting `/signup` , entering user details setup

   - Types of request:-
       - $GET$ : basic requesting ; visting any url ;
       to get the service from the server of that url website

## Start Project

- #### Make a new folder say ***Chaideploy*** in VSCode and open terminal
  - Make an empty Node application . How ?
    - simply `npm init` in terminal 
      - $💡ProTip$ :- do `npm init -y` only when yes to all defaults
    - observe the output
  - fill the needed ; leave the rest to default by just clicking ENTER to next
     - entry point :(index.js)
     - test command : 
     - git repository:
     - keywords : node chai
     - author : YourName(anything)
     - license:(ISC)
  - You will get a summary of all filled details ; 
  - Now check the automatically generated file : $package.json$

- #### Make a new file $index.js$
  - to test write 
     ```javascript
     console.log("Hello World")
     ```
  - run by typing `node index.js` in terminal 

- #### But How to run using package.json ?
  - use $Scripts$ commands 
  - go to _package.json_ and modify the code for scripts 
     ```javascript
     "scripts" :{
        "start" : "node index.js"
     }
     ```
     - $💡ProTip$ : - common approaches are build , dev
  - But will it do ; when we run  `npm run start` 
     - actually command run will be `node index.js`
     - observe that terminal terminates after running the file 

- #### Installing $Express$
  - Go to official docs of express ; search in browser 
     - simply run `npm install express` in terminal 
  - check _package.json_ if installed 
  - visit getting started , Hello world section in docs

- ### Code part 
    To be written in _index.js_ file

    ```javascript 
    const express = require('express')
    const app = express()
    const port = 3000 
    app.get('/' , (req , res) =>{
        res.send("Hello World")
    })

    app.listen(port , ()=>{
        console.log("Example app listening to port ${port}")
    })
    ```

- #### Code Explaination
1.   ```javascript 
     const express = require('express')
     ```
     - Style of writing : common vs modulejs
     - importin package ; use as it is 
2.  ```javascript 
    const app = express()
    ```
     - app is a object  like _Math_ in JS ; we use Math.random() 
3.  ```javascript 
    const port = 3000 
    ```
    - There are about 65K + available virtual ports that are free to use 
    - listening to server
    - Not fixed ; can use 2000 or 4000 too 
4. ```javascript 
    app.get('/' , (req , res) =>{
        res.send("Hello World")
    })
    ```
    - `/` : Denotes Home Route 
    - At this home route listen to the request , we will give a callback , sending message using `res.send()` :- response
    - can we handle more requests like these? Yes
      - ```javascript 
        app.get('/twitter' , (req , res) =>{
        res.send("Hello World")
        })
        ```
      - `'/twitter' `  :- it is route 
      - `(req , res) =>{}` : - callback

5.  ```javascript 
        app.listen(port , ()=>{
        console.log("Example app listening to port ${port}")
    })
    ```
    if succesfully listening we get a  this Message response in terminal 

- ### Run The application
  - run in terminal `npm run start`
  - 📝: Terminal didnt terminated ; means that app is listening to the requests constantly
  - 🎉 Congrats!! **You made a server**
  - How to visit the url 
     - go to ***http://localhost:3000*** or 127.0.0.1:300
     - depending on the localhosting url of pc

- ### Make More 
  - ```javascript
    app.get('/login' , (req , res)=> {
      res.send('<h1>Please Login </h1>)
    })
    ```
  - write in _index.js_ file
  - run at ***http://localhost:3000/login***
  - **Problem** occurs ; **cannot GET/login** at the page ,What and how ?
  - Concept of Hot Reloading ;in react app , every change is saved and reloaded automatically But here we have to reload manually 
  - **Solution** restart the application by :- 
     - ctrl+c/break in terminal 
     - `npm run start` again

- ### One More 
  - ```javascript
    app.get('/youtube' , (req , res)=> {
      res.send('<h2>Welcome to my yt channel</h2>)
    })
    ```
  - restart the app and see the output

## Production Deployment

We get some special variables which need to be taken care of ; like sensetive info , Db username .Also the port assigned in our pc works fine but who knows it is free or not on client 
 
 - install dot env ; go to Docs 
 - `npm i dotenv` run in terminal 
 - Make a new file _.env_ and write`PORT= 3000` 
   - standard practice to write in capitals
 - How to use ; 
    - At the start of _index.js_ add  at the top
        - `require('dotenv').config()`
    - Also change 
      - ```javascript
        app.listen(process.env.PORT , ()=>{
        console.log("Example app listening to port ${port}")
        })
        ```
  - run the application and see the changes

## Deployment 

### Git and Github
- Go to github 
   - create a new repositary say _chaibackend_
   - Add description
   - ✅ public and create
- if not done yet , do git ssh setup to configure your pc
- In terminal run the following commands
  - `git init`
  - $💡ProTip$ : Make a _.gitignore_ file and add 
      - node_modules 
      - .env
  - `git add .`
  -  $💡ProTip$ : Way to visulize , git section left sidebar
  - `git commit - m "first commit"`
  - `git branch -M main`
  - `git remote add origin repo_link` : add repo_link created
  - `git push -u origin main`


### Devops
- #### Deployment Alternatives

     #####  ***DigitalOcean , Heroker , railway , seanode , render , cyclic***







