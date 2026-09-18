# postman-qa

Practice repository for API testing with Postman, built while learning QA fundamentals.

## What's inside

This repo contains four Postman collections with real API requests and automated tests, used to practice the core QA workflow: read the documentation, build a request, validate the response, and write meaningful assertions.

### 🧵 Articles_API.postman_collection.json

The most complete collection in this repo: a full-coverage suite built from scratch against [GoRest](https://gorest.co.in), a free public REST API for testing/prototyping. Includes documentation (collection-level and per-request), a Collection Runner flow, and dynamic data generation.

**`Articles` folder** — full CRUD on the `posts` resource, chained automatically via environment variables:
- **Get_All_Articles** — `GET /posts`. Tests: status, response time, array shape, required fields.
- **Get_Articles_By_ID_fixed** — demonstrative version with a hardcoded ID, for comparison against the variable-based version.
- **Get_Article_By_Id_Variable** — `GET /posts/{{post_id}}`, with Bearer Token (GoRest requires auth to read recently created/modified resources, even on "public" endpoints).
- **Create_Article** — `POST /posts`. Automatically saves the created `id` into `post_id` for the rest of the flow.
- **Update_Article** — `PUT /posts/{{post_id}}`. Validates the response against `pm.request.body.raw` instead of hardcoded text, so the test stays valid regardless of content length.
- **Delete_Article** — `DELETE /posts/{{post_id}}`. Verifies 204.
- **Get_Deleted_Article_By_Id** — confirms a 404 after deletion, instead of trusting the DELETE status code alone.

**`Users` folder** — reads, negative testing, and query param filtering on the `users` resource:
- **Get_All_Users** — saves the first user's `id` into `user_id` for reuse.
- **Get_User_By_Id** — verifies the returned `id` actually matches the one requested, not just that the response "looks like" a user.
- **User_Not_Found** — negative test: requests a non-existent ID, verifies a 404 with the expected error message.
- **Get_Users_By_Gender** — filters by `{{gender}}`. The test reads the expected value directly from the request URL (`pm.request.url.query.get`), so the same script works for `male` or `female` with no code changes.
- **Get_Users_By_Gender_And_Name** — combines `gender`, `name`, and `per_page`. Includes a test that deliberately fails to document a real API behavior (see Findings below).

**`Articles_Dynamic` folder** — an isolated, advanced version of `Create_Article`, kept separate from the main flow so it can't break the Collection Runner:
- **Create_Article_Dynamic** — obtains a real `user_id` via `pm.sendRequest()` to `GET /users` in a Pre-request script (Postman waits for the callback automatically, no manual async handling needed), and generates random `title`/`body` with Postman's native dynamic variables (`{{$randomCatchPhrase}}`, `{{$randomLoremParagraph}}`).

**Findings documented in this collection:**
1. GoRest requires the Bearer Token even on GET requests for recently created/modified resources.
2. The `name` query param on `/users` matches partial strings, not prefixes — a test intentionally fails to prove this (`Get_Users_By_Gender_And_Name`).

**Environment required:** `Articles_API.postman_environment.json` (import alongside the collection). Variables: `base_url`, `token` (your own GoRest Access Token), `post_id`, `user_id`, `gender`, `letter`, `length`, `dynamic_user_id`, `dynamic_post_id`.

### 🌍 Countries.postman_collection.json
Requests against [countries.dev](https://countries.dev), a free, key-less REST API for country data.

- **Request** — `GET /countries`, lists every country. Test: status code is 200.
- **Euro** — `GET /currency/EUR`, lists countries whose currency is the Euro. Tests: status code is 200, first country's currency name equals `"Euro"`.
- **Spain** — `GET /alpha/ES`, a single country lookup. Tests: status code is 200, population is above 40 million, country name equals `"Spain"`.

### ⛅ Weather.postman_collection.json
Requests against [Open-Meteo](https://open-meteo.com), a free weather API.

- **Warsaw Weather** — `GET /v1/forecast` with hardcoded coordinates for Warsaw. Tests: status code is 200, response contains `current_weather`, temperature is a number, latitude matches Warsaw.
- **Terrassa Weather** — `GET /v1/forecast` with coordinates for Terrassa. Tests: status code is 200, response contains `current_weather`, response contains `elevation`, temperature is a number.

### 🎬 TMDB-Auth-Practice.postman_collection.json

Requests against The Movie Database (TMDB), a real-world API that requires authentication, used to practice different authentication patterns and validate both authorized and unauthorized responses. Credentials are stored as collection variables (`tmdb_token`, `tmdb_api_key`), left empty in this repo — add your own to run it.

- **Popular Movies - NO AUTH** — `GET /movie/popular`, sent with no authorization. Test: status code is 401 (confirms the API correctly rejects unauthenticated requests).
- **Popular Movies** — `GET /movie/popular`, sent with a Bearer Token (`{{tmdb_token}}`) in the Authorization header. Test: status code is 200.
- **Popular Movies - API Key v3'** — `GET /movie/popular`, sent with the API Key as a query parameter (`{{tmdb_api_key}}`), demonstrating an alternative authentication pattern. Tests: status code is 200; every movie in the response has an `original_language` field; at least one movie's `original_language` is Spanish (`es`).

## Tools & concepts practiced

- Writing `pm.test()` assertions in Postman (status codes, property checks, type checks, value checks)
- Reading API documentation to find the correct endpoint instead of guessing
- Full CRUD cycle (GET, POST, PUT, DELETE) with response validation and automatic ID chaining between requests via environment variables
- Robust assertions: comparing responses against `pm.request.body.raw` and `pm.request.url.query.get(...)` instead of hardcoded values, so tests stay valid regardless of dynamic content
- Negative testing (expecting and validating 404/401 responses)
- Async pre-request scripts with `pm.sendRequest()` to fetch real data before building a request
- Native Postman dynamic variables for randomized test data
- Collection and per-request documentation
- Running a full flow end-to-end with the Collection Runner
- Organizing requests into collections and folders
- Exporting collections/environments and version-controlling them with Git, keeping credentials out of source control

## How to use

1. Import any of the `.json` files into Postman (File → Import)
2. For `Articles_API`, also import its `.postman_environment.json` and fill in your own GoRest token; for `TMDB Auth Practice`, fill in `tmdb_token`/`tmdb_api_key` as collection variables
3. Run individual requests or a whole collection with the Collection Runner
4. Check the **Test Results** tab after each request

### Quick import (no download needed)

Paste any of these raw URLs into Postman's Import dialog (or drag-and-drop the link):

- **Articles_API (collection)**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Articles_API.postman_collection.json`
- **Articles_API (environment)**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Articles_API.postman_environment.json`
- **Countries**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Countries.postman_collection.json`
- **Weather**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/Weather.postman_collection.json`
- **TMDB Auth Practice**: `https://raw.githubusercontent.com/JorgeCeldran/postman-qa/refs/heads/main/TMDB-Auth-Practice.postman_collection.json`

---
