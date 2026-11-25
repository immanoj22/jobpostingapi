# jobpostingapi

A small Node and Express API for a job posting board. It stores job posts in PostgreSQL and exposes two endpoints: one to list posts and one to create a post. I wrote it as the back end for a React job board form.

## What it does
- Returns all job posts ordered by id
- Saves a new post with title, company name, district, job type, min and max salary, description and a date
- Uses the `pg` connection pool and reads the database URL from a `.env` file

## How it works

```mermaid
flowchart LR
    R[React job board form] -->|POST /submit| API[Express API :9000]
    R -->|GET /home| API
    API --> P[pg pool]
    P --> DB[(PostgreSQL companylist)]
```

## Tech stack
Node.js (ES modules), Express, pg, cors, body-parser, dotenv

## Files
- `index.js` - routes and server (port 9000)
- `database.js` - PostgreSQL pool

## Running it locally
You need Node and a PostgreSQL database.

1. Create a table named `companylist` with columns `id` (serial primary key), `createjobtitile`, `companyname`, `district`, `jobtype`, `minsalary`, `maximumsalary`, `jobdescription` and `selecteddate`. The code does not create it for you.
2. Create a `.env` file with your own connection string:
   ```
   DATABASE_URL=postgres://your_user:your_password@localhost:5432/your_db
   ```
   The pool is set up with SSL on, so for a local database without SSL you will need to change `database.js`.
3. Install and start:
   ```
   npm install
   node index.js
   ```

## Endpoints
- `GET /home` - list all job posts
- `POST /submit` - add a job post (JSON body), returns the updated list

## Notes
No update or delete, no validation, no auth and no tests. Errors are only logged to the console, so a failed query leaves the request hanging. The column name `createjobtitile` is a typo I kept so it matches the code.
