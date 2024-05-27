# Real-Time Bidding Platform

## Overview
This is a real-time bidding platform built with Node.js, Express, Socket.io, and MySQL. It supports user registration and authentication, role-based access control, CRUD operations for auction items, real-time bidding with notifications, image uploads, pagination, search, and filtering.

## Features
- User registration and authentication
- Role-based access control
- CRUD operations for auction items
- Real-time bidding with notifications
- Image upload for auction items
- Pagination, search, and filtering for auction items

## Prerequisites
- Node.js and npm
- MySQL
- Git

## Setup and Usage

### 1. Clone the Repository
Clone the project repository from GitHub:
```bash
git clone https://github.com/yourusername/real-time-bidding-platform.git
cd real-time-bidding-platform

2. Install Dependencies
Install the necessary dependencies using npm:
npm install
3. Configure Environment Variables
Create a .env file in the root directory and add the following variables:
DB_HOST=localhost
DB_NAME=bidding_db
DB_USER=root
DB_PASS=password
JWT_SECRET=your_jwt_secret

4. Set Up the Database
Create a MySQL database and run the provided SQL script to set up the database schema. Create a file sql/schema.sql with the following content:

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    role VARCHAR(50) DEFAULT 'user',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    starting_price DECIMAL(10, 2) NOT NULL,
    current_price DECIMAL(10, 2) DEFAULT starting_price,
    image_url VARCHAR(255),
    end_time TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE bids (
    id INT AUTO_INCREMENT PRIMARY KEY,
    item_id INT,
    user_id INT,
    bid_amount DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (item_id) REFERENCES items(id),
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE notifications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    message VARCHAR(255) NOT NULL,
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

5. Start the Server
Start the server using the following command:
npm start

6. API Endpoints
#Users
Register a new user: POST /users/register
Authenticate a user and return a token: POST /users/login
Get the profile of the logged-in user: GET /users/profile (Authenticated users only)

#Items
Retrieve all auction items (with pagination): GET /items
Retrieve a single auction item by ID: GET /items/:id
Create a new auction item: POST /items (Authenticated users, image upload)
Update an auction item by ID: PUT /items/:id (Authenticated users, only item owners or admins)
Delete an auction item by ID: DELETE /items/:id (Authenticated users, only item owners or admins)


#Bids
Retrieve all bids for a specific item: GET /items/:itemId/bids
Place a new bid on a specific item: POST /items/:itemId/bids (Authenticated users)

#Notifications
Retrieve notifications for the logged-in user: GET /notifications
Mark notifications as read: POST /notifications/mark-read

7. Testing
Run tests using the following command:
npm test

