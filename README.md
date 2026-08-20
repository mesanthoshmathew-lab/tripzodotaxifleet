# TripZodo Fleetzx

**TripZodo Travel Ventures Pvt Ltd** — Production-style taxi fleet management web application.

A complete fleet management dashboard for vehicles, drivers, trips, live GPS tracking, maintenance, and reports. Built with vanilla HTML5, CSS3, and JavaScript ES6+ with a swappable data layer.

## Quick Start

### Option 1: Open directly

Open `index.html` in a modern browser (Chrome, Firefox, Safari, Edge).

### Option 2: Local static server

```bash
python3 -m http.server 8080
# Visit http://localhost:8080
```

### Option 3: Express server (frontend + API stubs)

```bash
npm install
npm start
# Visit http://localhost:3000
```

## Demo Login

| Field    | Value     |
|----------|-----------|
| Username | `admin`   |
| Password | `admin123`|

## Project Structure

```
tripzodo-fleet/
├── index.html          # SPA shell (login + app layout)
├── css/
│   └── style.css       # Design system & responsive layout
├── js/
│   ├── utils.js        # UUID, dates, CSV export, status badges
│   ├── storage.js      # localStorage adapter + demo seed data
│   ├── api.js          # Data layer (local | remote adapter)
│   ├── ui.js           # Toasts, modals, validation, loading states
│   ├── auth.js         # Login/logout/session
│   ├── app.js          # Router, dashboard, charts, mini-map
│   ├── vehicles.js     # Vehicle CRUD + compliance badges
│   ├── drivers.js      # Driver CRUD + vehicle assignment
│   ├── trips.js        # Trip CRUD + lifecycle actions
│   ├── maintenance.js  # Maintenance CRUD + service alerts
│   ├── gps.js          # Live GPS map + realtime abstraction
│   ├── reports.js      # Analytics + CSV/JSON export
│   └── settings.js     # Company, fleet, API configuration
├── assets/
│   └── logo.svg        # TripZodo Fleet logo
├── backend/            # Express API stubs (for future wiring)
├── database/
│   └── schema.sql      # MySQL schema (UUID-based)
└── README.md
```

## Features

- **Dashboard** — 8 stat cards, 4 Chart.js charts, live fleet map
- **Vehicles** — Full CRUD, search/filter/sort, compliance expiry warnings
- **Drivers** — Full CRUD, vehicle assign/unassign, license expiry alerts
- **Trips** — Full CRUD, start/complete/cancel with vehicle status side effects
- **Live GPS** — Leaflet + OpenStreetMap, admin geolocation demo, status-colored markers
- **Maintenance** — Service records, upcoming alerts, cost tracking
- **Reports** — Daily/weekly/monthly metrics, driver performance, CSV/JSON export
- **Settings** — Company info, map center, GPS poll interval, API config placeholders

## How localStorage Works

All data is stored in the browser under these keys:

| Key | Content |
|-----|---------|
| `tzf_users` | User accounts |
| `tzf_vehicles` | Fleet vehicles |
| `tzf_drivers` | Driver roster |
| `tzf_trips` | Trip records |
| `tzf_maintenance` | Maintenance history |
| `tzf_settings` | App settings |
| `tzf_session` | Current login session |
| `tzf_seeded_v2` | One-time demo seed flag |

Demo data (10 vehicles around Bengaluru, 5 drivers, 5 trips) is seeded automatically on first load.

## Connect to REST API

1. Implement the backend routes in `backend/routes/`
2. Set `API._adapter = 'remote'` in `js/api.js`
3. Configure API Base URL in Settings page

### API Endpoints (prepared)

```
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/me

GET    /api/vehicles
POST   /api/vehicles
GET    /api/vehicles/:id
PUT    /api/vehicles/:id
DELETE /api/vehicles/:id
POST   /api/vehicles/:id/location
GET    /api/vehicles/live

GET    /api/drivers
POST   /api/drivers
GET    /api/drivers/:id
PUT    /api/drivers/:id
DELETE /api/drivers/:id

GET    /api/trips
POST   /api/trips
GET    /api/trips/:id
PUT    /api/trips/:id
DELETE /api/trips/:id

GET    /api/maintenance
POST   /api/maintenance
PUT    /api/maintenance/:id
DELETE /api/maintenance/:id
```

## Connect to MySQL

1. Run the schema: `mysql -u root -p < database/schema.sql`
2. Add `mysql2` to backend dependencies
3. Implement route handlers using the schema in `database/schema.sql`
4. Use bcrypt for password hashing (never store plain text)
5. Switch `API._adapter` to `'remote'`

## Connect to Firebase

Replace `Storage` methods in `js/storage.js` with Firestore calls, or implement a `FirebaseAdapter` class and point `api.js` to it. Keep the same function signatures (`getVehicles`, `createVehicle`, etc.) so UI modules require no changes.

## Add Real Driver GPS

The frontend is prepared for driver-phone tracking:

```javascript
// Driver mobile app sends:
POST /api/vehicles/:vehicleId/location
{
  "latitude": 12.9716,
  "longitude": 77.5946,
  "accuracy": 10,
  "speed": 45,
  "heading": 180,
  "timestamp": "2026-08-18T10:30:00Z"
}

// Admin dashboard polls:
GET /api/vehicles/live
```

For real-time updates, replace polling in `gps.js` with WebSocket:

```javascript
connectRealtime();      // Start WebSocket connection
disconnectRealtime();   // Stop connection
subscribeVehicleLocation(callback);  // Receive location updates
```

## Deploy

### Vercel (static)

```bash
vercel
```

The `vercel.json` config serves the root-level static files.

### Any static host

Upload `index.html`, `css/`, `js/`, and `assets/` to Netlify, GitHub Pages, S3, etc.

### With backend

Deploy the Express server to Railway, Render, or a VPS. Set environment variables for database credentials server-side only.

## Security Notes

- Demo passwords are stored in localStorage for development only
- Never put MySQL credentials, Firebase admin keys, or secret API keys in frontend code
- Use environment variables on the server for all secrets
- In production, implement proper JWT/session auth with bcrypt password hashing

## Tech Stack

- HTML5, CSS3, Vanilla JavaScript ES6+
- [Leaflet.js](https://leafletjs.com/) + OpenStreetMap tiles
- [Chart.js](https://www.chartjs.org/) for dashboard charts
- localStorage (swappable to REST API / MySQL / Firebase)

## License

Proprietary — TripZodo Travel Ventures Pvt Ltd
