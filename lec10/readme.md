# How to upload file in backend | Multer

- sometimes the code part is put in utility or middleware , depending on our goal 
- reusable code , put separate  , used when needed 
  - register time needed photo
  - not during login 
- "jane se pehle mujhse mil ke jana" : A way to remember middleware

## Let's Start

### Toolkit
-  Cloudinary 
Uses wrapper on  AWS 
- express-fileupload: previously used or multer
we will  use multer  ; a good [Docs](https://github.com/expressjs/multer) 


- #### Cloudinary [link](https://cloudinary.com/)
Go to the site and login : sdk choosen is nodejs
   - Step 1: Installation by `npm install cloudinary`
 <!-- and `npm i multer` -->
  - Step 2:configuration 
  - step 3: upload
  - sample code ; for permission and upload file 
```js
import { v2 as cloudinary } from 'cloudinary';
cloudinary.config({ 
        cloud_name: 'xxxxx', 
        api_key: 'xxxxxxxx', 
        api_secret: '<your_api_secret>' // Click 'View Credentials' below to copy your API secret
    });
cloudinary.uploader
       .upload(
           'https://res.cloudinary.com/demo/image/upload/getting-started/shoes.jpg', {
               public_id: 'shoes',
           }
       )
```

- Strategy
Two step settings.
  - We will get a file uploaded by user through multer ,  cloudinary is a just a service SDK that will take the file and store it in their server 
  - Using Multer , we will get the file and store it temporarily in our local server 
  - next step , get file from local server and upload to cloudinary
  - done to ensure , we got the file from user once , if any error occurs , user need not upload it again.

## Let's code
some put code in service folder , some in the utility 
- Make a new file _cloudinary.js_ in utility folder 
  - step followed: we get a file path from our local server and upload to cloudinary 
  - once file succesfully uploaded , we have to remove it from local 
- we use fs [link](https://nodejs.org/api/fs.html): pre installled library file system of nodejs , that will handle file
  - write , read , remove , async/sync , permisson change 
  - $Protip💡$: Terminology used in OS , unlink instead of delete

- Go to _.env_ file and write credentials from cloudinary website
```js
CLOUDINARY_CLOUD_NAME = 
CLOUDINARY_API_NAME = 
CLOUDINARY_API_SECRET = 
```
- Go to _cloudinary.js_ and write
```js
import {v2 as cloudinary} from "cloudinary"
import fs from "fs" 
cloudinary.config({ 
    cloud_name: process.env.CLOUDINARY_CLOUD_NAME, 
    api_key: process.env.CLOUDINARY_API_NAME, 
    api_secret: process.env.CLOUDINARY_API_SECRET // Click 'View Credentials' below to copy your API secret
});
const uploadOnCloudinary = async (localFilePath) => {
    try {
        if(!localFilePath) return null 
        //upload the file on cloudinary
        const response = cloudinary.uploader.upload(localFilePath, {
            resource_type: "auto",})
        // file has been uploade succesfully
        console.log("File has been uploaded" , response.url);
        return response;
    } catch (error) {
        // remove the locally saved file as the upload operation failed
        fs.unlinkSync(localFilePath);
        return null
    }
}
export {uploadOnCloudinary}
```

- Make a new file _multer.middleware.js_ in middleware folder
  - $Protip💡$: use when you want unique name of each file ```const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9)
    cb(null, file.fieldname + '-' + uniqueSuffix)``` 
```js
import multer from "multer"
const storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, './public/temp') // where to store
    },
    filename: function (req, file, cb) {
      cb(null, file.originalname) // what should be the file name
    }
  })
export  const upload = multer({ storage ,})
```


- All this part was configuration and actuall use will be done through controllers 
