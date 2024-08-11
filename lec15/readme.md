# Access Refresh Token , Middleware and cookies

- ## Let's start
  - two tokens:- access token and refresh token
     - access: short lived
       - authenticated user; like login , acess feature ; file upload 
       - login session 15 min 
     - refresh: long lived
       - stored in Db too , and user too
       - user is validated through access token
       but user do not  have to login again and again , so user hit a end point and 
       get a new access token on the basis of matching refresh token from user and db stored

- Login user
   - go to user.controller.js file
   - write the steps for login
```js
   // take data from req body 
    // check username / email
    // find the user
    // compare password
    // access and refresh tokens
    // send cookies [secure]
    // send response successfully
```

- // take data from req body 
```js
const {email , username , password} = req.body
```
- // check username / email
```js
if( !( username || email )){
        throw new ApiError( 400 , "username or email is required")
    }
```
- // find the user
```js
const user = await User.findOne(
         { $or: [ {email} , {username} ]} 
        )
```

- // compare password
```js
if( !user){
        throw new ApiError( 404 , "User not found")
    }
const isPasswordValid = await user.isPasswordCorect(password)

    if( !isPasswordValid){
        throw new ApiError( 400 , "Invalid password")
    }
```

- // access and refresh tokens 
we will call a function 
$Protip💡$: include keyword await where there is time constraint like an async function or mongo db function like user.save() or findById() etc
```js
const {accessToken , refreshToken} =  await generateAcessAndRefreshTokens(user._id)
``` 


```js
// we are taking user id as parameter and finding it from db and then calling tokens generating function from user model that were defined by us in the user.model.js file 
//then saving the refreshToken in db too
// as there are some fields like email that are required so to call save() function 
// we used a new parameter :- user.save( {validateBeforeSave : false} )
// so that no new issue is there while saving

// otherwise we are just throwing user not found custom error  
const  generateAcessAndRefreshTokens = async(userId)=>
    {
    try {
        
      const user =  await User.findById(userId)
      
      const accessToken = user.generateAccessToken()
      const refreshToken = user.generateRefreshToken() 

      user.refreshToken = refreshToken
      await user.save( {validateBeforeSave : false} ) 

      return {accessToken , refreshToken}
      
    } catch (error) {
        throw new ApiError( 500 , "Something went wrong while generating tokens")
    }
}
```

- // send cookies [secure] 

  - which cookie , what to send 
  - we can send user directly we got from 
```js
const user = await User.findOne(
         { $or: [ {email} , {username} ]} 
        )
```
   - But we got some unwanted fields like password that are not supposed to be sent
   - Also we got object from the above code and
   methods of generating token were called later on 
   - here , user variable is empty so we either
      - update the user object 
      - another query
   - We need to decide what is more optimized way (Best way)

   - We applied the filter and get all the required fields  (optional step)
```js
 const loggedInUser = await User.findById(user._id).
    select("-password -refreshToken ") 
```

   - When  we are sending cookies , we need to design some options [ object ]
     - by default cookies are modifiable by frontend 
     - so by enabling below options , only server can modify the cookie 
     - It is visible in frontend 
```js
const options = {
        httpOnly : true ,
        secure : true
    }
```


- // send response successfully
We have already configured the cookie parser in the app.js file 

   - we passed tokens in cookie as well as data field [Api response] because sometimes the user may need to store it in localstorage or may be working with mobile application
```js
return res.status(200).
    cookie("accessToken" , accessToken , options).
    cookie("refreshToken" , refreshToken , options).
    json(new ApiResponse( 200 ,
        {
            user : loggedInUser ,accessToken , refreshToken
        }
        , "User logged in successfully"))
```


## Logout 
- One thing we have to do is to remove the cookies
- Make a new file _auth.middleware.js_ in the middleware folder of src folder
- Code snippets
   - Go to postman and see the header section . Here ,we add the the key value pair as Authorization : Bearer [access token] as per jwt official if not in cookies
   - Then we will decode the token using jwt and find the user from db
```js
import { ApiError } from "../utils/ApiError";
import { asyncHandler } from "../utils/asyncHandler";
import jwt from "jsonwebtoken";
import {User} from "../models/user.model";
export const verifyJWT = asyncHandler( async (req ,_ , next) => {

  try {
     const token =  req.cookies?.accessToken || req.header("Authorization")?.replace("Bearer " , "") 
  
     if( !token){
         throw new ApiError( 401 , "Access denied. Login first")
     }
  
     const decodeddToken = jwt.verify ( token , process.env.ACCESS_TOKEN_SECRET )
  
     const user = await User.findById( decodeddToken?._id )
     .select("-password -refreshToken")
  
     if( !user){
      // disuss about frontend
         throw new ApiError( 401 , "Invalid token")
     }
  
     req.user = user
     next()
  } catch (error) {

    throw new ApiError( 401 , error?.message || "Invalid Access token")
    
  }
})
```

- Write the route in user.routes.js file
```js
// secured routes
router.route("/logout").post( verifyJWT ,logoutUser)

router.route("/login").post(loginUser)
```

- write in user.controllers.js
  - Code snippets
    - As we are getting the user from req.user from middlewaare , we can directly use the req.user._id to call the funciton
    - $set is used to update the field
    - new:true is used to return the updated document
    - clearCookie is used to clear the cookie
```js
const logoutUser = asyncHandler( async (req , res)=>{

    await User.findByIdAndUpdate(
        req.user._id ,
        {
            $set : { refreshToken : undefined }
        } ,
        {
            new:true
        }
    )

    const options = {
        httpOnly : true ,
        secure : true
    }

    return res
    .status(200)
    .clearCookie("accessToken" , options)
    .clearCookie("refreshToken" , options)
    .json(new ApiResponse( 200 , {}, "User logged out successfully"))
})
```


