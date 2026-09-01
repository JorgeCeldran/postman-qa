# postman-qa

Practice repository for API testing with Postman, built while learning QA fundamentals.

## What's inside

This repo contains three Postman collections with real API requests and automated tests, used to practice the core QA workflow: read the documentation, build a request, validate the response, and write meaningful assertions.

### 🌍 Countries.postman_collection.json
Requests against [countries.dev](https://countries.dev), a free, key-less REST API for country data.

- **Request** — `GET /countries`, lists every country. Test: status code is 200.
- **Euro** — `GET /currency/EUR`, lists countries whose currency is the Euro. Tests: status code is 200, first country's currency name equals `"Euro"`.
- **Spain** — `GET /alpha/ES`, a single country lookup. Tests: status code is 200, population is above 40 million, country name equals `"Spain"`.

### ⛅ Weather.postman_collection.json
Requests against [Open-Meteo](https://open-meteo.com), a free weather API, and [JSONPlaceholder](https://jsonplaceholder.typicode.com), a fake REST API used for initial practice.

- **First Request** — `GET jsonplaceholder.typicode.com/users`. Tests: status code is 200, response contains at least one user, first user has `name` and `email`.
- **Warsaw Weather** — `GET /v1/forecast` with hardcoded coordinates for Warsaw. Tests: status code is 200, response contains `current_weather`, temperature is a number.
- **Terrassa Weather** — `GET /v1/forecast` with coordinates for Terrassa (resolved beforehand via Open-Meteo's geocoding endpoint). Tests: status code is 200, response contains `current_weather`, response contains `elevation`, temperature is a number.

### 🔄 Posts-CRUD.postman_collection.json
Full CRUD cycle (Create, Read, Update, Delete) against [JSONPlaceholder](https://jsonplaceholder.typicode.com), practicing all four core HTTP verbs on the same resource.

- **Get All Posts** — `GET /posts`. Tests: status code is 200, response is an array.
- **Create POST** — `POST /posts` with a JSON body (`title`, `body`, `userId`). Tests: status code is 201, response has an `id`, title matches what was sent.
- **Update Post** — `PUT /posts/1` with an updated JSON body. Tests: status code is 200, title was updated.
- **DELETE Post** — `DELETE /posts/1`. Tests: status code is 200, response body is empty.

### 🎬 TMDB-Auth-Practice.postman_collection.json

Requests against The Movie Database (TMDB), a real-world API that requires authentication, used to practice different authentication patterns and validate both authorized and unauthorized responses.

- **Popular Movies - NO AUTH** — `GET /movie/popular`, sent with no authorization. Test: status code is 401 (confirms the API correctly rejects unauthenticated requests).
- **Popular Movies** — `GET /movie/popular`, sent with a Bearer Token (API Read Access Token) in the Authorization header. Test: status code is 200.
- **Popular Movies - API Key v3** — `GET /movie/popular`, sent with the API Key as a query parameter (`?api_key=...`), demonstrating an alternative authentication pattern. Tests: status code is 200; every movie in the response has an `original_language` field; at least one movie's `original_language` is Spanish (`es`).

## Tools & concepts practiced

- Writing `pm.test()` assertions in Postman (status codes, property checks, type checks, value checks)
- Reading API documentation to find the correct endpoint instead of guessing
- Handling path parameters vs. placeholders (e.g. `/region/{code}` → `/currency/EUR`)
- Practicing the full CRUD cycle: GET, POST, PUT, DELETE, with response validation for each verb
- Distinguishing status codes by operation (`200` for reads/updates/deletes, `201` for successful creation)
- Organizing requests into collections
- Exporting collections and version-controlling them with Git

## How to use

1. Import any of the three `.json` files into Postman (**File → Import**)
2. Run individual requests or a whole collection with the Collection Runner
3. Check the **Test Results** tab after each request

### Quick import (no download needed)

Paste any of these raw URLs into Postman's Import dialog (or drag-and-drop the link):

- **Countries**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Countries.postman_collection.json`
- **Weather**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Weather.postman_collection.json`
- **Posts CRUD**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Posts-CRUD.postman_collection.json`

---
