# ✨ Aura Atelier — Luxury E-Commerce Platform

[![MERN Stack](https://img.shields.io/badge/Stack-MERN-gold.svg)](https://react.dev)
[![Node.js](https://img.shields.io/badge/Node.js-v18+-339933.svg?logo=nodedotjs)](https://nodejs.org/)
[![React](https://img.shields.io/badge/React-v19-61DAFB.svg?logo=react)](https://react.dev)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248.svg?logo=mongodb)](https://www.mongodb.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Author](https://img.shields.io/badge/Author-Ruthvik%20Gowda-black.svg)](#-author)

An artisanal, production-grade luxury e-commerce web platform engineered with **React 19**, **Node.js**, **Express 5**, **MongoDB**, and **Razorpay Standard Checkout**. Designed with bespoke luxury dark aesthetics, real-time inventory synchronization, and cryptographic payment verification.

---

## 🌟 Key Features

### 🔐 Authentication & Session Security
- **Bcrypt Password Hashing**: Passwords salt-hashed (cost factor 10) before storage.
- **HttpOnly Cookie Authentication**: JWT access tokens stored exclusively in secure, HttpOnly cookies with CSRF/XSS protection.
- **Protected Route Guards**: Custom Express authentication middleware validating active sessions for customer resources.

### 🛍️ Dynamic Product Catalogue
- **Live Search & Filtering**: Server-side multi-parameter filtering (category, search keywords, price/name sorting) with ReDoS protection.
- **Inventory Tracking**: Real-time stock state validation (In Stock, Low Stock, and Out of Stock indicators).
- **Curated Atelier Highlights**: Editorial showcases and featured collections.

### 💖 Wishlist & Favorites
- **Persistent Mongoose References**: Customer wishlist stored as MongoDB ObjectId references (`ref: "Product"`).
- **Optimistic State Toggles**: Add, remove, and toggle favorites with duplicate prevention (`409 Conflict` resolution).

### 🛒 Global Shopping Cart & Checkout
- **Reactive State Management**: Global cart state powered by React Context (`CartContext`) without prop drilling.
- **Live Stock Validation**: Client and server-side stock re-verification to prevent overselling or race conditions.
- **Razorpay Standard Checkout**: Complete checkout flow with HMAC-SHA256 signature verification and order snapshot architecture.
- **Fulfillment Lifecycle**: Order status progression (`PLACED` → `CONFIRMED` → `SHIPPED` → `DELIVERED`).

---

## 🏗️ Architecture & Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React 19, React Router v7, Vite 8, Axios, Lucide Icons, Bespoke Luxury CSS |
| **Backend** | Node.js, Express.js 5, Mongoose 9, Razorpay SDK, Cookie Parser, JSON Web Token (JWT) |
| **Database** | MongoDB Atlas (with automatic in-memory fallback for offline/instant local development) |
| **Security** | Bcrypt, HttpOnly Cookies, CORS Whitelisting, HMAC-SHA256 Signatures |

---

## 📁 Repository Structure

```
aura-atelier/
├── backend/
│   ├── config/              # Razorpay & service configuration
│   ├── controllers/         # Business logic (Customer, Product, Cart, Wishlist, Order)
│   ├── middlewares/         # JWT Auth verification middleware
│   ├── models/              # Mongoose data schemas (Customer, Product, Order)
│   ├── routes/              # Express endpoint routers
│   ├── utils/               # JWT token generator & cryptographic helpers
│   ├── index.js             # Express server entry point & DB connector
│   └── seed.js              # Catalogue seeding script
├── frontend/
│   ├── public/              # Brand SVG assets & favicon
│   ├── src/
│   │   ├── components/      # Navbar, ProductCard, CartItem, SearchBar, WishlistCard
│   │   ├── context/         # CartContext reactive store
│   │   ├── pages/           # Home, Catalogue, Details, Wishlist, Cart, Checkout, Orders
│   │   ├── services/        # Centralized Axios API service layer
│   │   ├── App.jsx          # Route hierarchy & layouts
│   │   └── index.css        # Custom luxury typography & styling tokens
│   └── vite.config.js       # Vite build configuration
├── package.json             # Root workspace scripts
└── README.md
```

---

## 🚀 Quick Start Guide

### 1. Prerequisites
- **Node.js** v18+ installed on your machine
- **npm** or **yarn**
- *(Optional)* Free **MongoDB Atlas** cluster & **Razorpay** test account

### 2. Installation

Clone your repository and install dependencies:

```bash
# Clone the repository
git clone https://github.com/Ruthvik-Gowda-18/aura-atelier.git
cd aura-atelier

# Install dependencies for both backend and frontend
npm --prefix backend install
npm --prefix frontend install
```

### 3. Environment Configuration

#### Backend Configuration:
Create `backend/.env` (a template is available in `backend/.env.example`):
```env
PORT=5000
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/aura_atelier?retryWrites=true&w=majority
JWT_SECRET=your_cryptographically_secure_jwt_secret_here
RAZORPAY_KEY_ID=rzp_test_your_key_id
RAZORPAY_KEY_SECRET=your_razorpay_secret
CLIENT_URL=http://localhost:5173
```
> *Note: If `MONGO_URI` is left blank, the backend automatically spins up an in-memory MongoDB database so you can develop and test immediately.*

#### Frontend Configuration:
Create `frontend/.env` (a template is available in `frontend/.env.example`):
```env
VITE_API_BASE_URL=http://localhost:5000
VITE_APP_NAME="Aura Atelier"
```

### 4. Seed Catalogue Data
Populate the database with the initial collection:
```bash
npm run seed
```

### 5. Run the Application

In terminal 1 (start Backend API):
```bash
npm run backend
```

In terminal 2 (start Frontend Vite Server):
```bash
npm run dev
```

The application will be live at:
- **Frontend**: [http://localhost:5173](http://localhost:5173)
- **Backend API**: [http://localhost:5000](http://localhost:5000)

---

## 📡 API Reference Overview

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :---: |
| `POST` | `/customers/register` | Register new customer account | No |
| `POST` | `/customers/login` | Login and set HttpOnly JWT cookie | No |
| `GET` | `/customers/me` | Fetch authenticated customer profile | Yes |
| `POST` | `/customers/logout` | Clear session cookie | Yes |
| `GET` | `/products` | Filter & search products | No |
| `GET` | `/products/:id` | Get individual product details | No |
| `GET` | `/wishlist` | Retrieve customer wishlist | Yes |
| `PATCH`| `/wishlist/:id/toggle` | Toggle item in customer wishlist | Yes |
| `GET` | `/cart` | Get customer shopping cart | Yes |
| `POST` | `/cart/:productId` | Add item / increment quantity in cart | Yes |
| `PATCH`| `/cart/:productId` | Update cart item quantity | Yes |
| `DELETE`| `/cart/:productId` | Remove item from cart | Yes |
| `POST` | `/orders/create-payment-order` | Validate stock & create Razorpay order | Yes |
| `POST` | `/orders/verify-payment` | Verify HMAC signature & confirm order | Yes |
| `GET` | `/orders` | Retrieve customer order history | Yes |
| `GET` | `/orders/:id` | Get individual order details & status | Yes |

---

## 👤 Author

**Ruthvik Gowda**
- **GitHub**: [@Ruthvik-Gowda-18](https://github.com/Ruthvik-Gowda-18)

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
