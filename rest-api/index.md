# REST API

WordPress 4.4 introduced the infrastructure for a REST API.  The REST API provides an easy way to get data into and out of WordPress.  Data can be retrieved and stored by sending HTTP requests to the REST API server.  The REST API takes advantage of different HTTP methods.

- `GET` should be used for retrieving data from the API.
- `POST` should be used for creating new resources (i.e users, posts, taxonomies).
- `PUT` should be used for updating resources.
- `DELETE` should be used for deleting resources.
- `OPTIONS` should be used to provide context about our resources.

A resource is any single entity or object.  A good example of a resource for WordPress would be a post. A post has different properties like its title and content.  A response from the API could show us title and content as fields in the response.  The REST API enables us to interact with posts and other WordPress resources  in a new way.  The REST API makes sharing our content with the rest of the web easier, and it provides us a structured way to handle complex interactions within WordPress.

In this chapter of the Plugin Handbook, we will explore how the API works and how we can leverage its power to do great things with WordPress!
