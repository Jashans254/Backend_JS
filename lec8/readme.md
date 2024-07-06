# Cutom API response and error handling 

Taking the Code setup one step further :
  - How we define class for custom errors and custom api 
  - middlewares and packages 

## Let's Start

### All about configuration

- Open _src_ folder and _app.js_ file and start writing the following code 
```javascript
import express from "express"
const app = express()

export { app }
```
- $Protip💡$: we are calling `connectDB()` in _index.js_ file ; which is an asynch method ; so when it gets completed ; technically  a promise is returned 

- Go to _index.js_ file and append :-
  - this code shows that connection is succesful to DB , our app will start listening to the requests as a server
```javascript
connectDB()
.then( ()=>{
    app.listen(process.env.PORT || 8000 , () => {
        console.log(`Example app listening on port ${process.env.PORT}`)
    })
})
.catch((error) =>{
    console.log("MONGO db connection failed: ", error);
})
```

- Go to express documention and go to API reference section (5.x)  ; The section that we will mostly use are :- 
  - request
     - baseUrl , cookie , hostname , ip , body etc can be requested from user
     - third party package used : multer to accept file
     - Most studied:
        -  req.params() : any data received via url come via params eg: in url part you see "?search = "
        - req.body() : data can be in json or forms format ; we have to do some activites or configuration and our work is done 
        $Protip💡$:  bodyPaser is preincluded in latest versions
  - response 

- Two npm packages needed:-
  - cookie-parser 
  - cors: cross origin resource sharing settings 
    - $Protip💡$: When using middlewares  or doing some configuration setting , most of the time we use syntax : `app.use()` 
  - install them by `npm i cookie-parser cors`

- Let's use them 
  - go to _app.js_ file and write code
  - code snippets : 
    - `app.use(cors())` needed only to configure ; but more can be done as seen in docs `corsOptions` like defining origin : from which frontend comes
```javascript
import express from "express"
import cors from "cors"
import cookieParser from "cookie-parser"
const app = express()
app.use(cors())
export { app }
```

- lets modify  :- 
  - go to _.env_ file and append
    `CORS_ORIGIN=*`  ; the `*` signfies that it will accept request from any source
```javascript
app.use(cors({
    origin:process.env.CORS_ORIGIN ,
    credentials : true
}))
```
**Note** 
you can check whitelist options on docs;

before cookieoptions 

currently we are working on preparation of data , data can come from url , json , body , form , json-form ; there are some setting / restrictions like we can't allow unlimited data , otherwise crashing the server 

- Again in _app.js_ file we append ; to accept data filled in a form by user 
  - `express.json()` : means we are accepting json format
    - `limit:"16kb" : size defined
```javascript
app.use(express.json({limit:"16kb"}))
```
- $Protip💡$:Again , you see searching  **"hello world"** in chrome ; the url created is something like "https://www.google.com/search?q=hello+world&oq=Hello+world&gs_lcrp=EgZjaHJvbWUqDQgAEAAY4wIYsQMYgAQyDQgAEAAY4wIYsQMYgAQyCggBEC4YsQMYgAQyCggCEAAYsQMYgAQyBwgDEAAYgAQyBwgEEAAYgAQyDQgFEAAYgwEYsQMYgAQyBwgGEAAYgAQyBwgHEAAYgAQyCggIEAAYsQMYgAQyBwgJEAAYgATSAQgzODA4ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8"
  - notice the "hello+world" ; how it is encoded
  - %20 signfies space 
  - and many others

- So , to tackle above problem ,  in _app.js_ append 
  - `extended` : signfies nested url allowed
```javascript
app.use(express.urlencoded({extended:true , limit:"16kb"}))
```
- Again, in _app.js_ append 
  - to use folder _public_  that will contain assest like images ,favicon if needed
```javascript
app.use(express.static("public"))
```

- $Protip💡$: What about cookieParser 
  - The server will get access to user cookies and can set it too , perform CRUD operations on it ; secure cookies can be accessed by server only ; used everywhere in production

- Again , in _app.js_ append
```javascript
app.use(cookieParser())
```

### Middleware 


- #### Working 
  - hit an url "/instragram" , at server there is  a get function (req , res) to listen the request and say we send some req.send("Hello") ; this is how we handle simply 
  - Are you capable of requesting that page ;
  say you must be logged in to acess that info ; so we must include a check in between hitting the link url and listening the request 
  - What we do?
    - hit url "/instagram" then check if user is logged in ; mutlilayer checks too ;
    check if the req is done to user admin page 
    - checking is done as per code written in sequence
  - There are actually 4 paramater 
     - (err , req , res , next) 
     - when used _next_ ; it denotes a flag ; 
     when a middleware check is done with its task , it passes to next 

### Utilities 

#### Need 
- Need ? we have written code for connection of DB in db/index.js , but we need to use it at various places : user controller , video controller etc ;

#### Code 
- Create a new file _asyncHandler_ in src/db folder 
  - What does it do ?
  - It will create a method and export it 
- Code to be written : 
  - High order function
  - which can accept a function as parameter  or return a function 
  ```javascript
  const asyncHandler = () => {} // simple  function
  const asyncHandler = (func) => {() = > {} } // high order  , {} to understand
  const asyncHandler = (func) => async () = > {}  // with asynch
  ```
- Approach 1
  - sucess is symbol for frontend
```javascript
const asyncHandler = (fn) =>  async ( req , res , next) =>  {
    try {
        await fn(req , res , next)
    } catch (error) {
        res.status(error.code || 500).json({
            sucess : false,
            message : error.message
        })
    }
}

export {asyncHandler}
```

- Approach 2 
```javascript 
const asyncHandler = (requestHandler) => {
    (req , res , next) =>{
        Promise.resolve(requestHandler(req , res , next)).catch((err) => 
            next(err))
    }
}


export {asyncHandler}
```

#### Error class

- So , we create a generalised wrapper function ; we just pass the method in that wrapper ; industry common practice 
- we can standardise api response or error 
  like standarising 
```javascript
 res.status(error.code || 500).json({
            sucess : false,
            message : error.message
        })
```
- our codebase gets clean and format the response is shown is standard

- Study "nodejs api error" documentation
  - we get a whole error class 
  - concept of inheritence used ; overwriting 

#### Code Again 
- Make a new file _AppError.js_ in utils folder
```javascript
class ApiError extends Error {
    constructor(
        statusCode,
        message = "Something went wrong",
        errors =[] ,
        statck = "" // error stack
    ){
        super(message);
        this.statusCode = statusCode
        this.data = null // read this
        this.message = message
        this.success = false
        this.errors = errors

        if(statck){
            this.stack = statck

        }else{
            Error.captureStackTrace(this, this.constructor)
        }
    }
}

export {ApiError}
```

- Make a new file _AppResponse.js_ in utils folder
```javascript
class ApiResponse {
    constructor(statusCode, data, message = "Success") {
        this.statusCode = statusCode;
        this.data = data;
        this.message = message;
        this.success = statusCode < 400;
    }
}
export { ApiResponse}
```

- Give a read : http respomse status code
  - spec sheet / Memo is given at company that is to be followed