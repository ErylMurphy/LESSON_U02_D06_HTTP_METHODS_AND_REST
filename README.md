# RESTful Routing with HTTP methods

_After this lesson, students will be able to:_

*   Match the HTTP methods GET, POST, PATCH and DELETE to the database actions INSERT INTO, SELECT, UPDATE and DELETE
*   Describe the behavior associated with the HTTP methods GET, POST, PATCH and DELETE at different RESTful endpoints

## HTTP Methods

An HTTP request can include a lot of information, but the most important bits are:

*   The URL, for example `https://www.youtube.com/watch?v=dQw4w9WgXcQ`
*   The HTTP method, for example `GET` or `POST`

The method, like `GET`, describes what effect a request is intended to have, and how the server should process it.

Read more in [the HTTP Verbs section of this article](https://code.tutsplus.com/tutorials/a-beginners-guide-to-http-and-rest--net-16340).

### `GET`

A `GET` request retrieves data at the provided URL. This is the most common HTTP method, as it's the default when we enter a URL into our browser.

For example, making the request `GET https://www.nytimes.com` returns the HTML content for The New York Times homepage.

`GET` requests are **idempotent**. We can make the same request again and again and get the same result.

`GET` requests are similar to SQL `SELECT FROM` statements, in that both are requests for information.

### `POST`

A `POST` request creates a new item on the server.

A request like `POST https://www.yelp.com/little-beet-restaurant/review` could make a new review for Little Beet Restaurant when we click the "Write a Review" button on the webpage.

`POST` requests are often accompanied by data passed along in the HTTP request body. e.g. when we write a review for Little Beet, we would include our username (`eric`), our review (`The shrimp salad is so delightful`), and a star rating (`4` out of 5).

`POST` requests often return an ID of the new item, like `3`.

`POST` requests are **not** idempotent. When we make one of these requests, the state of the server changes. Making the request again would create another item with another ID.

`POST` requests are similar to SQL `INSERT INTO` statements.

In practice, `POST` is also frequently used to perform operations not covered by `GET`. From the [Wikipedia article](<https://en.wikipedia.org/wiki/POST_(HTTP)>):

> given most web browsers' natural limitation to use only GET or POST, designers felt the need to re-purpose POST to do many other data submission and data management tasks, including the alteration of existing records and their deletion.

The AJAX techniques we'll be using have ways of enabling methods beyond `GET` and `POST`, so this limitation isn't onerous for us and we can easily implement correct RESTful routes.

### `DELETE`

The `DELETE` method removes the resource at the URL.

For example, `DELETE https://www.yelp.com/little-beet-restaurant/review/353` will delete the review with the ID of `353`.

`DELETE` requests are **not** idempotent. When we make one of these requests, the state of the server changes. Making the request again would probably result in an error since the item was already deleted.

The `DELETE` method is similar to the `DELETE FROM` statement.

### `PATCH`

The `PATCH` method updates an item at the specified resouce.

A request like `PATCH https://www.yelp.com/little-beet-restaurant` could update the information for Little Beet Restaurant after the owner edits the restaurant's hours.

Like `POST` requests, `PATCH` requests come with data in the HTTP request body to specify the fields and new values to update. For example `Hours: M-F 10a-10p, closed on Saturday and Sunday`.

## Methods and URLs

There is a wonderful table laying out the relationship between methods and urls on wikipedia:

https://en.wikipedia.org/wiki/Representational_state_transfer#Relationship_between_URL_and_HTTP_methods

Here transcribed:

| URL                                                         | GET                                                                                                                   | PUT                                                                                 | PATCH                                          | POST                                                                                                                         | DELETE                                         |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| Collection, such as `https://api.example.com/resources/`    | List the URIs and perhaps other details of the collection's members.                                                  | Replace the entire collection with another collection.                              | Not generally used                             | Create a new entry in the collection. The new entry's URI is assigned automatically and is usually returned by the operation | Delete the entire collection.                  |
| Element, such as `https://api.example.com/resources/item17` | Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type. | Replace the addressed member of the collection, or if it does not exist, create it. | Update the addressed member of the collection. | Not generally used. Treat the addressed member as a collection in its own right and create a new entry within it             | Delete the addressed member of the collection. |

## Semantic URLs

A brief introduction to [REST APIs](https://dev.to/benhayehudi/a-brief-introduction-to-rest-apis-172).

Semantic URLs are a way of structuring the URLs (or "routes") accepted by our server so they form meaningful paths to the underlying resources.

In many ways this is more art than science, but there are some principles to follow.

*   Levels of resources are separated by slashes `/`
*   In a parent-child relationship, the parent comes before the child: `parent/child`
*   The collection-element relationship is a parent-child relationship
*   Collections of resources are pluralized
*   Individual elements within a collection may be represented by unique ids
*   In most cases collections can be represented by a more descriptive name

Some hand-wavy examples of semantic URLs can be found on the [relevant Wikipedia article](https://en.wikipedia.org/wiki/Semantic_URL#Structure).

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

1.  Visit the website [`http://localhost:5678/dinosaurs`](http://localhost:5678/dinosaurs)

2.  Notice that Dinoland is empty to begin with!

3.  Create a dinosaur by sending a reques to the `http://localhost:5678/dinosaurs` `POST` endpoint. You may need to provide some data, look at the response body for helpful information.

4.  Create a few more dinosaurs! Visit the homepage again [`http://localhost:5678/dinosaurs`](http://localhost:5678/dinosaurs) to see the web app update after adding a few, and the `GET http://localhost:5678/dinosaurs.json` endpoint to see what kind of data it offers us.

5.  See data about a specific dinosaur at the `GET http://localhost:5678/dinosaurs/:id.json` endpoint.

6.  Delete a dinosaur at the `http://localhost:5678/dinosaurs/:id` `DELETE` endpoint.

7.  Update information about a dinosaur at the `PATCH http://localhost:5678/dinosaurs/:id` endpoint.
