# How to connect database in MERN with debugging

- Go to [MongoDB](https://www.mongodb.com/products/platform/atlas-database)
  - check Pricing : - shared 
- sign in ; put organisation Name as your name
## Let's Start 
- New Project : say BackendProject
- project owner : default
- create a Deployment : `+create` and configure the deployment :-
  - select : `M0`(free)
  - Provider :  AWS
  - Region : Mumbai
  - and click  create 
- Go to Security : Quickstart 
   - username and password
   - ip address:  `0.0.0.0/0`  : allow access from anywhere
   - if missed; go to security and go to Db access (Role: Read and Write to any DB) and Network access to change
- Go to Deployment -> DataBase and click ->connect
- Select Compass and copy the url ; remove slash(/) while pasting at "url" in the following:-
- Now go to _.env_ file and write 
```
PORT = 8000
MONGODB_URI =  yourUrl 
```

- Now , name the database in the _src/constants.js_ file
```javascript
export const DB_NAME = "videotube"
```

## Database connection Approaches 

#### Approach 1
include all the code in the _index.js_ and write function specifying the connection and execute the function as soon as the _index.js_ file is loaded

#### Approach 2
writing the code in DB folder and import it in _index.js_ file and execute
- distributed and modular , organised code :- Professional Approach

## Packages needed
1. express
2. dotenv
3. mongoose 
- install by running `npm i mongoose express dotenv` in terminal  and check _package.json_ to see if all the dependencies are installed 

## Master $Protip💡$:
1. There wil be always some problem with connection with database ; so we need to use 
- promises
- try catch 
2. Database is always in another continent i.e it takes time to fetch data so we have to use
- async await


## Code 
### (Approach1 )
- in _index.js_
- code snippet :-
   - iffi function Semicolon(;) included before writing as an standard practice
   - `await mongoose.connect("url of database")`
```javascript 
import mongoose from "mongoose";
import {DB_NAME} from "./constants";

import express from "express"
const app = express()
;( async() => {
    try {
        await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        app.on('error', (error) => {
            console.log("error : ", error)
            throw error
        })
        app.listen(process.env.PORT, () => {
            console.log(`Example app listening on port ${process.env.PORT}`)
        })
    } catch (error) {
        console.log("error : ", error)
        throw error
    }
})()
```
- comment down the code 
### (Approach 2) ; followed here
- Make a new file _index.js_ in DB folder
- code snippets : -
  -  `process.exit(1)` give a read ; process is a reference to the current process of the running apllication 
  -  observe the output of 
  ```console.log(` ${connectionInstance}`);```

- code part to be writen in _src/db/index.js_ is
```javascript
import mongoose from "mongoose";
import {DB_NAME} from "../constants.js";


const connectDB = async () =>{
    try {
        const connectionInstance = await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`)
        console.log(`\n MongoDB Connected !! DB HOST: ${connectionInstance.connection.host}`);
    } catch (error) {
        console.log("MONGODB CONNECTION error : ", error);
        process.exit(1)
    }
}

export default connectDB
```

- code part to be writen in _src/index.js_ is
```javascript

import dotenv from "dotenv";
import connectDB from "./db/index.js";

dotenv.config(
    {path: '/.env'}
);

connectDB()
```
- code snippets:-
  - have to include _dotenv_ as early as possible ;
  - `require('dotenv').config(   
    {path: __dirname + '/.env'}
)` write at the top 
  - Issue ; it reduces the consitency : The require vs import 
  - Solution :- in _src/index.js_
```javascript
import dotenv from "dotenv";
dotenv.config(
    {path: '/.env'}
);
```
- As it is not launched as official ; we have to make changes in _package.json_ 
```javascript
 "scripts": {
    "dev": "nodemon -r dotenv/config --experimental-json-modules src/index.js"
  },
```
- run the server using : - `npm run dev`
and output should appear like :-  
>MongoDB Connected !! DB HOST: ac-xxxxxxxxx-.mongodb.net
- $Protip💡$:Possible error could be due to improper import 
like
`import connectDB from "./db/index.js";` here extension (.js) should be mentioned 


