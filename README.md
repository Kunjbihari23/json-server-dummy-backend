# User Management API

A lightweight local REST API built with **JSON Server** and **JSON Server Auth**.

This API provides a ready-to-use backend for frontend development, testing, learning, and API integration without requiring a real backend or database.

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
- 🔒 JWT-protected API resources

Everything runs locally using a `db.json` file.

---

# ⚙️ Requirements

Make sure you have installed:

- Node.js
- pnpm

Check your versions:

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

# 📁 Project Structure

```text
Dummy Backend/
│
├── db.json
├── routes.json
├── package.json
├── pnpm-lock.yaml
└── node_modules/
```

### `db.json`

Contains all API data and collections.

### `routes.json`

Defines authentication and authorization rules for API resources.

### `package.json`

Contains project dependencies and scripts.

---

# 💾 Data Storage

All API data is stored in:

```text
db.json
```

There is no external database.

Changes made through:

- `POST`
- `PATCH`
- `PUT`
- `DELETE`

are persisted directly to the JSON file.

Example:

```json
{
  "users": [],
  "userProfiles": []
}
```

Each top-level array represents an API resource.

---

# 🔐 Authentication

The API uses **JSON Server Auth** for authentication.

Authentication is based on the `users` collection inside `db.json`.

The authentication flow is:

```text
Register
   ↓
User created
   ↓
Login
   ↓
JWT access token
   ↓
Send token with API requests
   ↓
Protected API access
```

---

# 📝 Register

Registration is a public endpoint.

```http
POST /register
```

Example:

```http
POST http://localhost:9001/register
Content-Type: application/json
```

Request body:

```json
{
  "email": "admin@example.com",
  "password": "admin123"
}
```

A successful registration creates a user in the `users` collection and returns an access token.

The password is stored as a password hash rather than the original plaintext password.

---

# 🔑 Login

Login is also a public endpoint.

```http
POST /login
```

Example:

```http
POST http://localhost:9001/login
Content-Type: application/json
```

Request:

```json
{
  "email": "admin@example.com",
  "password": "admin123"
}
```

A successful login returns an authentication token.

Example:

```json
{
  "accessToken": "YOUR_ACCESS_TOKEN",
  "user": {
    "email": "admin@example.com",
    "id": 1
  }
}
```

Save the `accessToken` and use it for protected API requests.

---

# 🔒 Protected API Requests

Protected APIs require the JWT access token.

Send the token using the `Authorization` header:

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

const users = await response.json();
```

Requests without a valid token will be rejected.

---

# 🛡️ API Authentication Rules

The current API uses the following authentication rules:

| Endpoint                   | Authentication |
| -------------------------- | -------------- |
| `POST /register`           | ❌ Public      |
| `POST /login`              | ❌ Public      |
| `GET /userProfiles`        | 🔐 Required    |
| `GET /userProfiles/:id`    | 🔐 Required    |
| `POST /userProfiles`       | 🔐 Required    |
| `PATCH /userProfiles/:id`  | 🔐 Required    |
| `PUT /userProfiles/:id`    | 🔐 Required    |
| `DELETE /userProfiles/:id` | 🔐 Required    |

The authentication rules are configured through:

```text
routes.json
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

- First name
- Last name
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

```http
GET http://localhost:9001/userProfiles
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Returns an array of users.

---

# 👤 Get a Single User

Retrieve a user using their ID:

```http
GET /userProfiles/:id
```

Example:

```http
GET http://localhost:9001/userProfiles/usr_001
Authorization: Bearer YOUR_ACCESS_TOKEN
```

---

# ➕ Create a User

Create a new user:

```http
POST /userProfiles
```

Example:

```http
POST http://localhost:9001/userProfiles
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Request body:

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

The new record is saved to `db.json`.

---

# ✏️ Update a User

Update an existing user:

```http
PATCH /userProfiles/:id
```

Example:

```http
PATCH http://localhost:9001/userProfiles/usr_001
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
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

```http
DELETE http://localhost:9001/userProfiles/usr_001
Authorization: Bearer YOUR_ACCESS_TOKEN
```

The record is removed from `db.json`.

---

# 🔎 Search & Query Parameters

The API supports JSON Server query parameters.

For example:

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

Example:

```http
GET /userProfiles?email_like=@example.com
Authorization: Bearer YOUR_ACCESS_TOKEN
```

---

# 🎯 Filtering

Users can be filtered by field values.

Example:

```http
GET /userProfiles?department=Engineering
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Multiple filters can also be combined:

```http
GET /userProfiles?department=Engineering&status=active
Authorization: Bearer YOUR_ACCESS_TOKEN
```

This allows frontend applications to build filterable user lists without additional backend logic.

---

# ↕️ Sorting

The API supports sorting collection results.

Example:

```http
GET /userProfiles?_sort=salary&_order=desc
Authorization: Bearer YOUR_ACCESS_TOKEN
```

This returns users ordered by salary.

Sorting can be applied to fields supported by the data.

---

# 📄 Pagination

The API supports pagination using query parameters.

Example:

```http
GET /userProfiles?_page=1&_per_page=5
Authorization: Bearer YOUR_ACCESS_TOKEN
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
Authorization: Bearer YOUR_ACCESS_TOKEN
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

# 🧩 How to Add a New API Resource

One of the main benefits of this local API is that new resources can be added very easily.

For example, if you want to create a **Cart API**, you don't need to create a new backend server.

Simply add a new collection to `db.json`.

---

## 🛒 Example: Add Cart

Open:

```text
db.json
```

Add a `cart` collection:

```json
{
  "users": [],
  "userProfiles": [],
  "cart": []
}
```

Now JSON Server automatically provides REST endpoints for the new collection.

You can use:

```text
GET     /cart
GET     /cart/:id
POST    /cart
PATCH   /cart/:id
PUT     /cart/:id
DELETE  /cart/:id
```

---

## 🛒 Add Cart Data

You can add initial data:

```json
{
  "users": [],
  "userProfiles": [],
  "cart": [
    {
      "id": "cart_001",
      "userId": "usr_001",
      "items": [
        {
          "productId": "prod_001",
          "quantity": 2,
          "price": 499
        }
      ],
      "total": 998,
      "status": "active"
    }
  ]
}
```

Now you can request:

```http
GET /cart
```

or:

```http
GET /cart?userId=usr_001
```

---

# 🛍️ Example: Add Products

To create a products API, add:

```json
{
  "users": [],
  "userProfiles": [],
  "cart": [],
  "products": []
}
```

Example product:

```json
{
  "id": "prod_001",
  "name": "Wireless Keyboard",
  "price": 1499,
  "category": "electronics",
  "stock": 25,
  "status": "active"
}
```

You automatically get:

```text
GET     /products
GET     /products/:id
POST    /products
PATCH   /products/:id
PUT     /products/:id
DELETE  /products/:id
```

---

# 📦 Example: Add Orders

Add another collection:

```json
{
  "users": [],
  "userProfiles": [],
  "cart": [],
  "products": [],
  "orders": []
}
```

Example:

```json
{
  "id": "order_001",
  "userId": "usr_001",
  "items": [
    {
      "productId": "prod_001",
      "quantity": 2,
      "price": 1499
    }
  ],
  "total": 2998,
  "status": "pending",
  "createdAt": "2026-09-28T10:30:00Z"
}
```

This automatically creates:

```text
GET     /orders
GET     /orders/:id
POST    /orders
PATCH   /orders/:id
PUT     /orders/:id
DELETE  /orders/:id
```

---

# 🔐 Protecting a New Resource

Adding a collection to `db.json` makes the resource available through JSON Server.

If the resource should also require authentication, add it to:

```text
routes.json
```

For example:

```json
{
  "userProfiles": 660,
  "cart": 660,
  "products": 660,
  "orders": 660,
  "users": 600
}
```

Now these resources require authentication:

```text
/userProfiles
/cart
/products
/orders
```

A request without a valid JWT will be rejected.

---

# 🌐 Public vs Protected Resources

Not every resource has to be protected.

For example, if products should be publicly readable but only authenticated users can modify them, configure the appropriate JSON Server Auth permissions in `routes.json`.

For resources that contain private application data, use protected permissions.

The permission configuration is controlled by `routes.json`.

---

# 🧱 Creating Your Own Resource

Whenever you need another API resource, follow these steps:

### 1. Add a collection

Open:

```text
db.json
```

Example:

```json
{
  "users": [],
  "userProfiles": [],
  "products": [],
  "cart": [],
  "orders": [],
  "notifications": []
}
```

### 2. Add initial data if required

```json
{
  "id": "notification_001",
  "userId": "usr_001",
  "title": "New message",
  "message": "You have a new notification.",
  "read": false
}
```

### 3. Configure authentication

If the resource should require login, add it to:

```text
routes.json
```

Example:

```json
{
  "userProfiles": 660,
  "products": 660,
  "cart": 660,
  "orders": 660,
  "notifications": 660,
  "users": 600
}
```

### 4. Restart the API if necessary

```bash
pnpm dev
```

### 5. Use the generated REST endpoints

For a collection named:

```text
notifications
```

you automatically get:

```text
GET     /notifications
GET     /notifications/:id
POST    /notifications
PATCH   /notifications/:id
PUT     /notifications/:id
DELETE  /notifications/:id
```

No additional controller, route file, model, or database setup is required.

---

# 📚 Resource Examples

The same approach can be used to create many different resources:

```text
users
userProfiles
products
categories
cart
orders
orderItems
payments
notifications
messages
comments
posts
blogs
tasks
projects
employees
departments
courses
students
teachers
```

For example:

```json
{
  "users": [],
  "userProfiles": [],
  "products": [],
  "categories": [],
  "cart": [],
  "orders": [],
  "notifications": []
}
```

Each collection becomes an API resource.

---

# 🌍 Available Resources

The default API provides:

| Resource        | Purpose               |
| --------------- | --------------------- |
| `/users`        | Authentication users  |
| `/userProfiles` | Application user data |

Additional resources can be added by creating new collections in `db.json`.

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

First register or login to obtain an access token.

Then test the protected API:

```bash
curl \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  http://localhost:9001/userProfiles
```

Or using JavaScript:

```javascript
const response = await fetch("http://localhost:9001/userProfiles", {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});

const users = await response.json();

console.log(users);
```

Opening the protected endpoint directly in a browser without an authentication token will not provide the protected data.

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
const response = await fetch("http://localhost:9001/userProfiles", {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});

const users = await response.json();

console.log(users);
```

---

# 📌 API Summary

| Method   | Endpoint            | Authentication | Purpose      |
| -------- | ------------------- | -------------- | ------------ |
| `POST`   | `/register`         | ❌ Public      | Register     |
| `POST`   | `/login`            | ❌ Public      | Login        |
| `GET`    | `/userProfiles`     | 🔐 Required    | Get users    |
| `GET`    | `/userProfiles/:id` | 🔐 Required    | Get one user |
| `POST`   | `/userProfiles`     | 🔐 Required    | Create user  |
| `PATCH`  | `/userProfiles/:id` | 🔐 Required    | Update user  |
| `PUT`    | `/userProfiles/:id` | 🔐 Required    | Replace user |
| `DELETE` | `/userProfiles/:id` | 🔐 Required    | Delete user  |

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

---

# 📖 More Information

For complete information about authentication, authorization, route permissions, registration, login, JWT handling, and other JSON Server Auth features, see the official documentation:

**JSON Server Auth**

https://www.npmjs.com/package/json-server-auth
