# postman-qa

Practice repository for API testing with Postman, built while learning QA fundamentals.

## What's inside

This repo contains two Postman collections with real API requests and automated tests, used to practice the core QA workflow: read the documentation, build a request, validate the response, and write meaningful assertions.

### 🌍 Countries.postman_collection.json
Requests against [countries.dev](https://countries.dev), a free, key-less REST API for country data.

- **Request** — `GET /countries`, lists every country. Test: status code is 200.
- **Euro** — `GET /currency/EUR`, lists countries whose currency is the Euro. Tests: status code is 200, first country's currency name equals `"Euro"`.
- **Spain** — `GET /alpha/ES`, a single country lookup. Tests: status code is 200, population is above 40 million, country name equals `"Spain"`.

### ⛅ Weather.postman_collection.json
Requests against [Open-Meteo](https://open-meteo.com), a free weather API, and [JSONPlaceholder](https://jsonplaceholder.typicode.com), a fake REST API used for initial practice.

- **First Request** — `GET jsonplaceholder.typicode.com/users`. Tests: status code is 200, response contains at least one user, first user has `name` and `email`.
- **Clima Varsovia** — `GET /v1/forecast` with hardcoded coordinates for Warsaw. Tests: status code is 200, response contains `current_weather`, temperature is a number, latitude matches Warsaw.
- **Temperatura Terrassa** — `GET /v1/forecast` with coordinates for Terrassa (resolved beforehand via Open-Meteo's geocoding endpoint). Tests: status code is 200, response contains `current_weather`, response contains `elevation`, temperature is a number.

## Tools & concepts practiced

- Writing `pm.test()` assertions in Postman (status codes, property checks, type checks, value checks)
- Reading API documentation to find the correct endpoint instead of guessing
- Handling path parameters vs. placeholders (e.g. `/region/{code}` → `/region/Europe`)
- Organizing requests into collections
- Exporting collections and version-controlling them with Git

## How to use

1. Import either `.json` file into Postman (**File → Import**)
2. Run individual requests or the whole collection with the Collection Runner
3. Check the **Test Results** tab after each request

---
