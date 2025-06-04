
### 1. Get Method :
```js

const MyComponent = () => {
  const [data, setData] = useState([]); 
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get('http://localhost:5000/api/items');
        setData(response.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h2>Items from Backend:</h2>
      <ul>
        {data.map((item, index) => (
          <li key={index}>{item.name}</li> 
        ))}
      </ul>
    </div>
  );
};

```

### 3.Post Method :
```js

const PostComponent = () => {
  const [name, setName] = useState('');
  const [responseMsg, setResponseMsg] = useState('');
  const [error, setError] = useState(null);

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      const response = await axios.post('http://localhost:5000/api/items', {
        name: name,
      });

      setResponseMsg(`Success: ${response.data.message || 'Item added'}`);
      setName(''); 
      setError(null);
    } catch (err) {
      setError(err.response?.data?.message || err.message);
      setResponseMsg('');
    }
  };

  return (
    <div>
      <h2>Add Item</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Enter item name"
          value={name}
          onChange={(e) => setName(e.target.value)}
          required
        />
        <button type="submit">Submit</button>
      </form>

      {/* Message section */}
      {responseMsg && <p style={{ color: 'green' }}>{responseMsg}</p>}
      {error && <p style={{ color: 'red' }}>Error: {error}</p>}
    </div>
  );
};

```

### 3.put Method :
```js

import React, { useState } from 'react';
import axios from 'axios';

const UpdateComponent = () => {
  const [id, setId] = useState('');
  const [newName, setNewName] = useState('');
  const [responseMsg, setResponseMsg] = useState('');
  const [error, setError] = useState(null);

  const handleUpdate = async (e) => {
    e.preventDefault();

    try {
      const response = await axios.put(`http://localhost:5000/api/items/${id}`, {
        name: newName,
      });

      setResponseMsg(`Success: ${response.data.message}`);
      setError(null);
      setId('');
      setNewName('');
    } catch (err) {
      setError(err.response?.data?.message || err.message);
      setResponseMsg('');
    }
  };

  return (
    <div>
      <h2>Update Item</h2>
      <form onSubmit={handleUpdate}>
        <input
          type="text"
          placeholder="Enter item ID"
          value={id}
          onChange={(e) => setId(e.target.value)}
          required
        />
        <input
          type="text"
          placeholder="Enter new name"
          value={newName}
          onChange={(e) => setNewName(e.target.value)}
          required
        />
        <button type="submit">Update</button>
      </form>

      {/* Message section */}
      {responseMsg && <p style={{ color: 'green' }}>{responseMsg}</p>}
      {error && <p style={{ color: 'red' }}>Error: {error}</p>}
    </div>
  );
};

export default UpdateComponent;
```


### 4.Delete Method :
```js
import React, { useState } from 'react';
import axios from 'axios';

const DeleteComponent = () => {
  const [id, setId] = useState('');
  const [responseMsg, setResponseMsg] = useState('');
  const [error, setError] = useState(null);

  const handleDelete = async (e) => {
    e.preventDefault();

    try {
      const response = await axios.delete(`http://localhost:5000/api/items/${id}`);

      setResponseMsg(`Success: ${response.data.message}`);
      setError(null);
      setId('');
    } catch (err) {
      setError(err.response?.data?.message || err.message);
      setResponseMsg('');
    }
  };

  return (
    <div>
      <h2>Delete Item</h2>
      <form onSubmit={handleDelete}>
        <input
          type="text"
          placeholder="Enter item ID"
          value={id}
          onChange={(e) => setId(e.target.value)}
          required
        />
        <button type="submit">Delete</button>
      </form>

      {/* Message section */}
      {responseMsg && <p style={{ color: 'green' }}>{responseMsg}</p>}
      {error && <p style={{ color: 'red' }}>Error: {error}</p>}
    </div>
  );
};

export default DeleteComponent;

```
