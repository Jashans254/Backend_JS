# Logic Building | Register controller

- Go to user.controller.js
write the steps in comments 
---
```js
   // get user details from frontend
    // validation of user details - non empty - format of email etc
    // check if user  already exists: username , email
    // check for images , check for avatar
    //upload them on cloudinary,avatar
    // create user object
    // create user entry in DB 
    // send response , exclude password and token from response
    // check for user creation 
    // return res
    // check for error
    // send error 
```
- Let's code
 // get user details from frontend
  - Go to user.controller.js and write 
  - we can get data from `req.body` or as form or url or other format `req.params` or `req.query` or `req.files``req.headers`
  - log the data in console 
    - we will get data of console in terminal 
```js
const registerUser = asyncHandler( async (req , res)=>{
    const {fullName , email , username , password}=req.body

    console.log("email:" , email)
    res.status(200).json(
        {
            message:"ok"
        }
    )
})
```
- Now , to handle file go to user.routes.js and modify
  - import the upload middleware
  - and use function upload.fields
    - it takes an array of objects as input
    - each object contains the name of the field to be processed , and the maximum number of files to be processed
```js
import { upload } from "../middleware/multer.middleware.js"
const router  = Router()

router.route("/register").post(

    upload.fields([
        { name: "avatar", maxCount: 1 },
        { name: "coverImage", maxCount: 1 },
    ]),
    registerUser)
```

- now again go to user.controller.js and modify
 // validation of user details - non empty - format of email etc 
```js
// beginner
// if( fullName === ""){
    //     throw new ApiError( 400 , "fullname is required")
    // }

    // professional  ,some work as loop
    // and we check if field exists if yes then trim 
    // then again check empty
    if (
        [ fullName , email , username , password ].some( (field)=>field?.trim() === "")
    ){
        throw new ApiError( 400 , "All fields are required")
    }
```

- // check if user  already exists: username , email 
  - simply 
    - `User.findOne ( {email})` 
  - using operator : as we want either one field to be true
```js
const existedUser = await User.findOne ( {
        $or:[
            {email},
            {username}
        ]
    })
if(existedUser){
        throw new ApiError( 409 , "User with this email or username already exists")
    }
```

- // check for images , check for avatar
 - we get req.body as default from express
 - and req.files from multer

```js
const avatarLocalPath = req.files?.avatar[0]?.path
    const coverImageLocalPath = req.files?.coverImage[0]?.path

    if(! avatarLocalPath){
        throw new ApiError( 400 , "Avatar file is required")
    }
```
 
- //upload them on cloudinary,avatar
```js
const avatar = await uploadOnCloudinary(avatarLocalPath)
    const coverImage = await uploadOnCloudinary(coverImageLocalPath)

    if( !avatar){
        throw new ApiError( 500 , "Avatar upload failed")
    }
```

- // create user object
```js
const user = await User.create(
        {
            fullName,
            email,
            username : username.toLowerCase(),
            password,
            avatar : avatar.url ,
            coverImage : coverImage?.url || ""
        }
    )
```

- // create user entry in DB
- send response , exclude password and token from response
- check for user creation
```js
const createdUser = await User.findById(user._id).select(
        "-password -refreshToken"
    )
    if(!createdUser){
        throw new ApiError( 500 , "User creation failed")
    }
```

- return res ; we are using Apiresponse file 
```js
 return res.status(201 ).json(
         new ApiResponse( 200 ,createdUser , "User created successfully" ))
```
