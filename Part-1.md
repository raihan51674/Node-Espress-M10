## State manage :
```js
const initialdata = useLoaderData()
const [data, setdata]=useState(initialdata)
//map korbo {data.map}
//props kore pathai dibo another component data={data} setdata={setdata}
//....then(data=> ar niche
if(data.deletedCount){
const remaining=data.filter(item => item._id !== _id)
setdata(remaining)
}

```

### Server :
```js
const express = require('express');
const cors = require('cors');
const { MongoClient, ServerApiVersion, ObjectId} = require('mongodb');
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
    //Get route
    app.get("/users",async(req, res)=>{
      const cursor =userCollection.find()
      const users=await cursor.toArray()
      res.send(users)
    })
    //Delete route
    app.delete("/users/:id", async(req, res)=>{
      const id =req.params.id
      console.log(id);
      const query = {_id : new ObjectId(id)}
      const result= await userCollection.deleteOne(query)
      res.send(result)
    })
    //single data get
    app.get("/users/:id", async(req, res)=>{
      const id =req.params.id 
      const query ={_id : new ObjectId(id)}
      const result =await userCollection.findOne(query)
      res.send(result)
    })
    //update user
    app.put("/users/:id",async(req, res)=>{
      const id =req.params.id;
      const user =req.body;
      const filter ={_id : new ObjectId(id)}
      const UpdateDoc={
        $set :{
          name :user.name,
          email :user.email
        }
      }
    const options={upsert : true}
    const result =await userCollection.updateOne(filter, UpdateDoc, options)
    res.send(result)
    })
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

### Frontend :
```js
//main setup :
const router = createBrowserRouter([
  {
    path: "/",
    Component:Root,
    children:[
      {index:true, path:"/", Component:Home},
      {path:"/users/:id",
      loader: ({params})=>fetch(`http://localhost:3000/users/${params.id}`),
      Component:UsersDetails
    },
    {path:"/update/:id",
    loader:({params})=>fetch(`http://localhost:3000/users/${params.id}`),
    Component :UserUpdate
    },
    ]
  },
]);



//Create users
const CreateUser = ({FetchPromise}) => {
   const userData=use(FetchPromise)
    const [users, setUser]=useState(userData)
    console.log(users);
  const handleSubmit=e=>{
    e.preventDefault()
    const name=e.target.name.value;
    const email=e.target.email.value;
    const NewUser={name, email}
    console.log(NewUser);

    //create databse create
    fetch("http://localhost:3000/users",{
      method:"POST",
      headers:{
        "Content-Type" : "application/json"
      },
      body:JSON.stringify(NewUser)
    }).then(res=>res.json()).then(data=>{
     
      // console.log(data);
      if(data.insertedId){
      NewUser._id = data.insertedId
      const newUse=[...users, NewUser]
      setUser(newUse)
      e.target.reset()
      }
    })
  }
  return (
    <div>
      <h2>Create Users</h2>
      <form onSubmit={handleSubmit}>
        <input type="text" name='name' placeholder='Enter your name' />
        <input type="email" name='email' placeholder='Enter your email' />
        <div>
          <input type="submit" value="Add Users" />
        </div>
      </form>
      <div>
        <UsersAllDataShow users={users}></UsersAllDataShow>
      </div>
      
    </div>
  );
};

//Show data delete data

const UsersAllDataShow = ({users}) => {
  
  const [user, setUser]=useState(users)
  const handleDelate=(id)=>{
    console.log(id);
    fetch(`http://localhost:3000/users/${id}`,{
      method:"DELETE"
    }).then(res=>res.json()).then(data=>{
      console.log(data);
      if(data.deletedCount){
        const remaingUser=user.filter((data)=> data._id !== id);
        setUser(remaingUser)
      }
    })
  }
  return (
    <div>
     {
      user.map((data)=>{
        return <div key={data._id}>
          <h2> {data.name} </h2>
          <h2> {data.email} </h2>
          <div>
            <button onClick={()=>handleDelate(data._id)} >Delate</button>
            <Link to={`/users/${data._id}`}> Details page</Link>
            <Link to={`/update/${data._id}`}>Update User</Link>
          </div>
        </div>
      })
     }
    </div>
  );
};

//Details page
const UsersDetails = () => {
  const data =useLoaderData()
  console.log(data);
  
  return (
    <div>
      <h2>User Details Pages</h2>
      <div>
       <h2> {data.name} </h2>
       <h2> {data.email} </h2>
      </div>
    </div>
  );
};


//Update users

const UserUpdate = () => {
  const dataUpdate=useLoaderData()
  
  const handleUpdate=e=>{
    e.preventDefault()
    const name=e.target.name.value;
    const email=e.target.email.value;
    const newUpdate={name, email}
    console.log(newUpdate);
    
    fetch(`http://localhost:3000/users/${dataUpdate._id}`,{
      method:"PUT",
      headers:{
        "Content-Type": "application/json"
      },
      body:JSON.stringify(newUpdate)

    }).then(res=>res.json()).then((data)=>{
      console.log("Fronted update",data);
    })
  }
  return (
  <div>
    <form onSubmit={handleUpdate}>
        <input type="text" name='name' defaultValue={dataUpdate.name}/>
        <input type="email" name='email' defaultValue={dataUpdate.email} />
        <input type="submit" value="Update Users" />
      </form>
  </div>
  );
};

export default UserUpdate;



```












