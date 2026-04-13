# parksmart-sarajevo-issues


ParkSmart Sarajevo is a web application that provides real-time parking availability information across Sarajevo. The goal is to help drivers quickly locate free parking spots without unnecessary driving around the city. The application uses a simulated parking data layer that models real-time availability changes. The backend API is designed to be compatible with real sensor data sources, allowing future integration with live parking systems without frontend changes.

---

## Team

| Member | Features |
|--------|----------|
| Selma Karačić | F1 – Real-time Parking Map, F2 – Parking List & Filtering, F3 – Parking Detail Page |
| Zerina Kulić | F1 – User Registration & Login, F2 – Saved/Favourite Parkings, F3 – Search by Location |

---

## Zerina's Features

### F1 – User Registration & Login

Allows new users to create an account and existing users to log in securely. Authentication is required to access personalised features such as saved parkings. The system uses JWT-based session management. Passwords are hashed server-side and never returned in API responses.

**User Story:** As a driver, I want to register an account and log in so that I can access personalised features like saving my favourite parking locations.

**Acceptance Criteria:**
- Registration form accepts: full name, email address, and password
- Password must be at least 8 characters; confirmation field must match
- Email must be unique – duplicate registration returns a clear error message
- On successful registration, user is automatically logged in and redirected to the map
- Login form accepts email and password; invalid credentials show an error message
- Authenticated session persists across page reloads
- Logout clears the session and redirects to the login page
- Protected routes (/favourites) redirect unauthenticated users to the login page


---

### F2 – Saved/Favourite Parkings

Authenticated users can save parking locations to a personal favourites list for quick access. The favourites page shows each saved lot with its current availability status, refreshed every 60 seconds.

**User Story:** As a driver, I want to save my frequently used parking locations so that I can quickly check their availability without searching the full map every time.

**Acceptance Criteria:**
- A heart/bookmark icon on the Parking Detail Page allows the user to toggle a parking as a favourite
- Favourites are persisted server-side and linked to the user account
- The Favourites page (/favourites) lists all saved parkings with: name, zone, price, available spots, and status badge
- Availability data refreshes every 60 seconds automatically
- Removing a parking from favourites updates the list immediately without a full page reload
- If the user has no saved parkings, an empty state message is displayed
- The feature is inaccessible to unauthenticated users; they are redirected to the login page

---

### F3 – Search by Location

A search input that allows users to type a location or address in Sarajevo and view only the parking lots within a configurable radius. The map centers on the searched location and highlights nearby parking markers.

**User Story:** As a driver, I want to search for parking near a specific address so that I can find available spots close to my destination without manually scanning the map.

**Acceptance Criteria:**
- Search bar is visible on both the Map View and the Parking List page
- Autocomplete suggestions appear as the user types (based on Sarajevo addresses / known landmarks)
- On submission, the map centers on the resolved coordinates and zooms to an appropriate level
- Only parking lots within the default 500 m radius are shown; a radius selector (250 m / 500 m / 1 km) allows adjustment
- The parking list updates to reflect only lots within the selected radius
- A 'Clear search' button resets the view to the full city map and full parking list
- If no parking lots are found within the radius, a message is shown: 'No parking found near this location'
- Search works on both desktop and mobile screen sizes

---

## Selma's Features

### F1 – Parking List & Filtering

A structured list view of all parking locations, showing each lot's name, zone, price per hour, and current availability status. Users can filter the list by availability status, zone, and maximum hourly price, and sort results by availability or price. This view complements the map for users who prefer a text-based overview.

**User Story:**
As a driver, I want to filter and sort parking lots by availability and price so that I can quickly narrow down my options before deciding where to go.

**Acceptance Criteria:**
- List displays all parking lots with: name, zone, price per hour, available spots, and status badge
- Filter options: availability status (available / limited / full), zone, maximum price per hour
- Sort options: most available first, price low to high
- Multiple filters can be combined simultaneously (e.g. zone + max price)
- Empty state message is shown when no results match the active filters
- List data is consistent with the map view and reflects real-time availability

---

### F2 – Real-time Parking Map

An interactive map of Sarajevo displaying all parking locations as color-coded markers. Marker color reflects current availability: green for available, yellow for limited, and red for full. The map refreshes automatically every 60 seconds without a full page reload. Clicking a marker opens a popup with a short summary and a link to the full parking detail page.

**User Story:**
As a driver, I want to see all parking locations on a live map so that I can immediately identify where free spots are near my destination.

**Acceptance Criteria:**
- Map loads and displays all parking locations as markers on initial page load
- Marker color reflects real-time availability: green (available), yellow (limited), red (full)
- Clicking a marker shows a popup with: parking name, number of available spots, and a "View Details" link
- Map data refreshes every 60 seconds automatically, without user action
- Map is responsive and usable on mobile screen sizes

---

### F3 – Parking Detail Page

A dedicated page for each individual parking location showing complete information: name, address, total capacity, current available spots, price per hour, working hours, and an embedded mini-map with the exact location. A "Get Directions" button opens Google Maps navigation to that location. Availability is refreshed every 60 seconds.

**User Story:**
As a driver, I want to see full details about a specific parking lot so that I can confirm it suits my needs before driving there.

**Acceptance Criteria:**
- Page displays: name, address, zone, total capacity, current available spots, price per hour, working hours
- Embedded mini-map showing the parking lot's exact location
- "Get Directions" button opens Google Maps with that location as the destination
- Available spots count refreshes every 60 seconds automatically
- Back button or breadcrumb returns the user to the previous view (list or map)
- Page is accessible via a direct URL: `/parking/:id`

---


## Site Map

<img width="468" height="313" alt="Zerina's site map" src="https://github.com/user-attachments/assets/e1f344db-a9a7-4f7f-a5b2-dfe9380030e3" />

### Selma's features
<img width="921" height="496" alt="Slika zaslona 2026-04-09 u 16 52 02" src="https://github.com/user-attachments/assets/0d9015c7-7662-4724-b25a-4c0412d63495" />


### Combined navigation
```
ParkSmart Sarajevo
│
├── Home / Map View (/)                    ← Selma F2
│   └── Marker Popup (on click)
│       └── → Parking Detail Page
│
├── Parking List (/list)                   ← Selma F1
│   ├── Filter Panel (zone, price, status)
│   └── → Parking Detail Page
│
├── Parking Detail (/parking/:id)          ← Selma F3
│   └── → Google Maps (external)
│
├── Search (/search)                       ← Zerina F3
│   └── Autocomplete suggestions
│
├── Login (/login)                         ← Zerina F1
├── Register (/register)                   ← Zerina F1
│   └── → Redirect to /
│
└── Saved Parkings (/favourites)           ← Zerina F2
    ├── → Parking Detail (/parking/:id)
    └── requires login → /login
```


## Mockups

<img width="468" height="283" alt="Zerina's mockup" src="https://github.com/user-attachments/assets/f731601f-8e3c-47de-bd00-38dc1c9074fc" />
<img width="468" height="315" alt="Selma Mockup" src="https://github.com/user-attachments/assets/c6c84af5-1271-4322-8816-dec29454c5c3" />


## API Documentation
### Zerina's API endpoints
| Method | Endpoint | Feature | Description |
|--------|----------|---------|-------------|
| POST | `/auth/register` | F1 | Register new user |
| POST | `/auth/login` | F1 | Log in |
| POST | `/auth/logout` | F1 | Log out |
| GET | `/users/:userId/favourites` | F2 | Get saved parkings |
| POST | `/users/:userId/favourites` | F2 | Add to favourites |
| DELETE | `/users/:userId/favourites/:parkingId` | F2 | Remove from favourites |
| GET | `/parkings/search?lat=&lng=&radius=` | F3 | Search by location/radius |
| GET | `/locations/autocomplete?q=` | F3 | Autocomplete suggestions |

F1 – User Registration & Login
POST /auth/register
Registers a new user account. Called when the registration form is submitted.

Request
POST /auth/register
Content-Type: application/json

{
  "full_name": "Zerina Kulic",
  "email": "zerina@example.com",
  "password": "SecurePass123"
}

Request Payload
Field	Type	Required	Description
full_name	string	Yes	User full name
email	string	Yes	Unique email address
password	string	Yes	Minimum 8 characters

Response (201 Created)
{
  "id": "u01",
  "full_name": "Zerina Kulic",
  "email": "zerina@example.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

Error (409 Conflict)
{
  "error": "Email already in use",
  "code": 409
}

POST /auth/login
Authenticates an existing user. Called when the login form is submitted.

Request
POST /auth/login
Content-Type: application/json

{
  "email": "zerina@example.com",
  "password": "SecurePass123"
}

Response (200 OK)
{
  "id": "u01",
  "full_name": "Zerina Kulic",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

Error (401 Unauthorized)
{
  "error": "Invalid email or password",
  "code": 401
}

POST /auth/logout
Invalidates the current session token.

Request
POST /auth/logout
Authorization: Bearer <token>

Response (200 OK)
{
  "message": "Logged out successfully"
}

F2 – Saved/Favourite Parkings
GET /users/:userId/favourites
Returns all saved parkings for the authenticated user with current availability.

Request
GET /users/u01/favourites
Authorization: Bearer <token>

Response (200 OK)
[
  {
    "id": "p01",
    "name": "Parking Skenderija",
    "zone": "Zone 1",
    "price_per_hour": 1.50,
    "available_spots": 22,
    "status": "available"
  }
]

POST /users/:userId/favourites
Adds a parking lot to the favourites.

Request
POST /users/u01/favourites
Authorization: Bearer <token>
Content-Type: application/json

{
  "parking_id": "p01"
}

Response (201 Created)
{
  "message": "Parking added to favourites",
  "parking_id": "p01"
}

DELETE /users/:userId/favourites/:parkingId
Removes a parking lot from favourites.

Request
DELETE /users/u01/favourites/p01
Authorization: Bearer <token>

Response (200 OK)
{
  "message": "Parking removed from favourites",
  "parking_id": "p01"
}

F3 – Search by Location
GET /parkings/search
Returns parking lots within a given radius of the specified coordinates.

Request
GET /parkings/search?lat=43.8563&lng=18.4131&radius=500

Query Parameters


<img width="468" height="99" alt="image" src="https://github.com/user-attachments/assets/c6e0f8a6-3e8e-4c7b-a71f-d66d74ea8651" />


Response (200 OK)
[
  {
    "id": "p01",
    "name": "Parking A",
    "latitude": 43.8563,
    "longitude": 18.4131,
    "zone": "Zone 1",
    "total_capacity": 50,
    "available_spots": 18,
    "status": "available",
    "price_per_hour": 1.50,
    "distance_meters": 320
  }
]

GET /locations/autocomplete
Returns address suggestions for the search bar autocomplete, filtered to Sarajevo.

Request
GET /locations/autocomplete?q=Bascarsija

Query Parameters

Parameter	Description
q	        Partial address or landmark name (min 2 characters)
<img width="468" height="79" alt="image" src="https://github.com/user-attachments/assets/79df8137-058b-4f58-9533-98953f3e6cce" />



Response (200 OK)
[
  {
    "label": "Bascarsija, Sarajevo",
    "latitude": 43.8594,
    "longitude": 18.4319
  }
]




## Selma's endpoints

All endpoints return JSON. Base URL: `http://localhost:8080/api/v1`

### F1 – Parking List & Filtering

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/parkings` | Fetch all parking lots |
| GET | `/parkings?zone=&status=&max_price=&sort=` | Fetch filtered and sorted list |

**Request parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `zone` | string | No | Filter by zone name |
| `status` | string | No | `available`, `limited`, or `full` |
| `max_price` | number | No | Maximum price per hour in KM |
| `sort` | string | No | `available_desc` or `price_asc` |

**Response:**
```json
[
  {
    "id": "p01",
    "name": "Parking Skenderija",
    "zone": "Zone 1",
    "total_capacity": 80,
    "available_spots": 32,
    "status": "available",
    "price_per_hour": 1.50
  }
]
```

---

### F2 – Real-time Parking Map

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/parkings` | Fetch all parking locations with coordinates |

**Response:**
```json
[
  {
    "id": "p01",
    "name": "Parking Skenderija",
    "latitude": 43.8563,
    "longitude": 18.4131,
    "status": "available",
    "available_spots": 32
  }
]
```

---

### F3 – Parking Detail Page

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/parkings/:id` | Fetch full details for one parking lot |

**Response:**
```json
{
  "id": "p01",
  "name": "Parking Skenderija",
  "address": "Ulica Skenderija 1",
  "latitude": 43.8563,
  "longitude": 18.4131,
  "zone": "Zone 1",
  "total_capacity": 80,
  "available_spots": 32,
  "status": "available",
  "price_per_hour": 1.50,
  "working_hours": "07:00–22:00",
  "google_maps_url": "https://maps.google.com/?q=43.8563,18.4131"
}
```

**Error response:**
```json
{
  "error": "Parking not found",
  "code": 404
}
```
