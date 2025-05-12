



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










