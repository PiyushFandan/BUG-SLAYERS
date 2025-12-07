# BUG-SLAYERS
Retail Portal For Pizza and Cold Drinks

________________________________________
High Level Design (HLD) – Retail Pizza  & Cold Drinks Ordering System
________________________________________
1. Introduction
This document describes the High-Level Design (HLD) for a retail pizza ordering platform.
 The system allows customers to browse menu categories, view products, add items to a cart, and place carryout or delivery orders. The backend manages inventory, processes orders, and optionally sends confirmation emails. API security is implemented using an API key (x-api-key) in request headers.
The scope aligns with the MVP described in the problem statement.
________________________________________
2. Overall System Architecture
   
The platform follows a classic 3-tier architecture:
1.	Frontend Layer → React.js

2.	Backend Layer → REST API (Node.js + Express )

3.	Database Layer → Mongo DB

Optional supporting components:
●	Email Service (Nodemailer / SMTP)

●	CI/CD using GitHub Actions

●	Authentication service (for order history)

________________________________________
2.1 Architecture Diagram (Copy-Paste Friendly)
+-------------------------------------------------------------+
|                        Client Browser                       |
|                         (React.js)                          |
+---------------------------+---------------------------------+
                            |
                            | HTTPS Requests
                            v
+-------------------------------------------------------------+
|                     Backend API Service                     |
|                        (Node.js,  Express)                  |
|-------------------------------------------------------------|
|  • Menu API                                                  |
|  • Cart API                                                  |
|  • Order API                                                 |
|  • Inventory Management                                      |
|  • Authentication                               		   |
|  • Security Middleware (x-api-key)                           |
|  • Email Service Integration                                 |
|-------------------------------------------------------------|
+---------------------------+----------------------------------+
                            |
                            | SQL Queries
                            v
+-------------------------------------------------------------+
|                          MOngo DB                           |
|     users, categories, products, carts, cart_items,         |
|     orders, order_items, email_logs                         |
+-------------------------------------------------------------+
                            |
                            |
                            v
+-------------------------------------------------------------+
|                      Email Service                          |
|                   SMTP / Transactional Email API            |
+-------------------------------------------------------------+
________________________________________
3. Major Components & Responsibilities
________________________________________
3.1 Frontend (React / Next.js)
Responsibilities:
●	Display menu categories

●	Display product listings (name, image, price, quantity)

●	Manage cart (add/update/remove items)

●	Show cart summary with price breakdown

●	Collect user details for delivery orders

●	Trigger order placement via API

●	Display order confirmation page

●	(Stretch) Support login and show order history

________________________________________
3.2 Backend REST API
Runs business logic and exposes endpoints.
Responsibilities:
●	Manage categories, products, inventory

●	Maintain user carts and items

●	Validate cart and order requests

●	Create orders and order items

●	Update inventory after order placement

●	Secure API endpoints using API Key

●	 Send order confirmation emails

●	 Return authenticated user order history


3.4 Email Service 
Triggered when an order is placed.
Responsibilities:
●	Construct confirmation email with order number

●	Send via SMTP or transactional email API

●	Log email status in email_logs table

________________________________________
3.5 CI/CD Pipeline (GitHub Actions)
Responsibilities:
●	Build frontend and backend

●	Run automated tests

●	Deploy to cloud environment

●	Ensure consistent and repeatable deployments














4. User Flow Diagrams

4.1 End-to-End User Flow
User Opens Website
        |
        v
Menu Page Loads
        |
        v
User Selects Category
        |
        v
Product Page Loads (List of Products)
        |
        v
User Adds Items to Cart
        |
        v
Cart Page Displays Items & Prices
        |
        v
User Clicks "Place Order"
        |
        v
Enter Delivery Details
        |
        v
Backend Validates Cart & Inventory
        |
        v
Order Confirmed and Stored in DB
        |
        v
(Stretch) Email Confirmation Sent
        |
        v
Show Order Confirmation Page
________________________________________
4.2 Backend API Flow
Client Request
      |
      |---> Security Check (x-api-key)
      |
      v
API Route Handler
      |
      v
Business Logic Layer
      |
      |---> Inventory Check
      |
      |---> Cart / Order Creation
      |
      |--->  Send Email
      |
      v
Database Write
      |
      v
Return JSON Response
________________________________________
5. Data Flow Design
________________________________________
5.1 Menu Browsing
1.	Frontend calls GET /api/categories

2.	Backend returns category list

3.	User selects category

4.	Frontend calls GET /api/products?category_id=

5.	Backend returns list of products with price and inventory

________________________________________
5.2 Cart Management
Add Item to Cart
1.	Frontend sends request:

POST /api/cart/add
2.	
Backend identifies cart:

○	User cart (if authenticated)

○	Session-based cart (if not)

3.	Backend updates:

○	carts table

○	cart_items table

4.	Backend returns updated cart

________________________________________
5.3 Order Placement
1.	Frontend sends:

POST /api/order/place
2.	
Backend steps:

○	Validate cart

○	Check inventory for all items

○	Reduce inventory in products

○	Create order and order_items rows

○	Generate order number

○	(Stretch) Send email

3.	Backend responds with:
{order_id, status, total_amount}

API Endpoints

7.1 Menu APIs
GET /api/categories
GET /api/products?category_id=
7.2 Cart APIs
POST /api/cart/add
POST /api/cart/update
GET /api/cart
7.3 Order APIs
POST /api/order/place
GET /api/order/:id
GET /api/order/history   (stretch)
7.4 Security
Every request must include:
x-api-key: <api_key>


________________________________________






