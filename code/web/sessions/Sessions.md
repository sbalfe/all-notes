# Sessions

- Not practical or secure to store lots of data using client-side cookies.
- This is where sessions come from. 

---

- Sessions are server side data store that we use to make HTTP stateful

- Instead of storing data using cookies, we store the data on the server side and then send the browser a cookie

  used to retrieve the data.

---

- Different from persistent data in a database
- The sessions keeps data such as user who toggled to be left logged into the website for example. 

---

- There is a often a limit on the size of total cookies therefore sessions must be put to use. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210123155711885.png)

- Often a cookie to store each session id value for the user

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210123160704469.png)

https://www.npmjs.com/package/express-session

- Resave
  - forces the session to be saved back to the session store even if the session was never modified during the actual request. 
  - often set to be false as the default is true, however its a deprecated setting.

- SaveUninitialized
  - Forces a session that is not initialised to be saved to the store
  - A session is uninitialized when it is new but not modified. 

https://stackoverflow.com/questions/40381401/when-to-use-saveuninitialized-and-resave-in-express-session

# Connect-flash

https://www.npmjs.com/package/connect-flash

- A place in the session which flashes a message to the user. 
- messages are written to the flash and cleared after being displayed to the user, used in combination with redirects.
- Ensures that the message is available to the next page that is to be rendered

- Used after signing in or submitting form on redirects
  - refreshing however makes the message go away. 

HTTP ONLY

https://owasp.org/www-community/HttpOnly