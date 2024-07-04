# Professional Backend Project
All about the configuration needed before the start of a backend project.
- Check models [link](https://app.eraser.io/workspace/YtPqZ1VogxGy1jzIDkzj): - focus on Users 

## Let's Code 

- Make a folder _chaibackend_ 
  - intialise Node by `npm init`
  - keep the default
- Add _readme.md_ and add to github

- $Protip💡$: Images stored at Third Party service like AWS , Azure , Cloudnary 
   - temporaly stored at server ; so as if connection is lost ; user dont have to upload again

- $Protip💡$: Git can track empty folder so add _.gitkeep_ works opposite to .gitignore

- Make folder _public_ and folder  _temp_ in it ,  add _.gitkeep_ inside temp

- Make _.gitignore_ at root directory
   - gitignore generator ; search in browser
     [link](https://mrkandreev.name/snippets/gitignore-generator/) and search for node
   - copy paste in  _.gitignore_

- Make _.env_ at root directory

-  Make _src_ folder at root directory 
  - three files : app.js; index.js; constants.js

- change in _package.json_  `"type" : "module",`
- Reload if change in _index.js_ 
  - by --watch ; new Node feature
  - by nodemon ; followed here
    - install by ` npm i -D nodemon`
    - -D symbolises dev dependency instead of main dependency ; check docs official for more
   - change in _script.js_
```javascript
"Scripts" : {
   "dev" : "nodeman src/index.js"
}
```

- update github

- Make folders in _src_ folder
  - controllers
  - db
  - middleware
  - models
  - routes
  - utils

- $Protip💡$: Adding prettier plugin for an organised format
  - `npm i -D prettier`
  - Make two files 
    - _.prettierrc_ : holds configuration
```
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true ,
  "bracketSpacing" : true 
}
```
   - .prettierignore
```
*.env
.env
.env.*
/.vscode
/node_modules
./dist
```