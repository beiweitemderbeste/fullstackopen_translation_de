# part 2b

> table of contents

## Inhaltsverzeichnis

- [REST](#REST)
- [Sending Data to the Server](#Sending-Data-to-the-Server)
- [Changing the Importance of Notes](#Changing-the-Importance-of-Notes)
- [Extracting Communication with the Backend into a Separate Module](#Extracting-Communication-with-the-Backend-into-a-Separate-Module)
- [Cleaner Syntax for Defining Object Literals](#Cleaner-Syntax-for-Defining-Object-Literals)
- [Promises and Errors](#Promises-and-Errors)
- [exercises](#exercises)

> When creating notes in our application, we would naturally want to store them in some backend server. The json-server package claims to be a so-called REST or RESTful API in its documentation:

> - Get a full fake REST API with zero coding in less than 30 seconds (seriously)

> The json-server does not exactly match the description provided by the textbook definition of a REST API, but neither do most other APIs claiming to be RESTful.

> We will take a closer look at REST in the next part of the course. But it's important to familiarize ourselves at this point with some of the conventions used by json-server and REST APIs in general. In particular, we will be taking a look at the conventional use of routes, aka URLs and HTTP request types, in REST.

## REST

> In REST terminology, we refer to individual data objects, such as the notes in our application, as resources. Every resource has a unique address associated with it - its URL. According to a general convention used by json-server, we would be able to locate an individual note at the resource URL notes/3, where 3 is the id of the resource. The notes url, on the other hand, would point to a resource collection containing all the notes.

> Resources are fetched from the server with HTTP GET requests. For instance, an HTTP GET request to the URL notes/3 will return the note that has the id number 3. An HTTP GET request to the notes URL would return a list of all notes.

> Creating a new resource for storing a note is done by making an HTTP POST request to the notes URL according to the REST convention that the json-server adheres to. The data for the new note resource is sent in the body of the request.

> json-server requires all data to be sent in JSON format. What this means in practice is that the data must be a correctly formatted string, and that the request must contain the Content-Type request header with the value application/json.