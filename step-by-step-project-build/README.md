<h1>
  <span class="headline">Testing Express APIs with Jest Project</span>
  <span class="subhead">Project GitHub Repo URL Deliverable</span>
</h1>

## Project Instructions

For your project, you will be building and testing an API using **JavaScript**, **Express**, **Jest**, and **Supertest**.

In class, we practiced the basics of unit testing JavaScript functions using Jest. Now, we’re extending that knowledge to test API endpoints. This project will give you the opportunity to build a partial API and write automated tests to ensure its functionality.

### **Choose Your Theme**

Start by choosing a theme or service that interests you. Consider building an API that aligns with sustainability, community impact, or any area you’re passionate about.

### **What You'll Learn:**

By completing this project, you’ll:

1. Build a modular, well-structured Express API.
2. Write automated tests for your API endpoints using Jest and Supertest.
3. Develop skills for building and testing real-world APIs.

### **Getting Started**

Follow the provided project outline and reference our in class lesson materials and you’ll create a fully functional API with thorough test coverage. This project is your chance to combine coding and testing skills into a valuable, portfolio-worthy application.

## Adding Jest Testing to an Express API

This guide will walk you through setting up Jest testing for an Example Express API while encouraging you to design the API around a theme of your choice. You’ll learn how to test your API endpoints with mock data that fits your theme.

---

## 1. Initialize Node.js Project

1. **Start by creating a Node.js project:**

   ```bash
   npm init -y
   ```

2. **Install Required Dependencies:**

   Install the necessary packages:

   - `express` for the API framework.
   - `jest` for testing.
   - `supertest` for HTTP assertions.

   ```bash
   npm install express
   npm install --save-dev jest supertest
   ```

3. **Update `package.json` for Jest:**

   Add a test script:

   ```json
   "scripts": {
     "test": "jest"
   }
   ```

### Supertest

[**SuperTest**](https://www.npmjs.com/package/supertest) is a library for testing HTTP servers.

- It simplifies sending HTTP requests (like `GET`, `POST`, `PUT`, and `DELETE`) to your API during testing.
- Instead of manually using tools like Postman or curl, `supertest` lets you write tests that simulate these requests in code.
- It works alongside Jest to assert the behavior of your API.

### Why Do We Need `supertest`?

When building an API, it’s essential to test how it handles HTTP requests and responses. `supertest` allows us to:

- Simulate client interactions with the API without needing a live server.
- Send mock requests to endpoints and verify the responses (ex:. status codes, returned data).
- Test edge cases, such as missing data or invalid inputs, to ensure the API handles them gracefully.

For example, with `supertest`, you can easily write a test to check if your `/movies` route returns a list of movies:

```javascript
// *** This is an example for a Movies API ***

it('GET /api/movies should return a list of movies', async () => {
  const response = await request(app).get('/api/movies');
  expect(response.statusCode).toBe(200);
  expect(response.body.movies).toEqual([...]);
});
```

---

## 2. Choose Your API Theme

Think about a topic you’re passionate about or interested in, such as:

- **Expense Manager**: Track income, expenses, and create budgets.
- **Travel Planner**: Plan trips, manage itineraries, and store destination details.
- **Recipes Manager**: Store and retrieve recipes with ingredients and cooking steps.

> Example: If you choose a "Movies Database" theme, your API might include endpoints like `/movies` and `/movies/:id`,

For simplicity sake you only need to complete routes for GET, POST and GET one by ID.

| **HTTP Method** | **Endpoint**      | **Description**                 | **Request Body**                                                         | **Response**                             |
| --------------- | ----------------- | ------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------- |
| GET             | `/api/movies`     | Retrieve all movies             | None                                                                     | JSON object with an array of all movies  |
| POST            | `/api/movies`     | Add a new movie                 | `{ "id": number, "title": string, "director": string, "genre": string }` | JSON object with the newly created movie |
| GET             | `/api/movies/:id` | Retrieve a specific movie by ID | None                                                                     | JSON object with the movie details       |

---

## 3. Create Your Express API

### Set Up the Project Structure

Organize your app into folders:

```text
*** This is an example for a Movies API ***

/project
/controllers
    moviesController.js
/routes
    moviesRouter.js
app.js
server.js
```

### Implement the Controller

Create a file `controllers/moviesController.js` to handle the logic for your endpoints:

```javascript
// *** This is an example for a Movies API ***

const sampleMovies = [
  { id: 1, title: 'Inception', director: 'Christopher Nolan', genre: 'Sci-Fi' },
  { id: 2, title: 'The Matrix', director: 'Wachowskis', genre: 'Sci-Fi' },
];

// Get all movies
const getAllMovies = (req, res) => {
  res.status(200).json({ movies: sampleMovies });
};

// Create a new movie
const createMovie = (req, res) => {
  const newMovie = req.body;
  sampleMovies.push(newMovie);
  res.status(201).json({ movie: newMovie });
};

// Get a movie by ID
const getMovieById = (req, res) => {
  const id = parseInt(req.params.id, 10);
  const movie = sampleMovies.find((m) => m.id === id);
  if (movie) {
    res.status(200).json({ movie });
  } else {
    res.status(404).json({ error: 'Movie not found' });
  }
};

export default {
  getAllMovies,
  createMovie,
  getMovieById,
};
```

### Implement the Router

Create a file `routes/moviesRouter.js` to define the routes:

```javascript
// *** This is an example for a Movies API ***

import express from 'express';
import moviesController from '../controllers/moviesController.js';

const Router = express.Router();

Router.route('/movies')
  .get(moviesController.getAllMovies)
  .post(moviesController.createMovie);

Router.route('/movies/:id').get(moviesController.getMovieById);

export default Router;
```

### Start the Server

Create `server.js` to run the server:

```javascript
// *** This is an example for a Movies API ***

import express from 'express';
import moviesRouter from './routes/moviesRouter.js';

const app = express();
const PORT = 3000;

// Middleware
app.use(express.json());

// Routes
app.use('/api', moviesRouter);

// Start the server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

Find your API endpoints at:

| **HTTP Method** | **Endpoint**      |
| --------------- | ----------------- |
| GET             | `/api/movies`     |
| POST            | `/api/movies`     |
| GET             | `/api/movies/:id` |

### Test Your API in Postman

Before writing automated tests in code, use Postman to manually test your API and verify that it works as expected.

1. **Add New Items**  
   Use the `POST` endpoint to add new items to the mock data array. Provide the necessary data in the request body.

2. **Retrieve All Items**  
   Use the `GET` endpoint to retrieve all items. Ensure the newly added items are included in the response.

3. **Retrieve a Single Item by ID**  
   Use the `GET /:id` endpoint to retrieve a specific item by its ID (index in the array). Ensure the correct item is returned in the response, or that a `404` error is returned if the item does not exist.

## 4. Write Tests Using Jest and Mock Data

1. **Create a `tests` Directory:**

   Store your test files in a `tests` folder.

2. **Write Test Cases:**

   Create a file `tests/moviesRouter.test.js`:

```javascript
// *** This is an example for a Movies API ***

import request from 'supertest';
import app from '../app.js';

describe('Movies API', () => {
  it('GET /api/movies should return a list of movies', async () => {
    const response = await request(app).get('/api/movies');
    expect(response.statusCode).toBe(200);
    expect(response.body.movies).toEqual([
      { id: 1, title: 'Inception', director: 'Christopher Nolan', genre: 'Sci-Fi' },
      { id: 2, title: 'The Matrix', director: 'Wachowskis', genre: 'Sci-Fi' },
  ]);
});

it('POST /api/movies should add a new movie', async () => {
  const newMovie = { id: 3, title: 'Interstellar', director: 'Christopher Nolan', genre: 'Sci-Fi' };
  const response = await request(app).post('/api/movies').send(newMovie);
  expect(response.statusCode).toBe(201);
  expect(response.body.movie).toEqual(newMovie);
});

it('GET /api/movies/:id should return a specific movie', async () => {
  const response = await request(app).get('/api/movies/1');
  expect(response.statusCode).toBe(200);
  expect(response.body.movie).toEqual({ id: 1, title: 'Inception', director: 'Christopher Nolan', genre: 'Sci-Fi' });
});
```

---

## 5. Run the Tests

Run the tests using Jest:

```bash
npm test
```

You should see results indicating whether your API endpoints work as expected.

---
