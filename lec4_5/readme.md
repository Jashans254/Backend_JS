# Data Modelling for Backend using Mongoose

Question should be what to store ? and not where to store the data, its format and fields?

- ### Tools
  - Professionaly : Moon Modeler :- MongoDB , NoSQL ; fields are given and automatically code is generated
  - Free Tier : eraser.io 
    - use Diagram as a code > Entity Relationship 
    - Open editor: code is available

- ### Overview
  - How data will be saved ; say for Login /Register page; Visulise on pen/paper
    - Registeration Page fields
      -  username ; email ; password 
      -  $Protip💡$:only if there is another logic and how to store will all change so analyse all the required data need from user
      - dob ; photo 

## Let's Code
- ### Extra Tools 
  - no need to install packages; online sites
    - github codespaces
    - replit
    - stackblitz 
- ### go to stackblitz 
    - select new and express 
    - do `npm i mongoose`
    - $Protip💡$: maintain consistency ; standard followed nomenclature : - _ todo.models.js_

### Models
- #### File structure
  - todos
     - todo.models.js
     - user.models.js
     - sub_todos.models.js

- Boilerplate code ; say write in _user.models.js_
```javascript
import mongoose from 'mongoose'
const userSchema = new mongoose.Schema({
    // here all fields are defined
})
export const user User = mongoose.model("User" , userSchema)
```
  - $Protip💡$:"User" gets converted to lowercase and plural : users
  - write at line 2 : basic schema
```javascript
const userSchema = new mongoose.Schema({
   username: String , 
   email : String , 
   isActive: Boolean
})
```
   - Advanced schema ; unlocking the super power of mongoose
```javascript
const userSchema = new mongoose.Schema({
   username: {
    type : String ,
    required : true  ,
    unique : true , 
    lowercase : true
   } , 
   password  :{
    // as reqd
   } , 
})
```
  - Custom error messages : -
     - mini example: 
     ```javascript
     eggs: {
        type : Number , 
        min : [ 6 , `Must be atleast 6 got {value}`]
     }
     ```
     - use in _user.models.js_
```javascript
const userSchema = new mongoose.Schema({
   username: {
    //as reqd
   } , 
   password  :{
    type : String ,
    required : [true  , "This field can not be empty"] ,
    unique : true , 
    lowercase : true
   } , 
})
```
- timestamps 
  - `{timestamps: true }` will create two fields createdAt  and updatedAt automatically 
```javascript
const userSchema = new mongoose.Schema({
   username: {
    //as reqd
   } , 
   password  :{
   // as reqd 
   } , 
   } , {timestamps: true })
```
- reference to other model
```javascript
type: mongoose.Schema.Types.ObjectId,
ref: 'Hospital',
```
in _sub_todos.js_
```javascript
createdBy:{
type: mongoose.Schema.Types.ObjectId,
ref: 'User'
}
```
- restricting choices
  - in _order.models.js_ file
```javascript
Status:{
   type:String , 
   enum : ["PENDING" ,  "CANCELLED" , "DELAYED"]
}
```

- $Protip💡$:instead of giving links of files like images , video (bulky data/buffer) , they are stored in a cloud service like cloudanary  , AWS
### Other Models 
- Hospital Management 
- ecommerce

     
  






