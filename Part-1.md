### Express Setup :
```js
//Express setup
//1.npm init -y 2.npm i express 3.npm i -g nodemon 4.npm i cors

const express = require("express");
const app =express()
const cors = require('cors')
const FakeData=require("./FakeData.json")
const port = 3000;
app.use(cors())
app.get("/",(req, res)=>{
  res.send("Hello world my name is raihan")
})
//all data show json
app.get("/user",(req,res)=>{
  res.send(FakeData)
})
//Individual data show
app.get("/user/:id",(req,res) => {
  const id =parseInt(req.params.id);
   console.log("show id",id);
   const singleUser=FakeData.find(data=>data.id === id) || {};
   res.send(singleUser)
})
app.listen(port,()=>{
  console.log("Server is runing",{port});
})
```

### Server 
```js
//
const coffeeData=["mum","rakib","rifat"]
fetch(`https://github.com/raihan51674`,{
method :"POST",
headers : {
"Content-Type":"application/json",
},
body:JSON.stringify(coffeeData)
})
.then(res=>res.json())
.then(data=>console.log(data))


//server use
app.use(express,json())
app.post
```










