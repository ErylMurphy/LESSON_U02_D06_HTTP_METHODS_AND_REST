# RESTful Routing

A standard for client-server architectures.
https://en.wikipedia.org/wiki/Representational_state_transfer

## HTTP Methods

aka _verbs_. They specify what effect a request is intended to have and how the server should process it. It is up to the server to do so correctly.

[Wikipedia's documentation on the methods](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)

### Idempotence

Roughly speaking, something is _idempotent_ if doing it repeatedly gives the same effect as doing it the first time. The HTTP methods can be classified by whether or not they are idempotent.

### Important Methods

#### GET

Requests retrieve data, have no other effect. Idempotent?

#### POST

Classically, creates a new element in some collection-like resource, which is automatically assigned a new URI. A bit like `INSERT` in Postgres. Idempotent?

In practice, POST is also frequently used to perform operations not covered by GET. From the [Wikipedia article](https://en.wikipedia.org/wiki/POST_(HTTP)):

> given most web browsers' natural limitation to use only GET or POST, designers felt the need to re-purpose POST to do many other data submission and data management tasks, including the alteration of existing records and their deletion.

The AJAX techniques we'll be using have ways of enabling methods beyond `GET` and `POST`, so this limitation isn't onerous for us and we can easily implement correct RESTful routes.

#### PUT

Supplies all the information required to describe a resource except for the URI, to a URI. If there is already a resource at the targeted URI, it is updated with the new information; otherwise a new resource is created at that URI with the new information.

Idempotent?

#### DELETE

Deletes the resource at the URL.

Idempotent?

#### PATCH

Supplies partial information to a URI; the resource at that URI is then updated to reflect it. If the URI does not currently point to a specific resource, "the server MAY create a new resource," according to some [official-looking docs](https://tools.ietf.org/html/rfc5789).

In my opinion this _should_ be idempotent, to support idempotent merges. It is not required by the spec, however, for reasons that elude me. If you want to support PATCH (generally not required for what we're doing), you can write your server to support it (from the same docs: "A PATCH request can be issued in such a way as to be idempotent"). For bickering about whether PATCH should or should not be idempotent, see [this Hacker News thread](https://news.ycombinator.com/item?id=4198854).

## Semantic URLs

Semantic URLs are a way of structuring the URLs (or "routes") accepted by our server such that they form reasonably legible paths into the underlying resources.

In many ways this is more art than science, but there are some principles to follow.

- Levels of resources are separated by slashes `/`
- In a parent-child relationship, the parent comes before the child: `parent/child`
- The collection-element relationship is a parent-child relationship
- Collections of resources are pluralized
- Individual elements within a collection may be represented by unique ids
- In most cases collections can be represented by a more descriptive name

Some hand-wavy examples of semantic URLs can be found on the [relevant Wikipedia article](https://en.wikipedia.org/wiki/Semantic_URL#Structure).

## Methods and URLs

There is a wonderful table laying out the relationship between methods and urls on wikipedia:

https://en.wikipedia.org/wiki/Representational_state_transfer#Relationship_between_URL_and_HTTP_methods

Here transcribed:

| URL | GET | PUT | PATCH | POST | DELETE
|--|--|--|--|--|--|
|Collection, such as `https://api.example.com/resources/`| List the URIs and perhaps other details of the collection's members. | Replace the entire collection with another collection. | Not generally used | Create a new entry in the collection. The new entry's URI is assigned automatically and is usually returned by the operation | Delete the entire collection. |
| Element, such as `https://api.example.com/resources/item17` | Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type. | Replace the addressed member of the collection, or if it does not exist, create it. | Update the addressed member of the collection. | Not generally used. Treat the addressed member as a collection in its own right and create a new entry within it | Delete the addressed member of the collection. |

## Codealong Lab

Install [Postman](https://www.getpostman.com/), a GUI HTTP client.

Install JavaScript dependencies for running a server locally.

```
npm install
```

Run the server

```
node index.js
```

Visit the website [`http://localhost:5678/`](http://localhost:5678/)

Dinoland is empty. 

You can see that there are no dinosaurs by visiting the `http://localhost:5678/dinosaurs` `GET` endpoint in Postman.

Create a dinosaur at the `http://localhost:5678/dinosaurs` `POST` endpoint. You may need to provide some data.

Create a few dinosaurs. Visit the homepage again [`http://localhost:5678/`](http://localhost:5678/) to see the web app update after adding a few, and the `http://localhost:5678/dinosaurs` `GET` endpoint to see what kind of data it offers us.

See data about a specific dinosaur at the `http://localhost:5678/dinosaurs/:id` `GET` endpoint.

Delete a dinosaur at the `http://localhost:5678/dinosaurs/:id` `DELETE` endpoint.

Update information about a dinosaur at the `http://localhost:5678/dinosaurs/:id` `DELETE` endpoint.
