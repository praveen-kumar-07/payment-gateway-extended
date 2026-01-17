Payment Gateway Project

OVERVIEW

This project is a full-stack Payment Gateway simulation built as a capstone assignment.
It replicates the core workflow of real-world platforms like Razorpay or Stripe, allowing merchants to create payment orders and customers to complete payments through a hosted checkout page.

The system supports UPI and Card payments (with Luhn algorithm validation), simulates real banking delays, and provides a merchant dashboard to view transactions.

TECH STACK (PERN)

Database: PostgreSQL 15 (Dockerized)
Backend: Node.js + Express
Frontend (Dashboard): React + Vite + TailwindCSS
Frontend (Checkout): React
Containerization: Docker & Docker Compose

PREREQUISITES

Docker Desktop installed and running

No local Node.js or npm installation required

QUICK START (FOR EVALUATORS)

The entire application is fully containerized and can be started with a single command.

Clone the repository

git clone https://github.com/praveen-kumar-07/payment-gateway.git

cd payment-gateway

Start the application

docker-compose up -d

Please wait 30–60 seconds after startup for the database to initialize and services to become ready.

Verify running containers

docker ps

You should see the following containers:

gateway_api

pg_gateway

gateway_dashboard

gateway_checkout

SERVICE URLS

Backend API:
http://localhost:8000

Merchant Dashboard:
http://localhost:3000

Checkout Page:
http://localhost:3001

TESTING GUIDE

Verify System Health

GET http://localhost:8000/health

Expected response:
{
"status": "healthy",
"database": "connected"
}

Create a Test Order (Merchant API)

Since there is no merchant shop UI, orders must be created via API.

PowerShell (Windows):

Invoke-RestMethod -Uri "http://localhost:8000/api/v1/orders
" -Method Post
-Headers @{
"X-Api-Key"="key_test_abc123";
"X-Api-Secret"="secret_test_xyz789";
"Content-Type"="application/json"
} `
-Body '{
"amount": 50000,
"currency": "INR",
"receipt": "receipt_1",
"notes": { "desc": "Test Order" }
}'

cURL (Mac / Linux / Git Bash):

curl -X POST http://localhost:8000/api/v1/orders

-H "Content-Type: application/json"
-H "X-Api-Key: key_test_abc123"
-H "X-Api-Secret: secret_test_xyz789"
-d '{
"amount": 50000,
"currency": "INR",
"receipt": "receipt_1",
"notes": { "desc": "Test Order" }
}'

IMPORTANT: Copy the order id from the response
Example: order_xxxxxxxxxxxxxxxx

Complete Payment (Checkout Page)

Open the checkout page:

http://localhost:3001/checkout?order_id=YOUR_ORDER_ID

UPI Test:

Enter a valid VPA: test@okaxis

Click Pay

Card Test:

Card Number: 4111 1111 1111 1111

Expiry: 12/30 (any future date)

CVV: any 3 digits

Name: any value

Click Pay

The system simulates a 5–10 second bank processing delay.
The result will automatically update to Success or Failure.

Verify in Merchant Dashboard

Open:
http://localhost:3000

Login credentials:
Email: test@example.com

Password: any value

Navigate to Transactions and verify the payment status.

TEST MERCHANT (AUTO-SEEDED)

Email: test@example.com

API Key: key_test_abc123
API Secret: secret_test_xyz789

KEY FEATURES IMPLEMENTED

Merchant authentication using API Key & Secret

Order creation with strict ID formats

UPI validation using regex

Card validation using Luhn algorithm

Card network detection (Visa, Mastercard, Amex, RuPay)

Mock banking delay simulation

Deterministic test mode for evaluation

Hosted checkout page with polling-based status updates

Secure handling of sensitive card data

PROJECT STRUCTURE

backend/ Express API and validation logic
checkout-page/ React checkout application
frontend/ React merchant dashboard
database/ SQL schema and seed scripts
docker-compose.yml
.env.example
README.md

TEST MODE (FOR EVALUATION)

TEST_MODE=true
TEST_PAYMENT_SUCCESS=true
TEST_PROCESSING_DELAY=1000

Enables deterministic payment outcomes for automated testing.

NOTES

All payments are simulated

No real banking integrations are used

This project is for learning and evaluation purposes only

STATUS

Backend complete
Checkout flow fully working
UPI and Card payments functional
Dockerized and evaluation-ready
