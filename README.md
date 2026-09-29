# User Management API

A lightweight local REST API built with **JSON Server** and **JSON Server Auth**.

This API provides a ready-to-use backend for frontend development, testing, and API integration without requiring a real backend or database.

---

## 🚀 Overview

The API provides:

- 🔐 User authentication
- 👥 User management
- ➕ Create users
- 📖 Read users
- ✏️ Update users
- 🗑️ Delete users
- 🔎 Search users
- 🎯 Filter users
- ↕️ Sort users
- 📄 Pagination
- 🔗 Query parameters
- 💾 Local JSON data persistence
- 🌐 REST API endpoints

Everything runs locally using a `db.json` file.

---

# ⚙️ Requirements

Make sure you have installed:

- Node.js
- pnpm

Check:

```bash
node --version
pnpm --version
```

---

# 📦 Installation

Install dependencies:

```bash
pnpm install
```

---

# ▶️ Start the API

Run:

```bash
pnpm dev
```

The API will start at:

```text
http://localhost:9001
```

The API is now ready to use.

---

# 📁 Data Storage

All API data is stored in:

```text
db.json
```

There is no external database.

Changes made through `POST`, `PATCH`, `PUT`, and `DELETE` requests are persisted to the JSON file.

---

# 🔐 Authentication

The API uses **JSON Server Auth** for authentication.

Authentication is based on the `users` collection inside `db.json`.

## Login

```http
POST /login
```

Example:

```json
{
  "email": "admin@example.com",
  "password": "admin123"
}
```

Full request:

```text
POST http://localhost:9001/login
```

A successful login returns an authentication token.

```json
{
  "accessToken": "YOUR_ACCESS_TOKEN",
  "user": {
    "email": "admin@example.com",
    "name": "Admin User",
    "role": "admin"
  }
}
```

---

## 🔑 Using the Access Token

For authenticated requests, send the token using:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Example:

```javascript
const response = await fetch("http://localhost:9001/userProfiles", {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});
```

---

# 👤 User Profile API

The main user management resource is:

```text
/userProfiles
```

Base URL:

```text
http://localhost:9001/userProfiles
```

The `userProfiles` collection contains application user information such as:

- Name
- Email
- Phone
- Gender
- Date of birth
- Role
- Department
- Job title
- Status
- Location
- Address
- Joining date
- Salary
- Created/updated timestamps

---

# 📖 Get Users

Retrieve all users:

```http
GET /userProfiles
```

Example:

```text
GET http://localhost:9001/userProfiles
```

Returns an array of users.

---

# 👤 Get a Single User

Retrieve a user using their ID:

```http
GET /userProfiles/:id
```

Example:

```text
GET http://localhost:9001/userProfiles/usr_001
```

---

# ➕ Create a User

Create a new user:

```http
POST /userProfiles
```

Example request:

```json
{
  "firstName": "Neha",
  "lastName": "Desai",
  "email": "neha.desai@example.com",
  "role": "intern",
  "department": "Engineering",
  "status": "active",
  "salary": 25000
}
```

The new record is saved directly to `db.json`.

---

# ✏️ Update a User

Update an existing user:

```http
PATCH /userProfiles/:id
```

Example:

```text
PATCH http://localhost:9001/userProfiles/usr_001
```

Request body:

```json
{
  "department": "Product",
  "jobTitle": "Product Manager"
}
```

Only the provided fields are updated.

---

# 🗑️ Delete a User

Delete an existing user:

```http
DELETE /userProfiles/:id
```

Example:

```text
DELETE http://localhost:9001/userProfiles/usr_001
```

The record is removed from `db.json`.

---

# 🔎 Search & Query Parameters

The API supports JSON Server query parameters.

For example, users can be searched using:

```http
GET /userProfiles?firstName_like=aar
```

This returns users whose first name contains `aar`.

The same approach can be used with other fields such as:

```text
firstName
lastName
email
department
role
status
```

---

# 🎯 Filtering

Users can be filtered by field values.

Example:

```http
GET /userProfiles?department=Engineering
```

This returns users belonging to the Engineering department.

Multiple filters can also be combined:

```http
GET /userProfiles?department=Engineering&status=active
```

This allows frontend applications to build filterable user lists without additional backend logic.

---

# ↕️ Sorting

The API supports sorting collection results.

Example:

```http
GET /userProfiles?_sort=salary&_order=desc
```

This returns users ordered by salary.

Sorting can be applied to fields supported by the data.

---

# 📄 Pagination

The API supports pagination using query parameters.

Example:

```http
GET /userProfiles?_page=1&_per_page=5
```

This requests the first page with five users per page.

Pagination is useful for implementing frontend:

- Page navigation
- Next/previous buttons
- Paginated tables
- Large data lists

---

# 🔗 Combining Query Features

Search, filtering, sorting, and pagination can be combined in a single request.

Example:

```http
GET /userProfiles?department=Engineering&status=active&_sort=salary&_order=desc&_page=1&_per_page=5
```

This allows the frontend to build realistic data-management interfaces using only API query parameters.

---

# 📊 User Data Structure

A user profile looks like:

```json
{
  "id": "usr_001",
  "firstName": "Aarav",
  "lastName": "Sharma",
  "email": "aarav.sharma@example.com",
  "phone": "+91 98765 43210",
  "role": "admin",
  "department": "Engineering",
  "jobTitle": "Engineering Manager",
  "status": "active",
  "location": {
    "city": "Ahmedabad",
    "state": "Gujarat",
    "country": "India"
  },
  "address": "120, C.G. Road",
  "joiningDate": "2022-06-10",
  "salary": 95000,
  "createdAt": "2022-06-10T09:30:00Z",
  "updatedAt": "2026-09-20T10:15:00Z"
}
```

---

# 🌍 Available Resources

Currently the API provides:

| Resource        | Purpose               |
| --------------- | --------------------- |
| `/users`        | Authentication users  |
| `/userProfiles` | Application user data |

---

# 🔐 Authentication vs User Profiles

These two resources serve different purposes.

### `/users`

Used by JSON Server Auth for:

- Registration
- Login
- Authentication
- Access tokens
- Authenticated requests

### `/userProfiles`

Used by the application for:

- User information
- User management
- CRUD operations
- Search
- Filtering
- Sorting
- Pagination

Keeping authentication data separate from application profile data makes the API easier to work with.

---

# 🧪 Quick API Test

After starting the server:

```bash
pnpm dev
```

Test the API:

```bash
curl http://localhost:9001/userProfiles
```

Or open:

```text
http://localhost:9001/userProfiles
```

in your browser.

---

# 🛠️ Useful Tools

The API can be used with any HTTP client, including:

- Postman
- Bruno
- Insomnia
- Thunder Client
- `fetch`
- Axios
- TanStack Query

Example using `fetch`:

```javascript
const response = await fetch("http://localhost:9001/userProfiles");

const users = await response.json();

console.log(users);
```

---

# 📌 API Summary

| Method   | Endpoint            | Purpose      |
| -------- | ------------------- | ------------ |
| `POST`   | `/login`            | Login        |
| `GET`    | `/userProfiles`     | Get users    |
| `GET`    | `/userProfiles/:id` | Get one user |
| `POST`   | `/userProfiles`     | Create user  |
| `PATCH`  | `/userProfiles/:id` | Update user  |
| `PUT`    | `/userProfiles/:id` | Replace user |
| `DELETE` | `/userProfiles/:id` | Delete user  |

### Query capabilities

```text
Search
Filter
Sort
Pagination
Combined queries
```

---

# 🚦 Base URL

For local development:

```text
http://localhost:9001
```

All API requests should use this base URL.

Example:

```text
http://localhost:9001/userProfiles
```

---

# ⚠️ Important

This API is intended for:

- Local development
- Learning
- Frontend development
- API integration practice
- Prototyping
- Testing

It is **not intended for production use**.

The data is stored locally in `db.json`, and authentication is provided by JSON Server Auth rather than a production authentication service.
