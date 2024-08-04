# Complete guide for router and controller with debugging 
Logic Building :- spilting a big problems into smaller problems and solving them one by one
- some by working on real world projects [80%]
- some by leetcode problems

## Let's Start

- Make a new file _user.controller.js_ in the controlller folder of src folder 
  - let's code 
    - import required methods like asyncHandler
    - write the method 
      - this method is doing nothing but sending a ok response with 200 status code
```js
import { asyncHandler } from "../utils/asyncHandler.js";


const registerUser = asyncHandler( async (req , res)=>{

    res.status(200).json(
        {
            message:"ok"
        }
    )
})
export {registerUser}
```

- Making routes
  - when will this method registerUser will be called , it will be called when a certain url is hit 
  - that url route is defined here
  - Go to routes folder and make a file _user.routes.js_
    - just making a router object and exporting 
```js
import { Router } from "express"

const router  = Router()

export default router 
```

- Go to app.js now and write
  - so import the route and 
  - `app.use("/api/v1//users" , userRouter)` states that when url hit at http://localhost:8000/api/v1/users it will go to userRouter and see the routes in that 
```js
//routes import
import  userRouter from './routes/user.routes.js'


// routes declaration
app.use("/users" , userRouter)
export { app }
```

- now define the routes in the userRoutes
  - import the function to be called 
   - now if the client hits the url 
     - http://localhost:8000/api/v1/users/register  , it will call registerUser function 
     - http://localhost:8000/api/v1/users/register ,  it will call loginUser function 
     - so we made a separate file for user as prefix in the url and all other possible 
```js
import { Router } from "express"
import { registerUser } from "../controllers/user.controller.js"

const router  = Router()

router.route("/register").post(registerUser)
router.route("/login").post(loginUser)
```

- Tools to test API 
  - thunder client : vscode extension
  - postman : standard api testing tool

- Download Postman and do basic sign in setup 
   - Make a new collection say "backendchai" and a new request
   - send a post req at "http://localhost:8000/api/v1/users/register"
   - output should be 
   - {
    "message": "ok"
}