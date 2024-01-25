Designing and enhancing **in-house RESTful APIs** and working with third-party web services and APIs using OpenAPI 3.0.

**Step 1: Define the API**
Let's say we want to create a simple RESTful API for managing a collection of books. We'll have endpoints to list all books, get details for a specific book, add a new book, update an existing book, and delete a book.

**Step 2: PHP Implementation**
Here's a basic PHP implementation of this API using the Slim framework and the OpenAPI 3.0 specification:

<?php
require 'vendor/autoload.php';

use Slim\Factory\AppFactory;
use Psr\Http\Message\ResponseInterface as Response;
use Psr\Http\Message\ServerRequestInterface as Request;

$app = AppFactory::create();

$app->get('/api/books', function (Request $request, Response $response) {
 // Your code to retrieve all books from the database or another source
 $books = [
 ['id' => 1, 'title' => 'Book 1', 'author' => 'Author 1'],
 ['id' => 2, 'title' => 'Book 2', 'author' => 'Author 2'],
 ];

 $response->getBody()->write(json_encode($books));
 return $response->withHeader('Content-Type', 'application/json');
});

$app->get('/api/books/{id}', function (Request $request, Response $response, $args) {
 $id = $args['id'];
 // Your code to retrieve book details by ID
 $book = ['id' => $id, 'title' => 'Book 1', 'author' => 'Author 1'];

 $response->getBody()->write(json_encode($book));
 return $response->withHeader('Content-Type', 'application/json');
});

// Implement other HTTP methods (POST, PUT, DELETE) and endpoints as needed

$app->run();

This code sets up a basic API using the Slim framework with endpoints for listing all books and retrieving book details by ID. You would need to implement the remaining endpoints for adding, updating, and deleting books as required.

Step 3: OpenAPI 3.0 Documentation
To document your API using OpenAPI 3.0, you can create a swagger.yaml file like this:

openapi: 3.0.0
info:
 title: Bookstore API
 description: A simple API to manage books
 version: 1.0.0
paths:
 /api/books:
 get:
 summary: Get a list of all books
 responses:
 '200':
 description: A list of books
 content:
 application/json:
 schema:
 type: array
 items:
 $ref: '#/components/schemas/Book'
 /api/books/{id}:
 get:
 summary: Get a book by ID
 parameters:
 - name: id
 in: path
 required: true
 description: ID of the book to retrieve
 schema:
 type: integer
 responses:
 '200':
 description: A book object
 content:
 application/json:
 schema:
 $ref: '#/components/schemas/Book'
components:
 schemas:
 Book:
 type: object
 properties:
 id:
 type: integer
 title:
 type: string
 author:
 type: string
