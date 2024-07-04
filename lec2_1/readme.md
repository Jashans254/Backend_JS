# Connecting Frontend to Backend
 **fullstack ; proxy ; CORS ; tool chaining**

## Let's Start
- Make a new folder **FullStackBasic** and two folder inside
  -  backend
  - frontend
### Backend Part
- Click backend folder and open terminal 
  - Repeat the step **Start Project** of [lec2](/lec2/readme.md)
    - say here : entry name: _server.js_
    - `npm i express`
    - Make file  _server.js_
    - In package.json , Make start Script 
      - ```javascript 
         Scripts: {
            "start" : "node server.js"
         }
         ```
        
    - Open  _server.js_ file 
    - $ProTip💡$ : error cant import modules outside
        - common js -> require statement -> synch code
        - module js -> import statement -> asynch code
        - How To resolve this error
           - Go to package.json and add
           - ```javascript 
             "type" : "module"
             ```
    - Code to write in file:- 
      - ```javascript
        import express from 'express'
        const app = express()
        app.get('/' , (req, res)=>{
            res.send("Server is ready")
        })
        const port = process.env.PORT || 3000
        app.listen(port , ()={
            console.log(`Server is listening at ${port}`)
        })
        ```
      - **Code Discussion**
         - `const port = process.env.PORT || 3000`
         - process.env.PORT :- must for server
         - || 3000 : - either available for local
    - Run and Check the server at browser
    - Write more code
      - ```javascript
        // app.get('/', (req, res) => {
        //     res.send('Hello World');
        // })

        app.get('/api/jokes', (req, res) => {
            const jokes = [ 
                {
                    id: 1 , 
                    title : 'A joke',
                    content: 'A very funny joke',
                },
                {
                    id: 2 , 
                     : 'Another joke',
                    content: 'Another very funny joke',
                },  
                {
                    id: 3 , 
                    title : 'Yet another joke',
                    content: 'Yet another very funny joke',
                },
                {
                    id: 4 , 
                    title : 'Another joke',
                    content: 'Another very funny joke',
                },
                {
                    id: 5 , 
                    title : 'Yet another joke',
                 content: 'Yet another very funny joke',
             }
            ]
         res.send(jokes)
        })
        ```
        - our goal is display the jokes at frontend
        - $ProTip💡$ :- To professionally read API
          - go to json formatter whenever you get an API response
          - select tree structure mode
    - ### file structure
       - backend
         - node_modules
         - package-lock.json
         - package.json


### FrontEnd Part

$Protip💡$ : Bundlers/toolchains : help to build js files , understandable to browser; vite , parcel , create react app

#### Let's code
$Protip💡$ : rename the vscode terminal for better switching...
- Open the frontend folder and its terminal 
  - `npm create vite@latest .
    - $Protip💡$: Using ( .  ) at the end will make sure that all files will be made inside the frontend folder , and not in a new folder
  - select a framework : React 
  - select a varient : Javascript
  <!-- - Done now run `npm install` and `npm run dev` -->
  - Do `ls` and check _package.json_ file ; if yes then install npm by `npm i` type in terminal and hit enter
  - Now , `npm run dev` ; App run to serve 
     -  $Protip💡$:Remove all pre animations 
     - go to _src/App.jsx_ and write `<h1>This is a Heading<h1>` inside function App(); the code will look like the following :- 
```javascript
function App() {
  
  const [joke , SetJokes]  = useState([])


  return (
    <>
    <h1>Hello World</h1>

    <p>JOKES : {joke.length}</p>
    {
    joke.map((joke ,index) => (

      <div key={joke.index}>
        <h3>{joke.title}</h3>
        <p>{joke.content}</p>
      </div>
    ))
   }
    </>
  )
}
```
  - How we send an API request 
    - fecth ; Axios ; Reac query
  - `npm i axios` in terminal ; library used for web request; data loading and handling ; read axios npm and github : form parsing
  - write `import axios from 'axios'` and in function App() include this:-
  - ```javascript
    useEffect(() => {
    axios.get('/api/jokes')
    .then((res) => {
      SetJokes(res.data)
    }) 
    .catch((err) => {
      console.log(err)
    })

    } )
    ```
  - run again the App and inspect the change
  - Problem faced : CORS Policy 
    - What ? Provide security ; allow only origin based , as we might get request from many different urls 
  - Solutions : Whitelist the url / ip/domain ->done by backend developer ; search npm cors 
  -  $Protip💡$: Alternative ; Standard Practice 
     - instead of _/jokes_ we use _/api/jokes_ 
     - change in both App.jsx and server.js   
  - Concept of Proxy react ; search in browser 
  - Go to _vite.config.js_ file and allow every request starting from "api/ " ; code will look like ;
```javascript
export default defineConfig({
  server: {
    proxy: {
      '/api': 'http://localhost:3000',
    },
  } ,
  plugins: [react()],
})
```
- $Protip💡$ : no need to add dependency array [jokes] as shown; 
```javascript
{
    joke.map((joke ,index) => (

      <div key={joke.index}>
        <h3>{joke.title}</h3>
        <p>{joke.content}</p>
      </div>
    ), [jokes])
```


### Bad Practice 
-  pushed both front and back end folder to a service like digitalocean

- `npm run build`  run in terminal , we will get a _dist_ folder in frontend ; move it to backend and in _server.js_ "static assests" are served
- in middleware , added app.use(express.static('dist'))
- Restart the app now on backend and see the Magic
- Where is the issue?
  - everytime there is a change in frontend we have to delete previous dist folder and again repeat the process 
