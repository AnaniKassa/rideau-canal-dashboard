## Rideau Canal Monitoring Dashboard

### 1. Overview

The Rideau Canal Monitoring Dashboard is a cloud-native web application that visualizes real-time and historical environmental data collected from IoT sensors deployed along the Rideau Canal.
It displays ice thickness, surface temperature, snow accumulation, safety status, and trends for multiple locations.

- Dashboard Features

    - Real-time sensor data updates (from Cosmos DB)

    - Ice thickness and temperature trend charts

    - Per-location safety status

    - Multi-location comparison widgets

    - Auto-refreshing data feed

    - Responsive UI for web and mobile

- Technologies Used

    - Node.js + Express (backend API)

    - Cosmos DB (aggregated data storage)

    - Azure App Service (hosting)

    - HTML / CSS / JavaScript (vanilla)

    - Fetch API for real-time requests

    - Chart.js for historical trends

### 2. Prerequisites

Before installing or deploying the dashboard, ensure you have:

Node.js 18+

Azure Cosmos DB (Core API / SQL)

Azure App Service (Linux or Windows)

Stream Analytics job writing data into Cosmos DB

A valid .env configuration file (see below)

### 3. Installation

- Clone the repository:

git clone https://github.com/AnaniKassa/rideau-canal-dashboard

cd rideau-canal-dashboard


- Install dependencies:

npm install


- Run locally:

npm start


- The dashboard will be available at:

http://localhost:3000

4. Configuration

- Create a .env file in the project root:
```
COSMOS_ENDPOINT="https://your-account.documents.azure.com:443/"
COSMOS_KEY="your-primary-key"
COSMOS_DATABASE="RideauCanalDB"
COSMOS_CONTAINER="SensorAggregations"
PORT=3000
```

Modify .env.example as needed.

### 5. API Endpoints

The backend server exposes several API endpoints.

GET /api/latest

Returns the most recent metrics for all monitored locations.

Example Response:
```
{
  "success": true,
  "timestamp": "2025-12-08T15:20:12Z",
  "data": [
    {
      "deviceId": "dowslake",
      "location": "dowslake",
      "avg_ice_thickness": 21.3,
      "event_time": "2025-12-08T15:15:00Z"
    }
  ]
}
```

GET /api/history/:location?limit=12

Returns historical 5-minute window data.

Example:

/api/history/dowslake?limit=12

GET /api/status

Returns combined safety status for all locations.

Example Response:
```
{
  "success": true,
  "overallStatus": "Caution",
  "locations": [
    {
      "location": "dowslake",
      "safety_status": "Unsafe"
    }
  ]
}
```
GET /api/all

Returns all records in the Cosmos DB container.
Used for debugging.

### 6. Deployment to Azure App Service

- Step 1 — Prepare the App

Ensure these files exist before deploying:

server.js

public/ directory

.env created locally (values will later be set in Azure)

- Step 2 — Create App Service

In Azure Portal:

Create Web App

Runtime stack: Node 18 LTS

OS: Linux

Deploy from Local Git, GitHub, or VS Code

- Step 3 — Configure Environment Variables

Go to:

App Service → Configuration → Application Settings

Add:

COSMOS_ENDPOINT

COSMOS_KEY

COSMOS_DATABASE

COSMOS_CONTAINER

PORT=3000

Save & restart the app.

- Step 4 — Deploy

Using VS Code:

Azure: Deploy to Web App


Or using GitHub Actions (if enabled).

- Step 5 — Verify

Visit:

https://your-webapp-name.azurewebsites.net


Open browser console to ensure no API or CORS errors.

### 7. Dashboard Features

Real-Time Metrics

- The dashboard updates automatically every 5 seconds:

Ice thickness

Surface temperature

Snow accumulation

Safety alerts

Charts & Visualizations

- Powered by Chart.js:

Ice thickness trend

Temperature trend

Hourly rolling window

Safety Status Indicators

- Displays:

Green – Safe

Yellow – Caution

Red – Unsafe

Based on Stream Analytics computed rules.

### 8. Troubleshooting

Dashboard shows “Loading…”

Check /api/latest in the browser → might be empty

Ensure Stream Analytics is writing to Cosmos DB

Ensure Cosmos DB connection string is correct

Charts show no data

Verify /api/history/{location} returns records

Ensure event_time is in ISO 8601 format

Azure error “503 Service Unavailable”

Restart App Service

Ensure PORT variable is set

Ensure server.js calls app.listen(process.env.PORT)

CORS issues

Ensure app.use(cors()) is enabled

Use the full domain, e.g., https://yourapp.azurewebsites.net