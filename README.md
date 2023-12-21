#### the problem

when user goes to http://localhost:3000/ and click register,
or when user goes to http://localhost:3000/register directly,

the page moves to localhost:3000,
instead of showing input field that supposed to be displayed /register

#### possible hint
for this repo, i am not using cors that handles middleware.
but will this block me to get to the certain route? i want to test this by applying cors

also, im unsure with that i made correct route for /register.
it is exactly same from the video https://www.youtube.com/watch?v=mvfsC66xqj0 
'hook up the register form' and the guy there seemed to have no problem with 
have access to register page, but that was not happening for me. 
i asked this at chatgpt and it gave me to have
------------
router.post('/register', registerUser); // Place this before other routes
router.post('/login', loginUser);
router.get('/me', protect, getMe);
-------------
instead, in context of 
--------------
It seems like your /register route is defined correctly in your userRoutes.js file. The issue might be related to the order in which your routes are defined or how your application is handling routes in general.

Here are a few things you can check:

Route Order:
Ensure that the /register route is defined before other routes that might match a similar pattern. Express evaluates routes in the order they are defined, so if you have a generic route like router.post('/', registerUser), it might be catching requests before they reach /register.

Example:
javascript
Copy code
router.post('/register', registerUser); // Place this before other routes
router.post('/login', loginUser);
router.get('/me', protect, getMe);
----------

so i tried it, but still occuring same problem, so i currently reverted it. 


#### actionable plan to likely solve the problem
test this repo with cors
or take a look at more about routes
or... idk xD might be nice to get help with this!!!
