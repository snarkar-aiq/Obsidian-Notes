1. The component should log "User data fetched!" only when the `userId` prop changes. Why is it logging on every render? Find the error.
   
   ```tsx
   import React, { useState } from 'react';

	interface User {
	  id: number;
	  name: string;
	  email: string;
	}

	function UserProfile() {
	  const [user, setUser] = useState<User | null>(null);
	
	  const fetchUser = () => {
	    const newUser = {
	      id: 1,
	      name: 'John Doe'
	      // Missing email property
	    };
	    setUser(newUser); 
	  };
	
	  return (
	    <div>
	      <button onClick={fetchUser}>Load User</button>
	      <div>{user?.name}</div>
	    </div>
	  );
	}
   ```
2. 
   ```tsx
   import React from 'react';

	function SearchBox() {
	  const handleInputChange = (event: Event) => { 
	    console.log(event.target.value);
	  };
	
	  return (
	    <input 
	      type="text" 
	      placeholder="Search..."
	      onChange={handleInputChange} 
	    />
	  );
	}
   ```


## Backend PsudeoCode

```ts
function loginUser(username, password) {
    user = database.findUser(username);
    
    if (user && user.password === password) {
        return generateToken(user);
    } else {
        return error("Invalid credentials");
    }
}

function findUser(username) {
    // Returns user object OR null if not found
}
```