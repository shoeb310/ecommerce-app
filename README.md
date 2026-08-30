# Ecommerce App

A complete full-stack e-commerce application with a customer storefront, admin dashboard, and secure backend APIs. The project includes product management, cart handling, order placement, payment integration with Stripe and Razorpay, and role-based authentication.

## Overview

This project is built as a multi-app architecture:

- Frontend: customer-facing shopping website
- Admin: product and order management portal
- Backend: Express + MongoDB API server

The app supports:

- User registration and login
- Admin authentication
- Product listing and creation
- Product removal and details lookup
- Cart management
- Order placement via COD, Stripe, and Razorpay
- Order verification for payment gateways
- Admin order status updates
- Image upload to Cloudinary

---

## Tech Stack

### Frontend / Admin
- React.js
- Vite
- Tailwind CSS
- React Router DOM
- Axios
- React Toastify

### Backend
- Node.js
- Express.js
- MongoDB with Mongoose
- JWT Authentication
- Cloudinary
- Stripe
- Razorpay
- Multer
- Bcrypt

---

## Project Structure

```bash
ecommerce-app/
├── admin/
│   ├── public/
│   ├── src/
│   ├── index.html
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   └── vite.config.js
├── backend/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── package.json
│   ├── server.js
│   └── vercel.json
├── frontend/
│   ├── public/
│   ├── src/
│   ├── index.html
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   └── vite.config.js
├── .git/
└── README.md
```

---

## Features

### Customer Side
- Browse products by category and collection
- Search and view individual product details
- Add products to cart
- Update cart quantities
- Register and login as a user
- Place orders using:
  - Cash on Delivery (COD)
  - Stripe checkout
  - Razorpay checkout
- View purchase history and order status

### Admin Side
- Login as admin
- Add new products with multiple images
- Remove products from inventory
- List all products
- View all customer orders
- Update order status

---

## Environment Variables

Create a `.env` file inside the `backend` directory with the following values:

```env
PORT=4000
MONGODB_URI=mongodb://localhost:27017
JWT_SECRET=your_jwt_secret
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=your_admin_password
CLOUDINARY_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_SECRET_KEY=your_secret_key
STRIPE_SECRET_KEY=your_stripe_secret
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_secret
```

> The app connects MongoDB to the database name `e-commerce` automatically in the config file.

---

## Installation

### 1. Clone the repository

```bash
git clone <repository-url>
cd forever-full-stack
```

### 2. Install backend dependencies

```bash
cd backend
npm install
```

### 3. Install frontend dependencies

```bash
cd ../frontend
npm install
```

### 4. Install admin dependencies

```bash
cd ../admin
npm install
```

---

## Running the Project

### Start backend

```bash
cd backend
npm start
```

The backend runs on:

```bash
http://localhost:4000
```

### Start frontend

```bash
cd frontend
npm run dev
```

### Start admin panel

```bash
cd admin
npm run dev
```

---

## API Documentation

Base URL:

```bash
http://localhost:4000/api
```

### Authentication

The backend uses JWT tokens on protected routes. Tokens are passed in the `token` header.

---

## User APIs

### 1) Register User

**Endpoint:** `POST /api/user/register`

**Body:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "strongpassword"
}
```

**Response:**

```json
{
  "success": true,
  "token": "jwt_token_here"
}
```

---

### 2) Login User

**Endpoint:** `POST /api/user/login`

**Body:**

```json
{
  "email": "john@example.com",
  "password": "strongpassword"
}
```

**Response:**

```json
{
  "success": true,
  "token": "jwt_token_here"
}
```

---

### 3) Admin Login

**Endpoint:** `POST /api/user/admin`

**Body:**

```json
{
  "email": "admin@example.com",
  "password": "your_admin_password"
}
```

**Response:**

```json
{
  "success": true,
  "token": "jwt_token_here"
}
```

---

## Product APIs

### 4) Add Product

**Endpoint:** `POST /api/product/add`

**Headers:**

```http
token: <admin_jwt_token>
```

**Form Data:**
- `name`
- `description`
- `price`
- `category`
- `subCategory`
- `sizes`
- `bestseller`
- `image1`, `image2`, `image3`, `image4`

**Response:**

```json
{
  "success": true,
  "message": "Product Added"
}
```

---

### 5) Remove Product

**Endpoint:** `POST /api/product/remove`

**Headers:**

```http
token: <admin_jwt_token>
```

**Body:**

```json
{
  "id": "product_id"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Product Removed"
}
```

---

### 6) Get Product Details

**Endpoint:** `POST /api/product/single`

**Body:**

```json
{
  "productId": "product_id"
}
```

**Response:**

```json
{
  "success": true,
  "product": {
    "_id": "...",
    "name": "...",
    "description": "...",
    "price": 499,
    "category": "...",
    "subCategory": "...",
    "sizes": ["S", "M", "L"],
    "image": ["url1", "url2"]
  }
}
```

---

### 7) List All Products

**Endpoint:** `GET /api/product/list`

**Response:**

```json
{
  "success": true,
  "products": [
    {
      "_id": "...",
      "name": "...",
      "price": 999,
      "category": "...",
      "image": ["..."]
    }
  ]
}
```

---

## Cart APIs

### 8) Get User Cart

**Endpoint:** `POST /api/cart/get`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id"
}
```

**Response:**

```json
{
  "success": true,
  "cartData": {
    "product_id": {
      "S": 1,
      "M": 2
    }
  }
}
```

---

### 9) Add Item to Cart

**Endpoint:** `POST /api/cart/add`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "itemId": "product_id",
  "size": "M"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Added To Cart"
}
```

---

### 10) Update Cart Quantity

**Endpoint:** `POST /api/cart/update`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "itemId": "product_id",
  "size": "M",
  "quantity": 3
}
```

**Response:**

```json
{
  "success": true,
  "message": "Cart Updated"
}
```

---

## Order APIs

### 11) Place Order (Cash on Delivery)

**Endpoint:** `POST /api/order/place`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "items": [
    {
      "name": "Product Name",
      "price": 499,
      "quantity": 1
    }
  ],
  "amount": 499,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "country": "USA"
  }
}
```

**Response:**

```json
{
  "success": true,
  "message": "Order Placed"
}
```

---

### 12) Place Order with Stripe

**Endpoint:** `POST /api/order/stripe`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "items": [
    {
      "name": "Product Name",
      "price": 499,
      "quantity": 1
    }
  ],
  "amount": 499,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "country": "USA"
  }
}
```

**Response:**

```json
{
  "success": true,
  "session_url": "https://checkout.stripe.com/..."
}
```

---

### 13) Verify Stripe Payment

**Endpoint:** `POST /api/order/verifyStripe`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "orderId": "order_id",
  "success": "true",
  "userId": "user_id"
}
```

**Response:**

```json
{
  "success": true
}
```

---

### 14) Place Order with Razorpay

**Endpoint:** `POST /api/order/razorpay`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "items": [
    {
      "name": "Product Name",
      "price": 499,
      "quantity": 1
    }
  ],
  "amount": 499,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "country": "USA"
  }
}
```

**Response:**

```json
{
  "success": true,
  "order": {
    "id": "razorpay_order_id",
    "amount": 49900,
    "currency": "INR"
  }
}
```

---

### 15) Verify Razorpay Payment

**Endpoint:** `POST /api/order/verifyRazorpay`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id",
  "razorpay_order_id": "razorpay_order_id"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Payment Successful"
}
```

---

### 16) Get User Orders

**Endpoint:** `POST /api/order/userorders`

**Headers:**

```http
token: <user_jwt_token>
```

**Body:**

```json
{
  "userId": "user_id"
}
```

**Response:**

```json
{
  "success": true,
  "orders": [
    {
      "_id": "...",
      "userId": "...",
      "items": [],
      "amount": 999,
      "status": "Order Placed",
      "paymentMethod": "COD"
    }
  ]
}
```

---

### 17) Get All Orders (Admin)

**Endpoint:** `POST /api/order/list`

**Headers:**

```http
token: <admin_jwt_token>
```

**Response:**

```json
{
  "success": true,
  "orders": []
}
```

---

### 18) Update Order Status (Admin)

**Endpoint:** `POST /api/order/status`

**Headers:**

```http
token: <admin_jwt_token>
```

**Body:**

```json
{
  "orderId": "order_id",
  "status": "Shipped"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Status Updated"
}
```

---

## Authentication Notes

- User authentication is implemented using JWT.
- Protected routes check the `token` header.
- Admin authentication uses a custom middleware that validates admin credentials against the secret environment values.

---

## Default App Behavior

- Backend runs on port `4000`.
- Frontend uses Vite dev server by default.
- Admin app uses a separate Vite dev server.
- MongoDB database name is `e-commerce`.
- Product images are uploaded to Cloudinary.

---

## License

This project is available for learning and personal use. Add your own license if you plan to publish it publicly.

---

## Developer Notes

This project is a strong example of a small but complete e-commerce stack with:

- separate frontend and admin apps
- secure route protection
- MongoDB data models
- external payment integrations
- cloud-based media uploads
- role-driven workflows

If you want, this app can be extended with:

- wishlist support
- category filters
- reviews and ratings
- email verification
- pagination
- admin analytics dashboard
- deployment to Vercel / Render / Railway

---

## Project Status

This repository contains a working full-stack e-commerce application structure with customer storefront, admin management portal, and REST API backend.
