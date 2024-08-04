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

