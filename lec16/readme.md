# Access token and refresh token in Backend

- ## Let's start
  - Test Login in Postman
So go to postman and test for {{server}}users/login ; it is post method so it will give you a response with access token and refresh token.

  - Test Logout at {{server}}users/logout

- Need for refreshToken
  - accessToken expires after say 15 min , so after 15 min from frontend we hit the point of refreshToken and again get a new accessToken 

- Code snippets
  - we are comparing the refresh token from user and one stored in db
  - if matched we decode it and get user id
  - we generate new tokens and send them
```js
const refreshAccessToken = asyncHandler( async(req , res)=>{

     const incomingRefreshToken = req.cookies.refreshToken || req.body.refreshToken

     if( !incomingRefreshToken){
         throw new ApiError( 400 , "Unauthorized")
     }

    try {
         const decodedToken = jwt.verify(
            incomingRefreshToken ,process.env.REFRESH_TOKEN_SECRET ,)
    
        const user = await User.findById(decodedToken?._id)
    
        if( !user){
            throw new ApiError( 401 , "Invalid Refresh token")
        }
    
        if( incomingRefreshToken !== user?.refreshToken){
            throw new ApiError( 401 , "Expired Refresh token")
        }
    
        const options = {
            httpOnly : true ,
            secure : true
        }
    
        const {accessToken , newRefreshToken} = await generateAcessAndRefreshTokens(user._id)
    
        return res
        .status(200)
        .cookie("accessToken" , accessToken , options)
        .cookie("refreshToken" , newRefreshToken, options)
        .json(new ApiResponse( 200 , {
            accessToken , newRefreshToken
        } , "Access token refreshed successfully"))
    } catch (error) {
        
        throw new ApiError( 401 , error?.message || "Invalid Refresh token")
    }
})
```

