The following is a professional and informative `README.md` file for your MERN E-commerce Backend API.

---

# E-commerce API Backend

[![Node.js](https://img.shields.io/badge/Node.js-18.x-green?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-4.x-blue?style=for-the-badge&logo=express)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-4.x%2B-green?style=for-the-badge&logo=mongodb)](https://www.mongodb.com/)
[![JWT](https://img.shields.io/badge/JWT-Authentication-purple?style=for-the-badge&logo=json-web-tokens)](https://jwt.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Table of Contents

*   [Project Description](#project-description)
*   [Features](#features)
*   [Technologies Used](#technologies-used)
*   [API Endpoints](#api-endpoints)
    *   [Authentication & Authorization](#authentication--authorization)
    *   [Users](#users)
    *   [Categories](#categories)
    *   [Products](#products)
    *   [Orders](#orders)
*   [Installation](#installation)
    *   [Prerequisites](#prerequisites)
    *   [Setup Steps](#setup-steps)
    *   [Environment Variables](#environment-variables)
*   [Usage](#usage)
*   [License](#license)

## Project Description

This repository contains the backend API for a robust e-commerce application. Built with Node.js, Express.js, and MongoDB, it provides a comprehensive set of RESTful endpoints to manage products, categories, user accounts, and customer orders. The API includes features like secure user authentication using JSON Web Tokens (JWT), role-based authorization for administrative tasks, and dynamic image uploading for product management.

This backend serves as the core data and business logic layer for a full-stack MERN (MongoDB, Express, React, Node.js) e-commerce solution.

## Features

*   **User Management:**
    *   User registration and login with JWT authentication.
    *   Password hashing using `bcryptjs` for security.
    *   Ability to retrieve user details and user counts.
    *   Role-based access control (Admins only for most user management operations).
*   **Product Management:**
    *   Full CRUD (Create, Read, Update, Delete) operations for products.
    *   Support for product images and image galleries using `multer`.
    *   Filtering products by category.
    *   Retrieving featured products.
    *   Getting total product count.
*   **Category Management:**
    *   Full CRUD operations for product categories.
    *   Retrieving category details and total category count.
*   **Order Management:**
    *   Creation of new orders with multiple order items.
    *   Calculation of total price for orders.
    *   Updating order status.
    *   Retrieving all orders, specific orders, and user-specific orders.
    *   Calculating total sales revenue.
*   **Authentication & Authorization:**
    *   JWT-based authentication for secure API access.
    *   Middleware for validating tokens.
    *   Custom error handling for authentication failures.
    *   **Note on Authorization:** Based on the `jwt.js` helper, only users with `isAdmin: true` in their JWT payload are authorized to access protected routes. Regular user tokens will be revoked for protected routes.
*   **Database Integration:**
    *   MongoDB with Mongoose ODM for data modeling and interaction.
*   **API Utilities:**
    *   `morgan` for request logging.
    *   `cors` for cross-origin requests.
    *   `dotenv` for secure environment variable management.

## Technologies Used

*   **Node.js**: JavaScript runtime environment.
*   **Express.js**: Fast, unopinionated, minimalist web framework for Node.js.
*   **MongoDB**: NoSQL database for flexible data storage.
*   **Mongoose**: MongoDB object data modeling (ODM) for Node.js.
*   **JSON Web Tokens (JWT)**: For stateless user authentication.
*   **Bcrypt.js**: For hashing passwords securely.
*   **Multer**: Node.js middleware for handling `multipart/form-data`, primarily used for uploading files.
*   **Morgan**: HTTP request logger middleware for node.js.
*   **CORS**: Middleware for enabling Cross-Origin Resource Sharing.
*   **Dotenv**: For loading environment variables from a `.env` file.

## API Endpoints

The base URL for the API is `http://localhost:<PORT>/api/v1` (where `<PORT>` is typically 3000, and `/api/v1` is defined in your `.env` as `API_URL`).

### Authentication & Authorization

*   Routes marked **(Public)** do not require a JWT.
*   Routes marked **(Authenticated)** require a valid JWT in the `Authorization: Bearer <token>` header.
*   Routes marked **(Admin Only)** require an `isAdmin: true` flag in the JWT payload. Non-admin tokens will be revoked.

### Users

| Method | Endpoint                    | Description                                                                                                                              | Authentication    |
| :----- | :-------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :---------------- |
| `GET`  | `/users`                    | Retrieve a list of all users (excluding password hashes).                                                                                | (Public)          |
| `GET`  | `/users/:id`                | Retrieve details of a specific user by ID (excluding password hash).                                                                     | (Public)          |
| `POST` | `/users`                    | Create a new user.                                                                                                                       | (Public)          |
| `POST` | `/users/register`           | Register a new user account.                                                                                                             | (Public)          |
| `POST` | `/users/login`              | Authenticate a user with email and password, returning a JWT if successful.                                                              | (Public)          |
| `DELETE` | `/users/:id`              | Delete a user account by ID.                                                                                                             | (Public)          |
| `GET`  | `/users/get/count`          | Get the total number of users.                                                                                                           | (Public)          |

_**Note on User Routes Security:** As per the `jwt.js` configuration, all user-related routes (`/api/v1/users(.*)`) are explicitly excluded from JWT authentication, making them publicly accessible. For a production environment, you would typically protect `POST /users`, `GET /users`, `GET /users/:id`, and `DELETE /users/:id` for admin-only access, and restrict `PUT /users/:id` to the logged-in user or admin._

### Categories

| Method | Endpoint                     | Description                                            | Authentication |
| :----- | :--------------------------- | :----------------------------------------------------- | :------------- |
| `GET`  | `/categories`                | Retrieve a list of all product categories.             | (Public)       |
| `GET`  | `/categories/:id`            | Retrieve details of a specific category by ID.         | (Public)       |
| `POST` | `/categories`                | Create a new category.                                 | (Admin Only)   |
| `PUT`  | `/categories/:id`            | Update an existing category by ID.                     | (Admin Only)   |
| `DELETE` | `/categories/:id`          | Delete a category by ID.                               | (Admin Only)   |
| `GET`  | `/categories/get/count`      | Get the total number of categories.                    | (Admin Only)   |

### Products

| Method | Endpoint                         | Description                                                                    | Authentication |
| :----- | :------------------------------- | :----------------------------------------------------------------------------- | :------------- |
| `GET`  | `/products`                      | Retrieve a list of all products. Can filter by `?category=id1,id2`.          | (Public)       |
| `GET`  | `/products/:id`                  | Retrieve details of a specific product by ID, with category populated.         | (Public)       |
| `POST` | `/products`                      | Create a new product. Requires `multipart/form-data` for image upload.         | (Admin Only)   |
| `PUT`  | `/products/:id`                  | Update an existing product by ID. Can update image via `multipart/form-data`.  | (Admin Only)   |
| `DELETE` | `/products/:id`                | Delete a product by ID.                                                        | (Admin Only)   |
| `GET`  | `/products/get/count`            | Get the total number of products.                                              | (Public)       |
| `GET`  | `/products/get/featured/:count?` | Retrieve featured products. `count` is optional (default: 1).                  | (Public)       |
| `PUT`  | `/products/gallery-images/:id`   | Update a product's image gallery. Requires `multipart/form-data` for `image[]`. | (Admin Only)   |

### Orders

| Method | Endpoint                        | Description                                                                      | Authentication |
| :----- | :------------------------------ | :------------------------------------------------------------------------------- | :------------- |
| `GET`  | `/orders`                       | Retrieve a list of all orders, populated with user and product details.          | (Admin Only)   |
| `GET`  | `/orders/:id`                   | Retrieve details of a specific order by ID, populated.                           | (Admin Only)   |
| `POST` | `/orders`                       | Create a new order. Requires `orderItems` (array of `{ quantity, product }`).   | (Authenticated)|
| `PUT`  | `/orders/:id`                   | Update the status of an order by ID.                                             | (Admin Only)   |
| `DELETE` | `/orders/:id`                 | Delete an order by ID, including its associated order items.                     | (Admin Only)   |
| `GET`  | `/orders/get/totalsales`        | Get the total sales revenue across all orders.                                   | (Admin Only)   |
| `GET`  | `/orders/get/count`             | Get the total number of orders.                                                  | (Admin Only)   |
| `GET`  | `/orders/get/userorders/:userid` | Retrieve all orders for a specific user ID.                                      | (Authenticated)|

## Installation

Follow these steps to set up and run the backend locally.

### Prerequisites

*   Node.js (v18 or higher recommended)
*   MongoDB instance (local or cloud-hosted, e.g., MongoDB Atlas)

### Setup Steps

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd <repository_url>/backend
    ```
    (Replace `<repository_url>` with the actual URL of your repository)

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Create a `.env` file:**
    In the `backend` directory, create a file named `.env` and configure your environment variables as described in the next section.

4.  **Start the server:**
    ```bash
    npm start
    ```
    or for development with auto-restarts:
    ```bash
    nodemon app.js
    ```
    The server will typically run on `http://localhost:3000` unless you specify a different `PORT` in your `.env` file.

### Environment Variables

Create a `.env` file in the `backend` directory with the following variables:

```env
PORT=3000
CONNECTION_STRING="mongodb+srv://<username>:<password>@<cluster_url>/<database_name>?retryWrites=true&w=majority"
API_URL=/api/v1
secret=YOUR_SUPER_SECRET_JWT_KEY
```

*   **`PORT`**: The port on which the Express server will run. Default is `3000`.
*   **`CONNECTION_STRING`**: Your MongoDB connection string. This should point to your MongoDB database (e.g., from MongoDB Atlas or a local instance). Replace `<username>`, `<password>`, `<cluster_url>`, and `<database_name>` with your actual MongoDB credentials and details.
*   **`API_URL`**: The base path for your API endpoints. `/api/v1` is recommended.
*   **`secret`**: A strong, secret key used for signing and verifying JWTs. **Do not use a simple string like `YOUR_SUPER_SECRET_JWT_KEY` in production.** Generate a complex, random string.

## Usage

Once the server is running, you can interact with the API using tools like [Postman](https://www.postman.com/downloads/) or [Insomnia](https://insomnia.rest/download), or by connecting a frontend application.

1.  **Register a User:**
    *   `POST` to `http://localhost:3000/api/v1/users/register`
    *   Body: `raw` `JSON`
        ```json
        {
            "name": "Admin User",
            "email": "admin@example.com",
            "password": "your_strong_password",
            "phone": "1234567890",
            "isAdmin": true,
            "apartment": "1A",
            "zip": "12345",
            "city": "Anytown",
            "country": "USA"
        }
        ```
    *   Set `isAdmin: true` for an admin user to test protected routes.

2.  **Login to get a Token:**
    *   `POST` to `http://localhost:3000/api/v1/users/login`
    *   Body: `raw` `JSON`
        ```json
        {
            "email": "admin@example.com",
            "password": "your_strong_password"
        }
        ```
    *   The response will contain a `token`. Copy this token.

3.  **Access Protected Routes:**
    *   For any route requiring authentication (especially "Admin Only" routes), include the obtained JWT in the `Authorization` header:
        `Authorization: Bearer <your_jwt_token>`

Example: Create a Category (Admin Only)
*   `POST` to `http://localhost:3000/api/v1/categories`
*   Headers:
    *   `Content-Type: application/json`
    *   `Authorization: Bearer <your_jwt_token>`
*   Body: `raw` `JSON`
    ```json
    {
        "name": "Electronics",
        "icon": "fa fa-laptop",
        "color": "blue"
    }
    ```

---
