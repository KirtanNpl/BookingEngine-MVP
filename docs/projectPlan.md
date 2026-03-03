#  Booking Engine Development Plan (MVP Roadmap)

---

#  Start Plan (Not the final one. Might get changed while working)

---

## Phase 0 — Foundation 

**Goal:** Backend runs locally with DB and basic module skeletons.

### Tasks

#### 1. Create Repo + Solution Structure + Frontend setup

- BookingEngine.Booking.Api (Host)
- BookingEngine.Shared
- BookingEngine.Contracts

Create empty module projects:

- Catalog
- Inventory
- Rates
- Search
- Booking
- Identity
- Notifications

- Finally create a Next app for frontend.

#### 2. Infrastructure Setup

Add Docker Compose for:

- Postgres
- MinIO
- Redis (optional)

#### 3. Setup Basics

- Add EF Core connection
- Run simple DB connection test endpoint
- Add Swagger
- Add Health checks

###  Exit Criteria

- GET /health works
- Swagger loads

---

## Phase 1 — Catalog (Rooms) + Admin Login

**Why first:** Everything depends on room types.

### Identity Module

- Simple admin JWT authentication

### Catalog Module

- Room Types table (DB-first)
- Scaffold model
- Endpoints:
  - Create room type
  - List room types
  - Update room type
- Upload images (local first, MinIO later)

###  Exit Criteria

- Can create room types
- Can list room types via API

---

## Phase 2 — Inventory (Stock Calendar)

**Why now:** Booking correctness depends on stock.

### Inventory

- Table: inv_daily_stock

### Admin Endpoints

- Set stock / stop-sell for date range
- Query availability for a room type for date range

###  Exit Criteria

- Can set stock
- Can read remaining availability

---

## Phase 3 — Rates (Pricing)

**Why now:** Search needs price + availability.

### Rates

- Rate plans
- Daily prices (or seasonal ranges → generate daily)

### Pricing Function

- Compute total for date range
- Return breakdown:
  - Room total
  - Taxes
  - Fees

###  Exit Criteria

- Can compute total price for a room type for dates

---

## Phase 4 — Search (Availability + Price)

**Goal:** Show bookable options.

### Public Endpoint

- `/api/public/search`
- Input: dates + guests + rooms
- Output: room offers + total + breakdown

### Additional

- Add price snapshot table
- Optional Redis caching later

###  Exit Criteria

- Search returns correct offers and totals

---

## Phase 5 — Booking (Hold → Confirm → Email)

**This is the real booking engine.**

### Hold

- TTL: 10–15 minutes

### Confirm Booking

- Transactional inventory deduction (SP or safe UPDATE)
- Create reservation record

### Notifications

- Send confirmation email
- Queue + log email

###  Exit Criteria

- End-to-end booking works reliably

---

## Phase 6 — Admin Operations + Hardening

### Admin Features

- Bookings list
- Booking detail
- Cancel booking (restore stock)
- Resend confirmation

### Hardening

- Concurrency testing (last room case)
- Rate limiting
- Input validation

###  Exit Criteria

- Ready for MVP release