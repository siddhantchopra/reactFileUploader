# Run the following steps to setup the project from scratch

## To setup express server for file upload

1) npm init (to create package.json)
2) npm i express express-fileupload
3) npm i -D nodemon concurrently
4) Add scripts
`  "scripts": {
    "start": "node server.js",
    "server": "nodemon server.js",
    "client": "npm start --prefix client",
    "dev" : "concurrently \"npm run server\" \"npm run client\""
  }, `

5) make server.js file and add the following code
`const express = require('express')
const fileUpload = require('express-fileupload')

const app = express()

app.use(fileUpload())
// Upload Endpoint
app.post('/upload', (req,res)=>{
    if(req.files === null){
        return res.status(400).json({msg:'No file uploaded'})
    }
    const file = req.files.file
    file.mv(`${__dirname}/client/public/uploads/${file.name}`, err=> {
        if(err){
            console.error(err)
            return res.status(500).send(err)
        }
        res.json({
            fileName: file.name, filePath: `/uploads/${file.name}`
        })
    })
})
app.listen(5000, ()=> console.log('Server Started..'))`
6) npm run serve

## To setup react app, use following command

1) npx create-react-app <foldername>
2) npm i axios
3) add following line to package.json
`"proxy": "http://localhost:5000"`