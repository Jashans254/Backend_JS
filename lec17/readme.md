# Writing update controllers for user | Backend with JS

- ## Let's start
  
  - ### Subscriptionsm model
   two things channel and subscribers( both are users)
```js
import mongoose , {Schema} from "mongoose";

const subscriptionSchema = new Schema({

    subscriber:{
        type:Schema.Types.ObjectId, // one who is subscribing
        ref:"User",
        required:true
    } ,
    channel:{
        type:Schema.Types.ObjectId,  // one who is subscribed
        ref:"User",
    }
} , 
{timestamps:true}
)

export const Subscription = mongoose.model("Subscription" , subscriptionSchema)
```

- ## Update controllers
   - to change password
   - code snippets 
       - get old and new password from user 
       - get user from middleware
       - check old password
       - save new password  ; hashing done at 
       at  pre hook in user model

```js
const changeCurrentPassword = asyncHandler( async(req , res)=>{

    const { oldPassword , newPassword } = req.body

    const user =  await User.findById(req.user?._id)

    const isPasswordCorrect =  await user.isPasswordCorrect(oldPassword)

    if(!isPasswordCorrect){
        throw new ApiError( 400 , "Old password is incorrect")
    }

    user.password = newPassword
    await user.save( { validateBeforeSave : false } )


    return res
    .status(200)
    .json( new ApiResponse( 200 , {} , "Password changed successfully"))
})
```

  - to get user
    - we already got user from middleware 
```js
const getCurrentUser = asyncHandler( async(req , res)=>{
    
    return res.status(200)
    .json(200 , req.user , "User fetched successfully")
})
```

  - to update user details 
    - we already got user from middleware
    - get the fields to update 
    - update the user
```js
const updateAccountDetails = asyncHandler( async (req  , res)=>{
    const { fullName , email } = req.body
    if( !fullName || !email){
        throw new ApiError( 400 , "fullName and email are required")
    }
    const user = User.findByIdAndUpdate(
        req.user?._id ,
        {
            $set : { fullName , email }
        } ,
        { new:true}
    ).select("-password ")

    return res.status(200)
    .json(new ApiResponse( 200 , user , "Account details updated successfully"))
})
```