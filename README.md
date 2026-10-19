# RentWheels 🚗 | Modern Vehicle Rental & Fleet Management System

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0%2B-000000.svg?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![MongoDB Atlas](https://img.shields.io/badge/MongoDB-Atlas%20%7C%20PyMongo-47A248.svg?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Chart.js](https://img.shields.io/badge/Frontend-Vanilla%20JS%20%7C%20Chart.js-FF6384.svg?logo=chartdotjs&logoColor=white)](https://www.chartjs.org/)
[![Gunicorn](https://img.shields.io/badge/WSGI-Gunicorn-499848.svg?logo=gunicorn&logoColor=white)](https://gunicorn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**RentWheels** is an end-to-end full-stack vehicle rental and fleet management platform engineered for car rental agencies. Built with **Flask** and **MongoDB Atlas**, it automates vehicle reservations, hourly and daily billing, fleet health tracking, customer verification, security deposits, and real-time operational analytics.

---

## 📌 Project Overview

Managing a modern vehicle rental fleet requires precise rate calculation, dynamic availability tracking, and automated billing workflows. **RentWheels** solves these operational challenges by providing:

- **Hourly & Daily Fleet Booking**: Flexible rental schedules with automatic duration and pricing calculation.
- **Dynamic Fleet Availability**: Status transitions (`Available` ↔ `Rented` ↔ `Maintenance`) to prevent double-booking.
- **Customer Lifecycle Tracking**: Driver's license validation and auto-incremented customer IDs.
- **Intelligent Return & Billing Engine**: Automated late fee computation, damage assessment adjustments, and deposit reconciliation.
- **Executive Operations Dashboard**: Real-time revenue charts and fleet utilization breakdown.

> **Source Repository**: Core codebase based on [Yogesh33s/CarRental](https://github.com/Yogesh33s/CarRental).

---

## ✨ Key Features

### 🚗 Fleet Management
- **Complete Vehicle Profiles**: Track Make, Model, Year, VIN, License Plate, Vehicle Category, and Hourly Rental Rates.
- **Supported Categories**: `Sedan`, `SUV`, `Hatchback`, `Convertible`, `Truck`, and `Minivan`.
- **Condition & Health Monitoring**: Maintain condition grades (`Good`, `Scratched`, `Damaged`).
- **Maintenance Lifecycle**: Mark vehicles under repair and seamlessly return them to the available fleet upon service completion.

### 👥 Customer Management
- **Sequential Customer IDs**: Automatically generates human-readable numeric IDs (`1001`, `1002`, etc.) via atomic MongoDB sequence counters.
- **Verification Records**: Stores verified driver license numbers, contact emails, and phone numbers.
- **Unique Constraints**: Prevents duplicate customer registration through database-level unique indexing on emails and license numbers.

### ⏱️ Hourly Booking & Rental Lifecycle
- **Smart Scheduling**: Set rental start and scheduled return dates/times.
- **Automatic Pricing**: Calculates total duration in hours and applies hourly billing rates.
- **Security Deposit Handling**: Records upfront refundable deposits.
- **Booking Code Generation**: Produces unique alphanumeric booking codes (`BK-XXXXXX`) for ticket lookups.

### 🔄 Vehicle Returns & Inspection Engine
- **Late Fee Calculation**: Automatically assesses penalty fees for overdue returns (`DAILY_LATE_FEE = ₹500.00/day` prorated).
- **Condition Re-assessment**: Inspect vehicle on return; record new damage charges if scratches or damages are found.
- **Final Settlement**: Reconciles rental charges, damage fees, and late penalties against the initial security deposit to calculate net payable or refund.

### 📊 Real-Time Analytics Dashboard
- **KPI Metrics Cards**: Instant count of Total Fleet, Available Cars, Active Rentals, and Total Revenue.
- **Interactive Visualizations**:
  - **Revenue Over Time**: Visualized through Chart.js line/bar graphs.
  - **Category Breakdown**: Doughnut chart illustrating fleet distribution across vehicle types.
- **Recent Activity Table**: View latest rentals with immediate action shortcuts.

---

## 🛠️ Architecture & Data Model

### Technology Stack

| Layer | Technologies |
| :--- | :--- |
| **Backend** | Python 3.9+, Flask, PyMongo, python-dotenv, certifi, dnspython |
| **Frontend** | HTML5, CSS3 (Modern Responsive UI), Vanilla JavaScript (ES6+), Chart.js |
| **Database** | MongoDB Atlas (NoSQL Document Store with unique & compound indexes) |
| **Server / WSGI** | Gunicorn |

### MongoDB Collections & Schema

1. **`cars`**:
   - `make` (string), `model` (string), `year` (int), `vin` (unique string)
   - `license_plate` (unique string), `category` (string), `hourly_rate` (float)
   - `status` (`Available` | `Rented` | `Maintenance`), `condition` (`Good` | `Scratched` | `Damaged`)
   - `created_at` (datetime)

2. **`customers`**:
   - `customer_id` (unique int sequence), `name` (string), `email` (unique string)
   - `phone` (string), `license_number` (unique string), `created_at` (datetime)

3. **`rentals`**:
   - `booking_code` (unique string), `car_id` (ObjectId string), `customer_id` (int/string)
   - `start_time` (ISO datetime), `end_time` (ISO datetime), `hourly_rate` (float)
   - `deposit` (float), `status` (`Active` | `Completed` | `Cancelled`)
   - `return_details` (actual return time, late fees, damage charges, final total)

4. **`counters`**:
   - Atomic sequence counters for generating sequential numeric IDs (`customer_id`).

---

## 🔌 REST API Reference

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/` | Serve main Single-Page Application (SPA) dashboard |
| `GET` | `/api/health` | Health check endpoint verifying MongoDB connectivity |
| `GET` | `/api/dashboard` | Fetch aggregate KPIs, revenue totals, and chart data |
| `GET` | `/api/cars` | Retrieve fleet list (supports filtering by category/status) |
| `POST` | `/api/cars` | Register a new vehicle in the fleet |
| `DELETE` | `/api/cars/<car_id>` | Remove a vehicle from the fleet |
| `POST` | `/api/cars/<car_id>/maintenance-complete` | Complete vehicle maintenance and mark as available |
| `GET` | `/api/customers` | List all registered customers |
| `POST` | `/api/customers` | Register a new customer |
| `DELETE` | `/api/customers/<customer_id>` | Remove a customer record |
| `GET` | `/api/rentals` | Retrieve active and completed rental bookings |
| `POST` | `/api/rentals` | Create a new vehicle booking |
| `POST` | `/api/rentals/<rental_id>/return` | Process vehicle return, calculate late/damage fees, and close rental |

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your machine:
- **Python 3.9+** ([Download Python](https://www.python.org/downloads/))
- **MongoDB Atlas Account** or a local **MongoDB Community Server** ([MongoDB Atlas](https://www.mongodb.com/cloud/atlas))
- **Git**

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone git@github.com:bhaveshk25/CarRental.git
   cd CarRental
   ```

2. **Create and Activate a Virtual Environment**
   ```bash
   # macOS / Linux
   python3 -m venv venv
   source venv/bin/activate

   # Windows
   python -m venv venv
   venv\Scripts\activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure Environment Variables**
   Create a `.env` file in the root directory:
   ```env
   MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/?retryWrites=true&w=majority
   ```

5. **Test Database Connectivity**
   ```bash
   python test_db.py
   ```
   *Expected output: `SUCCESS: Connected to MongoDB Atlas!`*

6. **Run the Application**
   ```bash
   python app.py
   ```
   The application will start at `http://127.0.0.1:5000/`.

---

## 🌐 Production Deployment

To serve the application in a production environment using **Gunicorn**:

```bash
gunicorn app:app --bind 0.0.0.0:5000 --workers 3
```

---

## 📁 Project Structure

```text
CarRental/
├── app.py                # Main Flask backend with REST API routes & MongoDB business logic
├── requirements.txt      # Python dependencies
├── test_db.py            # MongoDB connection verification utility
├── .gitignore            # Git ignore rules for venv, .env, and cache files
├── README.md             # Project documentation
├── static/
│   └── app.js            # Frontend application logic, API calls, and Chart.js integration
└── templates/
    └── index.html        # Single-page application HTML & CSS styling
```

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
