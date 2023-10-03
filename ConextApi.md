# Context Api 
## There are two way to create Context Api, Both example works same.
## First Example in One File
### UserContext.js
```
import React, { useContext, useState } from 'react'

const UserContext = React.createContext();

export const useUserContext = ()=>{
    
    return useContext(UserContext);
}

export const ContextProvider = ({children}) => {

    const [user, setUser] = useState()

    return (
        <UserContext.Provider value={{user, setUser}}>
            {children}
        </UserContext.Provider>
    )
}
```

### App.js

```
import './App.css';
import Login from './components/Login';
import Profile from './components/Profile';
import { ContextProvider } from './context/UserContext';

function App() {
  return (
    <ContextProvider>
      <h1>Context Api</h1>
      <Login/>
      <Profile/>
    </ContextProvider>
  );
}

export default App;
```
### Login.js
```
import React, { useState } from 'react'; 
import { useUserContext } from '../context/UserContext';

function Login() {
    const [username, setUsername] = useState('')
    const [password, setPassword] = useState('');

    const {setUser} = useUserContext()

    const handleSubmit = (e ) => {
        e.preventDefault()
        setUser({username, password})
    }
  return (
    <div>
      <h2>Login</h2>
      <input type="text" value={username} onChange={ (e)=> setUsername(e.target.value)} placeholder='username'/>

      <input type="text" value={password} onChange={ (e)=> setPassword(e.target.value)} placeholder='password'/>
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
}

export default Login;
```
### Profile.js
```
import { useUserContext } from '../context/UserContext';

function Profile() {

    const {user} = useUserContext();

    if(!user) {
        return <div>Please Login</div>
    }
    
  return (
    <div>  welcome {user.username}  </div>
  );
}

export default Profile;
```

## Second Example in Separate Files
### UserContext.js
```
import React from 'react'

const UserContext = React.createContext();

export default UserContext;
```

### UserContextProvider.js
```
import React, { useState } from 'react'
import UserContext from './UserContext';  

const UserContextProvider = ({ children }) => {

    const [user, setUser] = useState()

    return (
        <UserContext.Provider value={{user, setUser}}>
            {children}
        </UserContext.Provider>
    )

}
export default UserContextProvider;
```
### App.js
```
import './App.css';
import Login from './components/Login';
import Profile from './components/Profile'; 
import UserContextProvider from './context/UserContextProvider';

function App() {
  return (
    <UserContextProvider> 
      <h1>Context Api</h1>
      <Login/>
      <Profile/> 
    </UserContextProvider>
  );
}

export default App;
```
### Login.js
```
import React, { useContext, useState } from 'react';
import UserContext from '../context/UserContext';

function Login() {
    const [username, setUsername] = useState('')
    const [password, setPassword] = useState('');

    const {setUser} = useContext(UserContext)

    const handleSubmit = (e ) => {
        e.preventDefault()
        setUser({username, password})
    }
  return (
    <div>
      <h2>Login</h2>
      <input type="text"
      value={username}
      onChange={ (e)=> setUsername(e.target.value)}
       placeholder='username'/>

      <input type="text" 
      value={password}
      onChange={ (e)=> setPassword(e.target.value)}
      placeholder='password'/>
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
}

export default Login;
```
### Profile.js
```
import React, { useContext } from 'react';
import UserContext from '../context/UserContext';

function Profile() {

    const {user} = useContext(UserContext) 

    if(!user) {
        return <div>Please Login</div>
    }
    
  return (
    <div>  welcome {user.username}  </div>
  );
}

export default Profile;
```