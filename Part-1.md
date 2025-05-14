### Frontend to post data
```js
//Frontend

const Users = () => {

  const handleAddUser=e=>{
    e.preventDefault()
    const name =e.target.name.value;
    const email=e.target.email.value;
    const newUser={name, email}
    console.log(newUser);
    //data patha in databes
    fetch("http://localhost:3000/users",{
      method:"POST",
      headers:{
        "Content-Type":"application/json"
      },
      body:JSON.stringify(newUser)
    })
    .then(res=>res.json()).then(data=>{
    console.log("Data patah complated",data);
    if(data.insertedId){
      e.target.reset()
    }
    })
  }
  return (
    <div>
        <form onSubmit={handleAddUser}>
          <input type="text" name='name' placeholder='Enter Name'/>
          <br />
          <input type="email" name='email' placeholder='Enter Email' />
          <br />
          <input type="submit" value="Add User" />
        </form>
      </div>
  );
};

//Server
const express = require('express');
const cors = require('cors');
const { MongoClient, ServerApiVersion } = require('mongodb');
const app = express();
const port = process.env.PORT || 3000;
// Middleware
app.use(cors());
app.use(express.json());
app.get("/", (req, res) => {
  res.send("Hello server is on");
});
// MongoDB connection
const uri = "mongodb+srv://mdraihan51674:8qxdd1PHK3oxJttE@cluster0.x0iekxk.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0";
const client = new MongoClient(uri, {
  serverApi: {
    version: ServerApiVersion.v1,
    strict: true,
    deprecationErrors: true,
  }
});
async function run() {
  try {
    await client.connect();
    console.log("✅ MongoDB connected");
    const userCollection = client.db("UserDB").collection("users");
    // POST route
    app.post("/users", async (req, res) => {
      console.log("📥 Received:", req.body);
      const newUser = req.body;
      const result = await userCollection.insertOne(newUser);
      res.send(result);
    });
  } catch (err) {
    console.error("❌ MongoDB connection failed:", err);
  }
}
run().catch(console.dir);
// Server listen
app.listen(port, () => {
  console.log(`🚀 Server is running on port ${port}`);
});

```
### Show data / Get all Data
```js
  const userData =use(FetchPromise)
  console.log(userData);
//show data
  const [user, setUser]=useState(userData)
    fetch("http://localhost:3000/users",{
      method:"POST",
      headers:{
        "Content-Type":"application/json"
      },
      body:JSON.stringify(newUser)
    })
    .then(res=>res.json()).then(data=>{
      const newUserData=[...user,newUser]
      setUser(newUserData)

 //data show
  {
            user?.map((data)=>{
              return <div key={data._id}>
                <h2 >{data.name}</h2>
                <h2 >{data.email}</h2>
              </div>
            })
          }
```



### Server + frontend (get and post and show)
```js
//Express setup
//1.npm init -y 2.npm i express 3.npm i -g nodemon 4.npm i cors
const express = require("express");
const cors = require("cors");
const app =express()
app.use(cors())
app.use(express.json())
 const port =3000
 app.get("/",(req,res)=>{
  res.send("Hello world")
 })
 const data=[
  {id:1, name:"sabina",email:"savina@gmail.com"},
  {id:2, name:"kabina",email:"kavina@gmail.com"},
  {id:3, name:"jorbina",email:"jorvina@gmail.com"},
 ]
 app.get("/user",(req,res)=>{
  res.send(data)
 })
 app.post("/user",(req,res)=>{
  console.log("post methoad");
  console.log(req.body);
  const newUser=req.body
  newUser.id = data.length + 1
  res.send(newUser)
  //data push
  data.push(newUser)
 })
 app.listen(port,()=>{
  console.log("server is run",{port});
  
 })


//Frontend
import { use, useState } from "react";

const User = ({fetchPromise}) => {
  const Fetchdata=use(fetchPromise)
  const [users, setUser]=useState(Fetchdata)

  const handleData=e=>{
    e.preventDefault()
    const name=e.target.name.value;
    const email=e.target.email.value;
    const user={name,email}
    console.log(user);
    //user create
    fetch("http://localhost:3000/user",{
      method :"POST",
      headers:{
        "Content-Type":"application/json"
      },
      body:JSON.stringify(user)

    }).then(res=>res.json())
    .then(data=>{
    const newUser=[...users,data]
    setUser(newUser)
    // e.target.reset()
  })
  }
  return (
    <div>
    <div>
      <form onSubmit={handleData} className="flex flex-col gap-3 border-2">
        <input type="text" name="name" placeholder="enter name"/>
        <input type="email" name="email"  placeholder="enter email"/>
        <input type="submit" value="Submit" />
      </form>
    </div>
     {
      Fetchdata.map((name)=><div key={name.id}>
       <h2>{name.name}</h2>
       <h3>{name.email}</h3>
      </div>)
     }
    </div>
  );
};

export default User;
```










