# RuralLink

### AI-Powered Rural Last-Mile Delivery Platform

RuralLink is a full-stack rural logistics platform designed to improve the coordination of deliveries in remote and underserved areas.

It combines AI-assisted order processing, delivery priority prediction, route optimization, GPS tracking, and an offline-first Driver PWA to support reliable last-mile delivery even when internet connectivity is limited.

> **My Role:** Driver PWA & Offline Storage

---

## Overview

Rural delivery networks can face challenges such as:

* Poor or changing road conditions
* Limited vehicle availability
* Small and scattered shipments
* Perishable goods
* Urgent medicine and essential-goods deliveries
* Unreliable internet connectivity
* Inefficient manual allocation

RuralLink provides a centralized coordination layer for customers, dispatchers and drivers.

### Core Workflow

```text
Customer / Producer
        ↓
Order Registration
        ↓
AI-Assisted Parsing
        ↓
Priority / ETA / Delay Prediction
        ↓
Route Optimization
        ↓
Driver Assignment
        ↓
Driver PWA
        ↓
Delivery + GPS Updates
        ↓
Offline Storage & Synchronization
```

---

## My Contribution

### Driver PWA & Offline Storage

I worked primarily on the driver-facing side of RuralLink, focusing on the Driver PWA and offline functionality.

My contributions include:

* Driver login and dashboard
* Assigned delivery management
* Delivery status workflow
* Delivery details interface
* Route and map visualization
* GPS/location tracking
* Offline delivery-status storage
* Offline GPS/event storage
* Synchronization after connectivity is restored
* PWA installation and frontend integration
* Backend API integration for driver workflows

The goal was to allow drivers to continue important delivery operations even during temporary network loss.

---

## Key Features

### Customer Order Portal

* Create delivery orders
* Submit natural-language order messages
* View order information
* Receive delivery priority and ETA information
* Track delivery progress

### AI-Assisted Order Parsing

* Uses Google Gemini when configured
* Extracts structured information from natural-language orders
* Includes a keyword-based fallback parser
* Continues basic parsing when Gemini is unavailable

### Delivery Prediction

The backend ML service supports:

* Delivery priority prediction
* ETA estimation
* Delay-risk prediction

The system includes fallback behavior when optional prediction services are unavailable.

### Route Optimization

* Google OR-Tools based route optimization
* Delivery sequence optimization
* Driver and delivery assignment support
* Optional OpenRouteService road routing
* Straight-line fallback routing for demonstration purposes

### Driver Progressive Web App

* Driver login
* Assigned delivery dashboard
* Delivery details
* Delivery status updates
* GPS tracking
* Route visualization
* Offline delivery-status storage
* Offline GPS/event storage
* Automatic synchronization after reconnection
* PWA installation support

### Backend API

* FastAPI REST API
* Swagger API documentation
* Health-check endpoint
* Order management
* Driver management
* Route planning
* Prediction services
* Offline synchronization

---

## Offline-First Driver Workflow

The Driver PWA is designed to continue essential operations during temporary network loss.

### Before Going Offline

The application can cache:

* Assigned deliveries
* Delivery information
* Route information
* Required driver data

### During Offline Operation

The driver can:

* View assigned deliveries
* View cached route information
* Update delivery status
* Store GPS locations
* Continue the delivery workflow

Events are stored locally using browser storage.

### After Reconnection

```text
Network Restored
       ↓
Detect Connectivity
       ↓
Read Pending Local Events
       ↓
Synchronize with Backend
       ↓
Update Delivery Status
       ↓
Synchronize GPS Data
       ↓
Refresh Application State
```

The current implementation is prototype-level. A production system would require stronger conflict resolution, encrypted local storage and additional device security.

---

## Technology Stack

### Frontend

* React
* Vite
* Tailwind CSS
* React Router
* Axios
* Leaflet
* React Leaflet
* Vite PWA

### Backend

* Python 3.12
* FastAPI
* Uvicorn
* SQLite
* Scikit-learn

### AI & Optimization

* Google Gemini API
* Google OR-Tools

### Routing

* OpenRouteService API
* Leaflet / OpenStreetMap

### Deployment

* Docker
* Render

---

## Project Structure

```text
RuralLink/
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── database.py
│   │   ├── gemini_service.py
│   │   ├── ml_service.py
│   │   ├── route_optimizer.py
│   │   ├── routing_service.py
│   │   └── schemas.py
│   │
│   ├── data/
│   │   └── generate_synthetic_data.py
│   │
│   ├── models/
│   │   ├── artifacts.joblib
│   │   └── train_models.py
│   │
│   ├── tests/
│   ├── .env.example
│   └── requirements.txt
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── utils/
│   │   └── App.jsx
│   │
│   ├── package.json
│   └── vite.config.js
│
├── data/
├── Dockerfile
├── render.yaml
├── start-RuralLink.ps1
├── .dockerignore
├── .gitignore
└── README.md
```

---

## Requirements

Install the following before running the project locally:

* Python 3.12
* Node.js 22+
* npm
* Git
* Docker Desktop (optional)

---

## Local Setup

### 1. Clone the Repository

```bash
git clone https://github.com/AmanSharma012/Rural-Link.git
cd Rural-Link
```

### 2. Build the Frontend

```bash
cd frontend
npm ci
npm run build
cd ..
```

The frontend build is required because the FastAPI backend serves the built frontend application.

### 3. Create a Python Virtual Environment

#### Windows PowerShell

```powershell
cd backend
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

#### Linux / macOS

```bash
cd backend
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure Environment Variables

From the `backend` directory:

#### Windows PowerShell

```powershell
Copy-Item .env.example .env
```

#### Linux / macOS

```bash
cp .env.example .env
```

Edit `backend/.env` if you want to enable optional external services.

**Never commit your actual `.env` file or API keys to GitHub.**

### 5. Start the Backend

From the `backend` directory:

```bash
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

---

## Local Application URLs

When the application is running:

| Service           | URL                             |
| ----------------- | ------------------------------- |
| Customer Portal   | `http://127.0.0.1:8000/`        |
| Driver PWA        | `http://127.0.0.1:8000/driver/` |
| API Documentation | `http://127.0.0.1:8000/docs`    |
| Health Check      | `http://127.0.0.1:8000/health`  |

---

## Environment Variables

The safe template is available at:

```text
backend/.env.example
```

Example:

```env
GEMINI_API_KEY=
GEMINI_MODEL_NAME=gemini-2.5-flash

ORS_API_KEY=
ORS_BASE_URL=https://api.openrouteservice.org
```

### Optional Services

If `GEMINI_API_KEY` is not configured:

* The application can use the keyword-based order parser.

If `ORS_API_KEY` is not configured:

* The application can use the configured fallback routing method.

Environment variable names and supported values should follow the current `.env.example` and application configuration.

---

## API Endpoints

| Method | Endpoint                                      | Description                          |
| ------ | --------------------------------------------- | ------------------------------------ |
| GET    | `/health`                                     | Application health status            |
| POST   | `/parse-message`                              | Parse a natural-language order       |
| POST   | `/predict`                                    | Predict priority, ETA and delay risk |
| POST   | `/optimize-route`                             | Optimize a delivery route            |
| POST   | `/api/orders`                                 | Create an order                      |
| GET    | `/api/orders`                                 | List orders                          |
| POST   | `/api/orders/plan`                            | Plan saved orders                    |
| POST   | `/api/driver/login`                           | Driver login                         |
| GET    | `/api/driver/{driver_id}/deliveries`          | Get driver deliveries                |
| GET    | `/api/driver/{driver_id}/route`               | Get driver route                     |
| POST   | `/api/driver/{driver_id}/location`            | Update driver GPS                    |
| PATCH  | `/api/driver/deliveries/{delivery_id}/status` | Update delivery status               |

Interactive documentation:

```text
http://127.0.0.1:8000/docs
```

---

## AI & Machine Learning

### Google Gemini

Gemini is used for natural-language order parsing.

Example:

```text
"I need urgent medicine delivered to village X"
```

The system can extract relevant delivery information and convert it into structured data.

A keyword-based fallback parser is available when Gemini is not configured.

### Machine Learning

The backend ML service supports:

* Delivery priority prediction
* ETA prediction
* Delay-risk prediction

The trained model artifact is stored in:

```text
backend/models/artifacts.joblib
```

### Route Optimization

Google OR-Tools is used for delivery route optimization.

The routing process can consider:

* Delivery locations
* Delivery sequence
* Driver availability
* Estimated travel time
* Route distance
* Delivery priority
* Delivery constraints

When configured, OpenRouteService can provide road-based routing information.

---

## Driver PWA

The Driver PWA is one of the main components of RuralLink.

### Driver Workflow

```text
Driver Login
     ↓
Dashboard
     ↓
Assigned Deliveries
     ↓
Delivery Details
     ↓
Route / GPS
     ↓
Update Delivery Status
     ↓
Offline Storage if Network is Unavailable
     ↓
Automatic Synchronization
```

The PWA allows drivers to continue essential delivery operations during temporary connectivity problems.

---

## Docker

### Build

From the repository root:

```bash
docker build -t rurallink .
```

### Run

Windows PowerShell:

```powershell
docker run --rm --name rurallink -p 8000:8000 --env-file .\backend\.env rurallink
```

Linux / macOS:

```bash
docker run --rm \
  --name rurallink \
  -p 8000:8000 \
  --env-file ./backend/.env \
  rurallink
```

---

## Render Deployment

The repository includes:

```text
Dockerfile
render.yaml
```

The project can be deployed using Render's Blueprint configuration.

General deployment flow:

```text
GitHub Repository
       ↓
Render Blueprint
       ↓
Configure Environment Variables
       ↓
Deploy
       ↓
Verify /health
       ↓
Verify Customer Portal
       ↓
Verify Driver PWA
```

For production deployment, persistent storage should be used instead of relying on ephemeral SQLite storage.

---

## Testing

Backend tests can be run using:

```bash
cd backend
pytest
```

Useful manual checks include:

```text
/health
/docs
/
/driver/
```

Important scenarios include:

* Creating a customer order
* Natural-language order parsing
* Keyword parser fallback
* Delivery prediction
* Route optimization
* Driver login
* Viewing assigned deliveries
* Updating delivery status
* Offline delivery updates
* Offline GPS storage
* Synchronization after reconnection

---

## Current Project Status

| Module                      | Status               |
| --------------------------- | -------------------- |
| Customer Order Portal       | Functional Prototype |
| Driver PWA                  | Complete             |
| FastAPI Backend             | Functional           |
| Gemini Order Parsing        | Optional             |
| Keyword Parser Fallback     | Functional           |
| ML Prediction Services      | Functional           |
| OR-Tools Route Optimization | Functional           |
| Offline Delivery Updates    | Complete             |
| Offline GPS Storage         | Complete             |
| Automatic Synchronization   | Functional           |
| Docker Configuration        | Available            |
| Render Configuration        | Available            |

The project is an academic/hackathon prototype rather than a production logistics platform.

---

## Demo Workflow

A typical demonstration can follow this flow:

1. Open the customer portal.
2. Create a delivery order.
3. Submit a natural-language order.
4. Show parsed order information.
5. Show priority, ETA and delay-risk prediction.
6. Plan the saved orders.
7. Display the optimized route.
8. Open the Driver PWA.
9. Log in as a driver.
10. View assigned deliveries.
11. Simulate network loss.
12. Update a delivery status.
13. Show the locally stored update.
14. Restore connectivity.
15. Demonstrate synchronization.
16. Show the route and GPS information.
17. Open Swagger UI.

---

## Limitations

* SQLite is suitable for the prototype but should be replaced with persistent production storage.
* Gemini and OpenRouteService require external API credentials.
* Straight-line routing is only a fallback and does not represent actual road travel.
* ML predictions depend on the quality of the training data.
* GPS behavior depends on browser and device permissions.
* Offline synchronization is prototype-level.
* Production deployment would require stronger authentication and authorization.
* Production systems handling medicines would require additional compliance and monitoring.
* Multi-fleet coordination is not fully implemented.

---

## Future Scope

* PostgreSQL and PostGIS
* Multi-vehicle and multi-fleet coordination
* Community pickup-point management
* Fairness-aware allocation
* Perishability-aware scheduling
* Road-condition reporting
* Dynamic rerouting
* Vehicle capacity constraints
* Cold-chain monitoring
* SMS and IVR support
* Regional-language and voice interfaces
* Improved ETA and demand prediction
* Government, NGO and FPO integrations
* Advanced audit and fairness analytics
* Scalable cloud deployment

---

## Team

| Member             | Contribution                 |
| ------------------ | ---------------------------- |
| Avinab Amlan Nayak | Backend                      |
| Dibyani Tripathy   | React Frontend               |
| Aryan Kumar Sahu   | AI/ML & OR-Tools             |
| Aman Kumar Sharma  | Driver PWA & Offline Storage |
| Yamini Mishra      | Presentation & Video         |
| Sanchita Raju      | Testing & Documentation      |

---

## Third-Party Technologies

RuralLink uses open-source libraries and third-party services including:

* React
* FastAPI
* Scikit-learn
* Google OR-Tools
* Google Gemini API
* OpenRouteService
* Leaflet
* OpenStreetMap

Appropriate licenses and attribution requirements should be retained for third-party dependencies.

If OpenStreetMap data is displayed:

```text
Map data © OpenStreetMap contributors.
```

---

## Acknowledgement

RuralLink was developed as an academic prototype for **SOA Ideathon 2026**.

The project demonstrates how AI-assisted order processing, machine-learning predictions, route optimization and offline-first driver workflows can be combined to improve rural last-mile delivery coordination.

---

## Repository

**GitHub:**
https://github.com/AmanSharma012/Rural-Link
