# Middleware

- middleware is a function between request and response
- each middleware has access to the request and response objects
- middleware can end the HTTP request by sending back a response with methods such as res.send()
- middleware can be chained together one after another by calling next.

---



https://expressjs.com/en/guide/using-middleware.html