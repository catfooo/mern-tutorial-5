##### new err happened!!!!!!



[1] Proxy error: Could not proxy request /api/users/login from localhost:3000 to http://localhost:5001.
[1] See https://nodejs.org/api/errors.html#errors_common_system_errors for more information (ECONNREFUSED).
[1] 


and this doesnt let me log in. it is rejected, giving me 500



##### msg from redux

type(pin):"auth/login/rejected"
payload(pin):"Request failed with status code 500"
email(pin):"tim@gmail.com"
password(pin):"123456"
requestId(pin):"aKY9j9lz4ugNgCtugjd6z"
rejectedWithValue(pin):true
requestStatus(pin):"rejected"
aborted(pin):false
condition(pin):false
message(pin):"Rejected"

##### msg from console

0] [nodemon] restarting due to changes...
[0] [nodemon] starting `node backend/server.js`
[1] Compiled successfully!
[1] webpack compiled successfully
[0] (node:26326) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
[0] (Use `node --trace-deprecation ...` to show where the warning was created)
[0] Server started on port 5002
[0] MongoDB Connected: ac-axovhog-shard-00-00.c00hmba.mongodb.net
[1] Compiling...
[1] Compiled successfully!
[1] webpack compiled successfully
[1] Proxy error: Could not proxy request /api/users/login from localhost:3000 to http://localhost:5001.
[1] See https://nodejs.org/api/errors.html#errors_common_system_errors for more information (ECONNREFUSED).
[1] 



###### what i tried to handle this


my fellow chatgpt said,

Update API Requests in React Code:
Now, in your React code, when making API requests, you can use relative URLs without specifying the full server address. For example, instead of:
javascript
Copy code
---------
const response = await axios.post('http://localhost:5001/api/users/login', userData);
---------
( i wasnt having this before though )


You can use:
javascript
Copy code
-------
const response = await axios.post('/api/users/login', userData);
-------
The development server will automatically forward requests to http://localhost:5001 as specified in the proxy field.


so i asked chatgpt where the hell i should have that and it answered

-------------
Usage in React Component:
Here's how it might look within a React component, such as in your Login.jsx file:
javascript
Copy code
// Import necessary dependencies
import axios from 'axios';

// ... other imports ...

function Login() {
  // ... other component code ...

  const onSubmit = async (e) => {
    e.preventDefault();

    const userData = {
      email,
      password
    };

    // Updated API request with a relative URL
    try {
      const response = await axios.post('/api/users/login', userData);
      // Handle response as needed
    } catch (error) {
      // Handle error
    }
  };

  // ... rest of the component code ...
}
--------

to have it here like this.
so i had it and having bunch of syntax error, finally managed to fix syntax,
which looks like this finally,




--------
Login.jsx

import {useState, useEffect} from 'react'
import { FaSignInAlt } from 'react-icons/fa'
import { useSelector, useDispatch } from 'react-redux'
import { useNavigate } from 'react-router-dom'
import { toast } from 'react-toastify'
import { login, reset } from '../features/auth/authSlice'
import Spinner from '../components/Spinner'
import axios from 'axios'

function Login() {
  const [formData, setFormData] = useState({
    email: '',
    password: '',
  })

  const { email, password } = formData

  const navigate = useNavigate()
  const dispatch = useDispatch()

  const { user, isLoading, isError, isSuccess, message } = useSelector((state) => state.auth)

  useEffect(() => {
    if(isError) {
      toast.error(message)
    }

    if(isSuccess || user) {
      navigate('/')
    }

    dispatch(reset())

  }, [user, isError, isSuccess, message, navigate, dispatch])


  const onChange = (e) => {
    setFormData((prevState) => ({
       ...prevState,
       [e.target.name]: e.target.value
    }))
  }

  const onSubmit = async (e) => {
    e.preventDefault()

    const userData = {
      email,
      password
    }

        // Updated API request with a relative URL
        try {
          const response = await axios.post('/api/users/login', userData);
          // Handle response as needed
          dispatch(login(userData))
        } catch (error) {
          // Handle error
        }
      }; 
  

  if(isLoading) {
    return <Spinner />
  }

  return (
  <>
    <section className="heading">
        <h1>
          <FaSignInAlt /> Login
        </h1>
        <p>Login and start setting goals</p>
    </section>
    
    <section className="form">
        <form onSubmit={onSubmit}>
            <div className="form-group">
            <input 
              type="email" 
              className="form-control" 
              id='email' 
              name='email' 
              value={email} 
              placeholder='Enter your email' 
              onChange={onChange} 
            />
            </div>  
            <div className="form-group">
            <input 
              type="password" 
              className="form-control" 
              id='password' 
              name='password' 
              value={password} 
              placeholder='Enter password' 
              onChange={onChange} 
            />
            </div>     
            <div className="form-group">
                <button type="submit" className='btn btn-block'>Submit</button>
            </div>      
        </form>
    </section>
  </>
  )
}

export default Login
-------
i moved dispatch(login(userData)) to inside of try block,
and regardless of syntax error, i think this might be biggest fix ive done.
idk what effect this will made, what i can say is this is what i got from chatgpt.


 and,...

##### err not solved. im still having same proxy error. 
but, this made me get rid of error that got from the browser, saying

------
Compiled with problems:
×
ERROR in ./src/pages/Login.jsx
Module build failed (from ./node_modules/babel-loader/lib/index.js):
SyntaxError: /Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/src/pages/Login.jsx: 'return' outside of function. (66:4)

  64 |
  65 |   if(isLoading) {
> 66 |     return <Spinner />
     |     ^
  67 |   }
  68 |
  69 |   return (
    at constructor (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:356:19)
    at FlowParserMixin.raise (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:3223:19)
    at FlowParserMixin.parseReturnStatement (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12995:12)
    at FlowParserMixin.parseStatementContent (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12621:21)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12588:17)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:5088:24)
    at FlowParserMixin.parseStatementListItem (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12568:17)
    at FlowParserMixin.parseBlockOrModuleBlockBody (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:13189:61)
    at FlowParserMixin.parseBlockBody (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:13182:10)
    at FlowParserMixin.parseBlock (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:13170:10)
    at FlowParserMixin.parseStatementContent (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12690:21)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12588:17)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:5088:24)
    at FlowParserMixin.parseStatementOrSloppyAnnexBFunctionDeclaration (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12578:17)
    at FlowParserMixin.parseIfStatement (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12989:28)
    at FlowParserMixin.parseStatementContent (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12619:21)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12588:17)
    at FlowParserMixin.parseStatementLike (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:5088:24)
    at FlowParserMixin.parseModuleItem (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12565:17)
    at FlowParserMixin.parseBlockOrModuleBlockBody (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:13189:36)
    at FlowParserMixin.parseBlockBody (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:13182:10)
    at FlowParserMixin.parseProgram (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12464:10)
    at FlowParserMixin.parseTopLevel (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:12454:25)
    at FlowParserMixin.parseTopLevel (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:5893:28)
    at FlowParserMixin.parse (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:14376:10)
    at parse (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/parser/lib/index.js:14417:38)
    at parser (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/core/lib/parser/index.js:41:34)
    at parser.next (<anonymous>)
    at normalizeFile (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/core/lib/transformation/normalize-file.js:64:37)
    at normalizeFile.next (<anonymous>)
    at run (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/core/lib/transformation/index.js:21:50)
    at run.next (<anonymous>)
    at transform (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/@babel/core/lib/transform.js:22:33)
    at transform.next (<anonymous>)
    at step (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/gensync/index.js:261:32)
    at /Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/gensync/index.js:273:13
    at async.call.result.err.err (/Users/catfood/mern-tutorial-5/mern-tutorial-4/frontend/node_modules/gensync/index.js:223:11)
ERROR
[eslint] 
src/pages/Login.jsx
  Line 66:4:  Parsing error: 'return' outside of function. (66:4)
  -------


  ##### but this is kind of fucked up the state.
  i cant see the rejected result when i try to log in. 
  before, i could see 
  ##### msg from redux

type(pin):"auth/login/rejected"
payload(pin):"Request failed with status code 500"
email(pin):"tim@gmail.com"
password(pin):"123456"
requestId(pin):"aKY9j9lz4ugNgCtugjd6z"
rejectedWithValue(pin):true
requestStatus(pin):"rejected"
aborted(pin):false
condition(pin):false
message(pin):"Rejected"


this from redux, but now, it is only showing 
(pin):undefined

with 
auth/reset at left side.
before, this side was showing it is rejected by this and that, 
but now i cant even see that. 


##### i need to revert!!!!!
but like, how? will pressing cmd z will take me to the state,
that i can generate right error with rejected login?
btw i am angry bcs i had useless time with taking care of syntax error 
(like things like {} or () or stuff like that, as you might know, it is rly annoying)
and got absoultely nothing out of it!!! i worked for nothing lol


luckly, i got the version that before i got before working with this horrible err,
and one more thing is... if i happened to validate this process,
i can go back and grab stuff from here without dealing with syntax error that chatgpt gave me.
likely not to be happen i guess...
anyway, i got the previous Login.jsx,

and might same error(proxy error, rejected login, 'complied with problems' from browser,...)
will happen again,
but ill take care of it tomorrow :D im out!!!!