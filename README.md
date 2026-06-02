# RPC Auth System

A minimal Express + MySQL authentication demo with a simple static client.

## Project Overview

This repository contains a small Node.js server under `server/` that provides:
- `POST /login` for user authentication
- `GET /dashboard` as a protected endpoint requiring a JWT bearer token
- A root health check route at `GET /`

A static client is available in `client/` with basic login and dashboard pages.

## Repository Structure

- `server/`
  - `server.js` – main Express app and auth routes
  - `authMiddleware.js` – JWT verification middleware
  - `db.js` – MySQL connection configuration
  - `package.json` – server dependencies and start script
- `client/`
  - `index.html` – login page
  - `dashboard.html` – protected dashboard page
  - `app.js` – login request logic
  - `client.js` – RPC helper function (currently not used by the login flow)
- `RPC_ server.sql` – database file placeholder (currently empty)
- `database/` – empty folder reserved for DB assets

## Requirements

- Node.js
- npm
- MySQL

## Setup

1. Install server dependencies:

```bash
cd server
npm install
```

2. Configure MySQL in `server/db.js` if needed:

```js
const connection = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "19122006",
  database: "rpc_auth_system"
});
```

3. Create the database and users table manually in MySQL.

Example SQL:

```sql
CREATE DATABASE IF NOT EXISTS rpc_auth_system;
USE rpc_auth_system;

CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL
);

INSERT INTO users (username, password) VALUES ('admin', 'password123');
```

> Note: Passwords are stored and compared in plain text in the current implementation.

## Run the Server

From the `server/` folder:

```bash
npm start
```

Then open `http://localhost:3000/` to verify the server is running.

## API Endpoints

### `GET /`
Returns a simple status message.

### `POST /login`
Authenticate with JSON payload:

```json
{
  "username": "your-username",
  "password": "your-password"
}
```

Success response:

```json
{
  "message": "Login Success",
  "token": "<JWT>"
}
```

### `GET /dashboard`
Protected route. Send the JWT in the `Authorization` header:

```
Authorization: Bearer <token>
```

Success response includes the authenticated user data.

## Client Usage

1. Open `client/index.html` in a browser.
2. Enter a username and password.
3. On successful login, the app stores the JWT in `localStorage` and redirects to `client/dashboard.html`.
4. `dashboard.html` requests `/dashboard` with the stored token.

## Notes and Recommendations

- `server/db.js` currently hard-codes the database credentials.
- `server/server.js` uses a hard-coded JWT secret (`mysecretkey`). Use environment variables in production.
- Passwords are compared as plain text. For production, use `bcryptjs` to hash and verify passwords.
- `client/client.js` contains an RPC helper that calls `/rpc`, but the server does not implement a `/rpc` endpoint yet.
- `RPC_ server.sql` is currently empty and should be populated with the DB schema if desired.

## Suggested Improvements

- Add user registration and password hashing.
- Move secrets and DB config to environment variables.
- Implement the `/rpc` endpoint if RPC-style API calls are required.
- Serve the `client/` pages from the Express app or use a static server.
