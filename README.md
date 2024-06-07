# Lab 8

Lab 8 was showing a way to obtain JSON data from an outside site, and use it in your project. This was the first time using "fetch" with chaining to obtain the information:
```ruby
const fastify = require('fastify')();
const fetch = require('node-fetch');

fastify.get("/photos", (request, reply) => {
    fetch("https://jsonplaceholder.typicode.com/photos")
        .then((response) => response.json())
        .then((json) => {
            console.log(json);
            reply
            .code(200)
            .header("Content-Type", "text/json; charset=utf-8")
            .send({ error: "", statusCode: 200, photos: json });
        })
        .catch((err) => {
            console.log(`Error: ${err}`);
            reply
            .code(404)
            .header("Content-Type", "text/json; charset=utf-8")
            .send({ error: err, statusCode: 404, photos: [] });
        });
  });
  
  fastify.get("/photos/:id", (request, reply) => {
    const { id = "" } = request.params; 

    fetch(`https://jsonplaceholder.typicode.com/photos`)
    .then((response) => response.json())
    .then((json) => {
        reply
        .code(200)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: "", statusCode: 200, photo: json[id] });
    })
    .catch((err) => {
        reply
        .code(404)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: err, statusCode: 404, photo: {} });
    });
  });
  
  // Start server and listen to requests using Fastify
  const listenIP = "localhost";
  const listenPort = 8080;
  fastify.listen(listenPort, listenIP, (err, address) => {
    if (err) {
      console.log(err);
      process.exit(1);
    }
    console.log(`Server listening on ${address}`);
  });
```
<a href="https://joeybez.github.io/joeybezner.github.io/">Back to Home</a>
