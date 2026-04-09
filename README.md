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

## Site Map

<img width="468" height="313" alt="Zerina's site map" src="https://github.com/user-attachments/assets/e1f344db-a9a7-4f7f-a5b2-dfe9380030e3" />

## Mockups

<img width="468" height="283" alt="Zerina's mockup" src="https://github.com/user-attachments/assets/f731601f-8e3c-47de-bd00-38dc1c9074fc" />

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
