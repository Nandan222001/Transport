

# AI-Based Logistics / Transport Management System - Complete System Design

---

## Document 1: System Architecture, DFD, ER Diagram, Use Case, Sequence & Flow Diagrams

---

## 1. HIGH-LEVEL SYSTEM ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        AI-BASED LOGISTICS MANAGEMENT SYSTEM                     │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │  MOBILE APP   │  │   WEB APP    │  │  ADMIN PANEL │  │ VISUALIZATION    │   │
│  │  (Driver)     │  │  (Client)    │  │  (Operator)  │  │ PLATFORM         │   │
│  │              │  │              │  │              │  │ (Reports/BI)     │   │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘   │
│         │                 │                 │                    │              │
│  ───────┴─────────────────┴─────────────────┴────────────────────┴──────────   │
│                              API GATEWAY                                        │
│                         (REST API / GraphQL)                                    │
│  ──────────────────────────────┬─────────────────────────────────────────────   │
│                                │                                                │
│  ┌─────────────────────────────┴──────────────────────────────────────────┐     │
│  │                      MICROSERVICES LAYER                               │     │
│  │                                                                        │     │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────────┐  │     │
│  │  │ Auth       │ │ Trip       │ │ Tracking   │ │ Payment            │  │     │
│  │  │ Service    │ │ Service    │ │ Service    │ │ Service            │  │     │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────────────┘  │     │
│  │                                                                        │     │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────────┐  │     │
│  │  │ Driver     │ │ Fleet      │ │ Notification│ │ Report             │  │     │
│  │  │ Service    │ │ Service    │ │ Service    │ │ Service            │  │     │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────────────┘  │     │
│  │                                                                        │     │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐                         │     │
│  │  │ Demand     │ │ Dispatch   │ │ Delay      │                         │     │
│  │  │ Service    │ │ Service    │ │ Detection  │                         │     │
│  │  └────────────┘ └────────────┘ └────────────┘                         │     │
│  └───────────────────────────────┬────────────────────────────────────────┘     │
│                                  │                                              │
│  ┌───────────────────────────────┴────────────────────────────────────────┐     │
│  │                         AI / ML ENGINE                                  │     │
│  │                                                                        │     │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐  │     │
│  │  │ ETA          │ │ Route        │ │ Dynamic      │ │ Driver       │  │     │
│  │  │ Prediction   │ │ Optimization │ │ Pricing      │ │ Scoring      │  │     │
│  │  │ Model        │ │ Model        │ │ Model        │ │ Model        │  │     │
│  │  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘  │     │
│  │                                                                        │     │
│  │  ┌──────────────┐ ┌──────────────┐                                    │     │
│  │  │ Delay        │ │ Demand       │                                    │     │
│  │  │ Detection    │ │ Forecasting  │                                    │     │
│  │  │ Model        │ │ Model        │                                    │     │
│  │  └──────────────┘ └──────────────┘                                    │     │
│  └───────────────────────────────┬────────────────────────────────────────┘     │
│                                  │                                              │
│  ┌───────────────────────────────┴────────────────────────────────────────┐     │
│  │                         DATA LAYER                                      │     │
│  │                                                                        │     │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐  │     │
│  │  │ PostgreSQL   │ │ MongoDB      │ │ Redis        │ │ InfluxDB     │  │     │
│  │  │ (Relational) │ │ (Documents)  │ │ (Cache)      │ │ (Time-series)│  │     │
│  │  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘  │     │
│  └────────────────────────────────────────────────────────────────────────┘     │
│                                                                                 │
│  ┌────────────────────────────────────────────────────────────────────────┐     │
│  │                    EXTERNAL INTEGRATIONS                                │     │
│  │  Google Maps API │ Payment Gateway │ SMS/Email │ Weather API │ IoT    │     │
│  └────────────────────────────────────────────────────────────────────────┘     │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. DATA FLOW DIAGRAMS (DFD)

### 2.1 Context Diagram (DFD Level 0)

```
                    ┌─────────────┐
    Trip Request    │             │   Trip Confirmation
  ─────────────────►│             │◄─────────────────────
                    │             │
  ┌──────────┐      │             │      ┌──────────┐
  │          │      │   AI-BASED  │      │          │
  │  CLIENT  │◄────►│  LOGISTICS  │◄────►│  DRIVER  │
  │          │      │  MANAGEMENT │      │          │
  └──────────┘      │   SYSTEM    │      └──────────┘
    Tracking Info   │             │   Location Updates
  ◄─────────────────│             │◄─────────────────────
    Payment Receipt │             │   Trip Status
  ◄─────────────────│             │─────────────────────►
                    │             │
  ┌──────────┐      │             │      ┌──────────────┐
  │          │      │             │      │              │
  │  ADMIN   │◄────►│             │◄────►│  EXTERNAL    │
  │          │      │             │      │  SERVICES    │
  └──────────┘      │             │      │  (Maps,Pay)  │
    Reports/        └─────────────┘      └──────────────┘
    Analytics           │
                        │
                   ┌────┴────┐
                   │ AI/ML   │
                   │ ENGINE  │
                   └─────────┘
                   Predictions
                   & Insights
```

### 2.2 DFD Level 1

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                            DFD LEVEL 1                                         │
│                                                                                │
│  ┌────────┐     Trip Request      ┌─────────────────┐                         │
│  │ CLIENT │──────────────────────►│  1.0 DEMAND     │                         │
│  │        │◄──────────────────────│    CREATION     │                         │
│  └───┬────┘  Confirmation/Reject  └────────┬────────┘                         │
│      │                                      │                                  │
│      │                                      │ Demand Data                      │
│      │                                      ▼                                  │
│      │                            ┌─────────────────┐      ┌──────────────┐   │
│      │                            │  2.0 AI TRIP    │◄────►│ Google Maps  │   │
│      │                            │    PLANNING     │      │ API          │   │
│      │                            └────────┬────────┘      └──────────────┘   │
│      │                                      │                                  │
│      │                              Route + Price + Slot                       │
│      │                                      ▼                                  │
│      │                            ┌─────────────────┐                         │
│      │                            │  3.0 DISPATCH   │                         │
│  ┌───┴────┐                       │  & ASSIGNMENT   │                         │
│  │        │                       └────────┬────────┘                         │
│  │  D1    │                                │                                  │
│  │ Trip   │◄───────────────────────────────┤  Trip Assignment                 │
│  │ Store  │                                ▼                                  │
│  │        │                       ┌─────────────────┐      ┌──────────────┐   │
│  └────────┘                       │  4.0 EXECUTION  │◄────►│   DRIVER     │   │
│                                   │  & TRACKING     │      │              │   │
│                                   └────────┬────────┘      └──────────────┘   │
│                                            │                                  │
│                                     GPS + Status Data                         │
│                                            ▼                                  │
│                                   ┌─────────────────┐                         │
│                                   │  5.0 DELAY      │                         │
│                                   │  DETECTION (AI) │                         │
│                                   └────────┬────────┘                         │
│                                            │                                  │
│                                      Alert / Normal                           │
│                                            ▼                                  │
│                                   ┌─────────────────┐      ┌──────────────┐   │
│                                   │  6.0 DELIVERY   │─────►│   PAYMENT    │   │
│                                   │  COMPLETION     │      │   GATEWAY    │   │
│                                   └────────┬────────┘      └──────────────┘   │
│                                            │                                  │
│                                   Completed Trip Data                         │
│                                            ▼                                  │
│  ┌────────┐                       ┌─────────────────┐                         │
│  │ ADMIN  │◄─────────────────────│  7.0 REPORTS    │                         │
│  │        │   Reports/Analytics   │  & ANALYTICS   │                         │
│  └────────┘                       └────────┬────────┘                         │
│                                            │                                  │
│                                            ▼                                  │
│                                   ┌─────────────────┐                         │
│                                   │  8.0 AI         │                         │
│                                   │  LEARNING       │                         │
│                                   │  (Feedback Loop)│                         │
│                                   └─────────────────┘                         │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

### 2.3 DFD Level 2 - Process 2.0 (AI Trip Planning - Expanded)

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                    DFD LEVEL 2 — PROCESS 2.0: AI TRIP PLANNING                 │
│                                                                                │
│                          Demand Data (from 1.0)                                │
│                                  │                                             │
│                                  ▼                                             │
│                        ┌──────────────────┐                                    │
│                        │ 2.1 VALIDATE     │                                    │
│                        │ DEMAND REQUEST   │                                    │
│                        └────────┬─────────┘                                    │
│                                 │                                              │
│                          Valid Demand                                           │
│                                 │                                              │
│               ┌─────────────────┼─────────────────┐                            │
│               ▼                 ▼                  ▼                            │
│     ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐                  │
│     │ 2.2 GENERATE │  │ 2.3 CALCULATE│  │ 2.4 AI DYNAMIC   │                  │
│     │ TIME SLOTS   │  │ DISTANCE &   │  │ PRICING ENGINE   │                  │
│     │              │  │ ROUTE        │  │                  │                  │
│     │ Morning Slot │  │              │  │ Base Price       │                  │
│     │ Afternoon    │  │ Google Maps  │  │ + Distance Cost  │                  │
│     │ Evening Slot │  │ API Call     │  │ + Time Surcharge │                  │
│     │ Urgent       │  │              │  │ + Demand Factor  │                  │
│     └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘                  │
│            │                 │                    │                             │
│            └─────────────────┼────────────────────┘                            │
│                              ▼                                                 │
│                    ┌──────────────────┐                                         │
│                    │ 2.5 OPTIMIZE     │                                         │
│                    │ ROUTE (AI Model) │                                         │
│                    │                  │                                         │
│                    │ Input:           │                                         │
│                    │ - Traffic data   │                                         │
│                    │ - Weather        │                                         │
│                    │ - Road conditions│                                         │
│                    │ - Historical data│                                         │
│                    └────────┬─────────┘                                         │
│                             │                                                  │
│                     Optimized Plan                                              │
│                             ▼                                                  │
│                    ┌──────────────────┐                                         │
│                    │ 2.6 PREDICT ETA  │                                         │
│                    │ (ML Model)       │                                         │
│                    └────────┬─────────┘                                         │
│                             │                                                  │
│                   Trip Plan + Price + ETA                                       │
│                             │                                                  │
│                             ▼                                                  │
│                      (To Process 3.0)                                          │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

### 2.4 DFD Level 2 - Process 5.0 (Delay Detection - Expanded)

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                 DFD LEVEL 2 — PROCESS 5.0: AI DELAY DETECTION                  │
│                                                                                │
│                    GPS + Status Data (from 4.0)                                │
│                                │                                               │
│                                ▼                                               │
│                     ┌──────────────────┐                                        │
│                     │ 5.1 CONTINUOUS   │                                        │
│                     │ LOCATION MONITOR │                                        │
│                     └────────┬─────────┘                                        │
│                              │                                                 │
│                     Current Position + Speed                                   │
│                              │                                                 │
│                              ▼                                                 │
│                     ┌──────────────────┐        ┌──────────────────┐           │
│                     │ 5.2 COMPARE WITH │◄──────│ D2: PLANNED      │           │
│                     │ PLANNED ROUTE    │       │ ROUTE STORE      │           │
│                     └────────┬─────────┘        └──────────────────┘           │
│                              │                                                 │
│                    Deviation Detected?                                          │
│                     ┌────────┴────────┐                                        │
│                     │                 │                                         │
│                  YES ▼              NO ▼                                        │
│          ┌──────────────┐    ┌──────────────┐                                  │
│          │ 5.3 CALCULATE│    │ Continue     │                                  │
│          │ DELAY TIME   │    │ Monitoring   │                                  │
│          └──────┬───────┘    └──────────────┘                                  │
│                 │                                                               │
│           Delay > 30 min?                                                      │
│            ┌────┴────┐                                                         │
│            │         │                                                         │
│         YES ▼      NO ▼                                                        │
│   ┌────────────┐  ┌────────────┐                                               │
│   │ 5.4 TRIGGER│  │ 5.5 LOG    │                                               │
│   │ ALERT      │  │ MINOR      │                                               │
│   │            │  │ DELAY      │                                               │
│   │ → Client   │  └────────────┘                                               │
│   │ → Admin    │                                                               │
│   │ → Driver   │                                                               │
│   └────────────┘                                                               │
│          │                                                                     │
│          ▼                                                                     │
│   ┌────────────────┐                                                           │
│   │ 5.6 RECALCULATE│                                                           │
│   │ ETA (AI Model) │                                                           │
│   └────────────────┘                                                           │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. ENTITY-RELATIONSHIP DIAGRAM (ERD)

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                           ENTITY-RELATIONSHIP DIAGRAM                            │
│                                                                                  │
│  ┌─────────────────┐          ┌─────────────────┐          ┌─────────────────┐  │
│  │      USER        │          │      TRIP        │          │     TRUCK       │  │
│  ├─────────────────┤          ├─────────────────┤          ├─────────────────┤  │
│  │ PK: user_id     │          │ PK: trip_id     │          │ PK: truck_id    │  │
│  │ name            │          │ FK: client_id   │──┐       │ reg_number      │  │
│  │ email           │    ┌────►│ FK: driver_id   │  │       │ capacity        │  │
│  │ password_hash   │    │     │ FK: truck_id    │──┼──────►│ type            │  │
│  │ phone           │    │     │ origin          │  │       │ status          │  │
│  │ role (ENUM)     │    │     │ destination     │  │       │ current_location│  │
│  │  - ADMIN        │    │     │ distance_km     │  │       │ fuel_type       │  │
│  │  - CLIENT       │────┘     │ price           │  │       │ mileage         │  │
│  │  - DRIVER       │────┐     │ status (ENUM)   │  │       │ last_service    │  │
│  │ created_at      │    │     │  - PENDING      │  │       │ FK: owner_id    │  │
│  │ is_active       │    │     │  - ASSIGNED     │  │       └─────────────────┘  │
│  └────────┬────────┘    │     │  - IN_TRANSIT   │  │                            │
│           │             │     │  - DELIVERED    │  │       ┌─────────────────┐  │
│           │             │     │  - CANCELLED    │  │       │   TIME_SLOT     │  │
│           │             │     │ time_slot_id    │──┼──────►├─────────────────┤  │
│           │             │     │ planned_eta     │  │       │ PK: slot_id     │  │
│           │             │     │ actual_eta      │  │       │ FK: trip_id     │  │
│           │             │     │ created_at      │  │       │ slot_type       │  │
│           │             │     │ completed_at    │  │       │  - MORNING      │  │
│           │             │     └────────┬────────┘  │       │  - AFTERNOON    │  │
│           │             │              │           │       │  - EVENING      │  │
│           │             │              │           │       │  - URGENT       │  │
│           │             │              │           │       │ start_time      │  │
│  ┌────────┴────────┐    │     ┌────────┴────────┐  │       │ end_time        │  │
│  │  DRIVER_PROFILE  │    │     │   TRACKING      │  │       │ price_multiplier│  │
│  ├─────────────────┤    │     ├─────────────────┤  │       └─────────────────┘  │
│  │ PK: profile_id  │    │     │ PK: tracking_id │  │                            │
│  │ FK: user_id     │◄───┘     │ FK: trip_id     │  │       ┌─────────────────┐  │
│  │ license_number  │          │ latitude        │  │       │    PAYMENT      │  │
│  │ license_expiry  │          │ longitude       │  │       ├─────────────────┤  │
│  │ experience_years│          │ speed           │  ├──────►│ PK: payment_id  │  │
│  │ rating          │          │ heading         │         │ FK: trip_id     │  │
│  │ total_trips     │          │ timestamp       │         │ amount          │  │
│  │ performance_score│         │ status          │         │ payment_type    │  │
│  │ current_location│          └─────────────────┘         │  - DRIVER_ONLY  │  │
│  │ availability    │                                      │  - DRIVER_TRUCK │  │
│  └─────────────────┘          ┌─────────────────┐         │ status          │  │
│                               │     DELAY       │         │  - PENDING      │  │
│  ┌─────────────────┐          ├─────────────────┤         │  - PROCESSED    │  │
│  │  NOTIFICATION   │          │ PK: delay_id    │         │  - FAILED       │  │
│  ├─────────────────┤          │ FK: trip_id     │         │ processed_at    │  │
│  │ PK: notif_id   │          │ delay_minutes   │         └─────────────────┘  │
│  │ FK: user_id     │          │ reason          │                               │
│  │ type            │          │ detected_at     │         ┌─────────────────┐  │
│  │ message         │          │ resolved_at     │         │   AI_LOG        │  │
│  │ is_read         │          │ ai_prediction   │         ├─────────────────┤  │
│  │ created_at      │          └─────────────────┘         │ PK: log_id      │  │
│  └─────────────────┘                                      │ FK: trip_id     │  │
│                               ┌─────────────────┐         │ model_type      │  │
│                               │   ROUTE         │         │ prediction      │  │
│                               ├─────────────────┤         │ actual_value    │  │
│                               │ PK: route_id    │         │ accuracy        │  │
│                               │ FK: trip_id     │         │ timestamp       │  │
│                               │ waypoints (JSON)│         └─────────────────┘  │
│                               │ total_distance  │                               │
│                               │ optimized_path  │         ┌─────────────────┐  │
│                               │ traffic_data    │         │  DEMAND         │  │
│                               │ weather_data    │         ├─────────────────┤  │
│                               └─────────────────┘         │ PK: demand_id   │  │
│                                                           │ FK: client_id   │  │
│                                                           │ source_type     │  │
│                                                           │  - MANUAL       │  │
│                                                           │  - SENSOR       │  │
│                                                           │  - AUTOMATED    │  │
│                                                           │ origin          │  │
│                                                           │ destination     │  │
│                                                           │ cargo_type      │  │
│                                                           │ weight          │  │
│                                                           │ priority        │  │
│                                                           │ status          │  │
│                                                           │ created_at      │  │
│                                                           └─────────────────┘  │
│                                                                                  │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### ERD Relationships Summary

```
USER (1) ────────── (M) TRIP              [Client creates many trips]
USER (1) ────────── (M) TRIP              [Driver assigned many trips]
TRUCK (1) ───────── (M) TRIP              [Truck used in many trips]
TRIP (1) ─────────── (M) TRACKING         [Trip has many tracking points]
TRIP (1) ─────────── (1) ROUTE            [Trip has one optimized route]
TRIP (1) ─────────── (1) PAYMENT          [Trip has one payment]
TRIP (1) ─────────── (M) DELAY            [Trip can have many delays]
TRIP (1) ─────────── (1) TIME_SLOT        [Trip booked in one time slot]
TRIP (1) ─────────── (M) AI_LOG           [Trip has many AI predictions]
USER (1) ────────── (1) DRIVER_PROFILE    [Driver has one profile]
USER (1) ────────── (M) NOTIFICATION      [User gets many notifications]
CLIENT (1) ──────── (M) DEMAND            [Client creates many demands]
```

---

## 4. USE CASE DIAGRAM

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                              USE CASE DIAGRAM                                    │
│                                                                                  │
│  ┌─────────┐                                                    ┌─────────┐     │
│  │         │    ┌─────────────────────────────────────────┐      │         │     │
│  │         │    │           SYSTEM BOUNDARY               │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC1: Register / Login           │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │  CLIENT │───►│  │ UC2: Create Trip Demand         │   │      │  ADMIN  │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC3: Select Time Slot           │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC4: Track Delivery Live        │   │◄────│         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC5: View ETA & Alerts          │   │◄────│         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC6: Make / Receive Payment     │   │◄────│         │     │
│  └─────────┘    │  └─────────────────────────────────┘   │      │         │     │
│                 │                                         │      │         │     │
│  ┌─────────┐    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │    │  │ UC7: Accept / Reject Trip       │   │      │         │     │
│  │         │───►│  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │  DRIVER │───►│  │ UC8: Navigate Route             │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC9: Update Delivery Status     │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │                                         │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC10: Send Live Location        │   │      │         │     │
│  └─────────┘    │  └─────────────────────────────────┘   │      │         │     │
│                 │                                         │      │         │     │
│                 │  ┌─────────────────────────────────┐   │      │         │     │
│                 │  │ UC11: Manage Trucks & Drivers   │   │◄────│         │     │
│                 │  └─────────────────────────────────┘   │      │         │     │
│                 │                                         │      │         │     │
│                 │  ┌─────────────────────────────────┐   │      │         │     │
│                 │  │ UC12: Monitor All Trips         │   │◄────│         │     │
│                 │  └─────────────────────────────────┘   │      │         │     │
│                 │                                         │      │         │     │
│                 │  ┌─────────────────────────────────┐   │      │         │     │
│                 │  │ UC13: View Reports & Analytics  │   │◄────│         │     │
│                 │  └─────────────────────────────────┘   │      │         │     │
│                 │                                         │      │         │     │
│  ┌─────────┐    │  ┌─────────────────────────────────┐   │      │         │     │
│  │  AI     │    │  │ UC14: Predict ETA               │   │      │         │     │
│  │ ENGINE  │───►│  └─────────────────────────────────┘   │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC15: Detect Delays             │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC16: Optimize Route             │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC17: Dynamic Pricing           │   │      │         │     │
│  │         │    │  └─────────────────────────────────┘   │      │         │     │
│  │         │    │  ┌─────────────────────────────────┐   │      │         │     │
│  │         │───►│  │ UC18: Learn & Improve           │   │      │         │     │
│  └─────────┘    │  └─────────────────────────────────┘   │      └─────────┘     │
│                 │                                         │                      │
│                 └─────────────────────────────────────────┘                      │
│                                                                                  │
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. SEQUENCE DIAGRAMS

### 5.1 Trip Booking & Planning Sequence

```
  CLIENT          WEB APP        API GATEWAY      DEMAND SVC      AI ENGINE       MAPS API        DB
    │                │                │                │              │               │             │
    │  Login         │                │                │              │               │             │
    │───────────────►│                │                │              │               │             │
    │                │  Auth Request  │                │              │               │             │
    │                │───────────────►│                │              │               │             │
    │                │  JWT Token     │                │              │               │             │
    │                │◄───────────────│                │              │               │             │
    │  Token         │                │                │              │               │             │
    │◄───────────────│                │                │              │               │             │
    │                │                │                │              │               │             │
    │  Create Trip   │                │                │              │               │             │
    │  Request       │                │                │              │               │             │
    │───────────────►│                │                │              │               │             │
    │                │  POST /demand  │                │              │               │             │
    │                │───────────────►│                │              │               │             │
    │                │                │  Create Demand │              │               │             │
    │                │                │───────────────►│              │               │             │
    │                │                │                │  Validate    │               │             │
    │                │                │                │──────┐       │               │             │
    │                │                │                │      │       │               │             │
    │                │                │                │◄─────┘       │               │             │
    │                │                │                │              │               │             │
    │                │                │                │  Get Route   │               │             │
    │                │                │                │─────────────►│               │             │
    │                │                │                │              │  Distance API │             │
    │                │                │                │              │──────────────►│             │
    │                │                │                │              │  Distance +   │             │
    │                │                │                │              │  Duration     │             │
    │                │                │                │              │◄──────────────│             │
    │                │                │                │              │               │             │
    │                │                │                │  Calc Price  │               │             │
    │                │                │                │  + Predict   │               │             │
    │                │                │                │  ETA         │               │             │
    │                │                │                │──────┐       │               │             │
    │                │                │                │      │ML     │               │             │
    │                │                │                │◄─────┘       │               │             │
    │                │                │                │              │               │             │
    │                │                │                │ Gen Slots    │               │             │
    │                │                │                │──────┐       │               │             │
    │                │                │                │      │       │               │             │
    │                │                │                │◄─────┘       │               │             │
    │                │                │                │              │               │             │
    │                │                │                │  Save Demand │               │             │
    │                │                │                │─────────────────────────────────────────►│
    │                │                │                │              │               │  Saved     │
    │                │                │                │◄────────────────────────────────────────│
    │                │                │                │              │               │             │
    │                │                │  Trip Options  │              │               │             │
    │                │                │  (Slots+Price) │              │               │             │
    │                │                │◄───────────────│              │               │             │
    │                │  Display Opts  │                │              │               │             │
    │                │◄───────────────│                │              │               │             │
    │  Show Options  │                │                │              │               │             │
    │◄───────────────│                │                │              │               │             │
    │                │                │                │              │               │             │
    │  Select Slot   │                │                │              │               │             │
    │  + Confirm     │                │                │              │               │             │
    │───────────────►│                │                │              │               │             │
    │                │  POST /confirm │                │              │               │             │
    │                │───────────────►│                │              │               │             │
    │                │                │  Confirm Trip  │              │               │             │
    │                │                │───────────────►│              │               │             │
    │                │                │                │  Update DB   │               │             │
    │                │                │                │─────────────────────────────────────────►│
    │                │                │  Confirmation  │              │               │             │
    │                │◄───────────────│                │              │               │             │
    │  Trip Confirmed│                │                │              │               │             │
    │◄───────────────│                │                │              │               │             │
    │                │                │                │              │               │             │
```

### 5.2 Dispatch & Delivery Sequence

```
  DISPATCH SVC     DRIVER SVC      DRIVER APP       TRACKING SVC    DELAY AI      NOTIFICATION
       │                │               │                │              │               │
       │  Find Best     │               │                │              │               │
       │  Driver        │               │                │              │               │
       │───────────────►│               │                │              │               │
       │                │  Check        │                │              │               │
       │                │  Availability │                │              │               │
       │                │──────┐        │                │              │               │
       │                │      │        │                │              │               │
       │                │◄─────┘        │                │              │               │
       │  Driver Found  │               │                │              │               │
       │◄───────────────│               │                │              │               │
       │                │               │                │              │               │
       │  Assign Trip   │               │                │              │               │
       │───────────────►│               │                │              │               │
       │                │  Push Notif   │                │              │               │
       │                │──────────────►│                │              │               │
       │                │               │                │              │               │
       │                │  Accept Trip  │                │              │               │
       │                │◄──────────────│                │              │               │
       │  Trip Accepted │               │                │              │               │
       │◄───────────────│               │                │              │               │
       │                │               │                │              │               │
       │                │               │  Start Trip    │              │               │
       │                │               │───────────────►│              │               │
       │                │               │                │              │               │
       │                │               │                │              │               │
       │          ┌─────┤  GPS Loop     │                │              │               │
       │          │     │  (Every 30s)  │                │              │               │
       │          │     │               │  Send Location │              │               │
       │          │     │               │───────────────►│              │               │
       │          │     │               │                │              │               │
       │          │     │               │                │  Check Delay │               │
       │          │     │               │                │──────┐       │               │
       │          │     │               │                │      │       │               │
       │          │     │               │                │◄─────┘       │               │
       │          │     │               │                │              │               │
       │          │     │               │                │  IF DELAY    │               │
       │          │     │               │                │  > 30 min    │               │
       │          │     │               │                │─────────────►│  Alert        │
       │          │     │               │                │              │──────────────►│
       │          │     │               │                │              │  (to Client   │
       │          │     │               │                │              │   & Admin)    │
       │          └─────┤               │                │              │               │
       │                │               │                │              │               │
       │                │               │  Mark Delivered│              │               │
       │                │               │───────────────►│              │               │
       │                │               │                │              │               │
       │  Trip Complete │               │                │              │               │
       │◄───────────────────────────────│                │              │               │
       │                │               │                │              │               │
```

### 5.3 Payment Processing Sequence

```
  TRIP SVC         PAYMENT SVC      PAYMENT GW       DRIVER SVC        CLIENT
     │                  │                │                │               │
     │  Trip Delivered  │                │                │               │
     │─────────────────►│                │                │               │
     │                  │                │                │               │
     │                  │  Check Payment │                │               │
     │                  │  Type          │                │               │
     │                  │──────┐         │                │               │
     │                  │      │         │                │               │
     │                  │◄─────┘         │                │               │
     │                  │                │                │               │
     │                  │                │                │               │
     │    ┌─────────────┴──────┐         │                │               │
     │    │                    │         │                │               │
     │    ▼                    ▼         │                │               │
     │  DRIVER_ONLY     DRIVER+TRUCK    │                │               │
     │  (Pay Later)     (Pay Instant)   │                │               │
     │    │                    │         │                │               │
     │    │  Schedule          │ Process │                │               │
     │    │  Payment           │ Payment │                │               │
     │    │  (Mark Pending)    │────────►│                │               │
     │    │                    │         │  Charge Client │               │
     │    │                    │         │───────────────────────────────►│
     │    │                    │         │                │               │
     │    │                    │         │  Payment OK    │               │
     │    │                    │         │◄───────────────│               │
     │    │                    │         │                │               │
     │    │                    │  Pay    │                │               │
     │    │                    │  Driver │                │               │
     │    │                    │────────►│                │               │
     │    │                    │         │  Transfer     │               │
     │    │                    │         │──────────────►│               │
     │    │                    │         │               │               │
     │    │                    │  Done   │               │               │
     │    │                    │◄────────│               │               │
     │    │                    │         │               │               │
     │    └────────┬───────────┘         │               │               │
     │             │                     │               │               │
     │  Update     │                     │               │               │
     │  Records    │                     │               │               │
     │◄────────────│                     │               │               │
     │             │                     │               │               │
```

---

## 6. ACTIVITY / FLOW DIAGRAMS

### 6.1 Complete Trip Lifecycle Flow

```
                              ┌───────────┐
                              │   START   │
                              └─────┬─────┘
                                    │
                                    ▼
                           ┌─────────────────┐
                           │ Client Logs In  │
                           └────────┬────────┘
                                    │
                                    ▼
                           ┌─────────────────┐
                           │ Create Demand   │
                           │ (Origin, Dest,  │
                           │  Cargo, Weight) │
                           └────────┬────────┘
                                    │
                                    ▼
                          ┌──────────────────┐
                          │ Source of Demand?│
                          └───┬──────┬───┬───┘
                              │      │   │
                     Manual   │ Sensor│  Auto
                              │      │   │
                              ▼      ▼   ▼
                           ┌─────────────────┐
                           │ Validate Demand │
                           └────────┬────────┘
                                    │
                              ┌─────┴─────┐
                              │  Valid?   │
                              └──┬────┬───┘
                              NO │    │ YES
                                 ▼    ▼
                          ┌──────┐  ┌─────────────────┐
                          │Reject│  │ AI Plans Trip   │
                          │& Notif│ │                 │
                          └──────┘  │ - Calc Distance │
                                    │ - Gen Time Slots│
                                    │ - Dynamic Price │
                                    │ - Predict ETA   │
                                    │ - Optimize Route│
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Show Options to │
                                    │ Client          │
                                    │ (Slots + Price) │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Client Selects  │
                                    │ Slot & Confirms │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Find Available  │
                                    │ Driver + Truck  │
                                    └────────┬────────┘
                                             │
                                       ┌─────┴─────┐
                                       │ Found?    │
                                       └──┬────┬───┘
                                       NO │    │ YES
                                          ▼    ▼
                                   ┌──────┐  ┌─────────────────┐
                                   │Queue │  │ Assign Driver   │
                                   │Trip  │  │ & Truck         │
                                   └──────┘  └────────┬────────┘
                                                      │
                                                      ▼
                                             ┌─────────────────┐
                                             │ Notify Driver   │
                                             │ (Push Notif)    │
                                             └────────┬────────┘
                                                      │
                                                ┌─────┴─────┐
                                                │ Accepted? │
                                                └──┬────┬───┘
                                                NO │    │ YES
                                                   ▼    ▼
                                            ┌──────┐  ┌─────────────────┐
                                            │Find  │  │ Trip Starts     │
                                            │New   │  │ Driver Begins   │
                                            │Driver│  │ Navigation      │
                                            └──────┘  └────────┬────────┘
                                                               │
                                                               ▼
                                                 ┌──────────────────────┐
                                                 │ LIVE TRACKING LOOP   │
                                                 │                      │
                                                 │  GPS → Server        │
                                                 │  (Every 30 seconds)  │
                                                 │                      │
                                                 │  ┌────────────────┐  │
                                                 │  │ AI: Check      │  │
                                                 │  │ for Delay      │  │
                                                 │  └───────┬────────┘  │
                                                 │          │           │
                                                 │    ┌─────┴─────┐    │
                                                 │    │Delay>30m? │    │
                                                 │    └──┬────┬───┘    │
                                                 │    NO │    │ YES    │
                                                 │       ▼    ▼        │
                                                 │ ┌──────┐ ┌──────┐  │
                                                 │ │ OK   │ │ALERT │  │
                                                 │ │      │ │Send  │  │
                                                 │ │      │ │Notif │  │
                                                 │ │      │ │Recalc│  │
                                                 │ │      │ │ETA   │  │
                                                 │ └──────┘ └──────┘  │
                                                 │                      │
                                                 └──────────┬───────────┘
                                                            │
                                                            ▼
                                                   ┌─────────────────┐
                                                   │ Reached         │
                                                   │ Destination?    │
                                                   └───┬─────────┬───┘
                                                    NO │         │ YES
                                                       ▼         ▼
                                                  Continue  ┌─────────────────┐
                                                  Loop      │ Driver Marks    │
                                                            │ DELIVERED       │
                                                            └────────┬────────┘
                                                                     │
                                                                     ▼
                                                            ┌─────────────────┐
                                                            │ Check Payment   │
                                                            │ Type            │
                                                            └───┬─────────┬───┘
                                                                │         │
                                                     Driver Only│   Driver+Truck
                                                                ▼         ▼
                                                        ┌──────────┐ ┌──────────┐
                                                        │ Schedule │ │ Instant  │
                                                        │ Payment  │ │ Payment  │
                                                        │ (Later)  │ │ Process  │
                                                        └────┬─────┘ └────┬─────┘
                                                             │            │
                                                             └──────┬─────┘
                                                                    ▼
                                                           ┌─────────────────┐
                                                           │ Generate Report │
                                                           │ & Receipt       │
                                                           └────────┬────────┘
                                                                    │
                                                                    ▼
                                                           ┌─────────────────┐
                                                           │ AI Learns from  │
                                                           │ Trip Data       │
                                                           │ - Actual ETA    │
                                                           │ - Delay Patterns│
                                                           │ - Driver Perf   │
                                                           │ - Route Quality │
                                                           └────────┬────────┘
                                                                    │
                                                                    ▼
                                                              ┌───────────┐
                                                              │    END    │
                                                              └───────────┘
```

---

## 7. STATE DIAGRAM (Trip States)

```
                                 ┌─────────────┐
                                 │   CREATED   │
                                 │  (Initial)  │
                                 └──────┬──────┘
                                        │
                              Client Confirms
                                        │
                                        ▼
                                 ┌─────────────┐
                          ┌─────│   PENDING    │─────┐
                          │     │ (Awaiting    │     │
                          │     │  Driver)     │     │
                          │     └──────┬──────┘     │
                          │            │             │
                     No Driver    Driver Found    Client
                     Available    & Assigned      Cancels
                          │            │             │
                          ▼            ▼             ▼
                   ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
                   │   QUEUED    │ │  ASSIGNED   │ │  CANCELLED  │
                   │             │ │             │ │  (Terminal)  │
                   └──────┬──────┘ └──────┬──────┘ └─────────────┘
                          │               │
                   Driver Found      Driver Accepts
                          │               │
                          └───────┬───────┘
                                  │
                                  ▼
                           ┌─────────────┐
                           │  ACCEPTED   │
                           │             │
                           └──────┬──────┘
                                  │
                           Driver Starts
                             Trip
                                  │
                                  ▼
                           ┌─────────────┐
                     ┌────│ IN_TRANSIT  │────┐
                     │    │             │    │
                     │    └──────┬──────┘    │
                     │           │           │
                  Delay       Normal     Issue/
                  Detected   Progress    Breakdown
                     │           │           │
                     ▼           │           ▼
              ┌─────────────┐    │    ┌─────────────┐
              │   DELAYED   │    │    │   ON_HOLD   │
              │             │    │    │             │
              └──────┬──────┘    │    └──────┬──────┘
                     │           │           │
                     │    Resume │    Issue Resolved
                     └─────┬─────┘           │
                           │                 │
                           └────────┬────────┘
                                    │
                               Reached
                             Destination
                                    │
                                    ▼
                           ┌─────────────────┐
                           │   DELIVERED     │
                           │                 │
                           └────────┬────────┘
                                    │
                              Payment
                              Processed
                                    │
                                    ▼
                           ┌─────────────────┐
                           │   COMPLETED     │
                           │   (Terminal)     │
                           └─────────────────┘
```

---

## 8. COMPONENT DIAGRAM

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          COMPONENT DIAGRAM                                    │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                     PRESENTATION LAYER                               │    │
│  │                                                                      │    │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │    │
│  │  │ React.js     │  │ React Native │  │ Power BI     │               │    │
│  │  │ Web App      │  │ Mobile App   │  │ Dashboard    │               │    │
│  │  │              │  │              │  │              │               │    │
│  │  │ - Admin UI   │  │ - Driver UI  │  │ - Analytics  │               │    │
│  │  │ - Client UI  │  │ - Navigation │  │ - Reports    │               │    │
│  │  │ - Dashboard  │  │ - GPS Track  │  │ - KPIs       │               │    │
│  │  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘               │    │
│  └─────────┼─────────────────┼─────────────────┼────────────────────────┘    │
│            │                 │                 │                              │
│            └─────────────────┼─────────────────┘                             │
│                              │                                               │
│                              ▼                                               │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                      API GATEWAY LAYER                               │    │
│  │                                                                      │    │
│  │  ┌──────────────────────────────────────────────────────────────┐    │    │
│  │  │                   Kong / Nginx API Gateway                    │    │    │
│  │  │                                                              │    │    │
│  │  │  - Rate Limiting    - Load Balancing    - SSL Termination    │    │    │
│  │  │  - Authentication   - Request Routing   - Logging            │    │    │
│  │  └──────────────────────────────┬───────────────────────────────┘    │    │
│  └─────────────────────────────────┼────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                     BUSINESS LOGIC LAYER (Microservices)              │    │
│  │                                                                      │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐       │    │
│  │  │ Auth       │ │ Demand     │ │ Trip       │ │ Dispatch   │       │    │
│  │  │ Service    │ │ Service    │ │ Service    │ │ Service    │       │    │
│  │  │            │ │            │ │            │ │            │       │    │
│  │  │ JWT Auth   │ │ Create     │ │ CRUD Trips │ │ Auto       │       │    │
│  │  │ RBAC       │ │ Validate   │ │ Status Mgmt│ │ Assignment │       │    │
│  │  │ OAuth      │ │ Transform  │ │ Lifecycle  │ │ Scheduling │       │    │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────┘       │    │
│  │                                                                      │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐       │    │
│  │  │ Tracking   │ │ Fleet      │ │ Payment    │ │ Notificat- │       │    │
│  │  │ Service    │ │ Service    │ │ Service    │ │ ion Service│       │    │
│  │  │            │ │            │ │            │ │            │       │    │
│  │  │ GPS Ingest │ │ Truck Mgmt │ │ Process Pay│ │ Push/SMS   │       │    │
│  │  │ Geo Queries│ │ Driver Mgmt│ │ Invoicing  │ │ Email      │       │    │
│  │  │ History    │ │ Maintenance│ │ Settlements│ │ In-App     │       │    │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────┘       │    │
│  │                                                                      │    │
│  │  ┌────────────┐ ┌────────────┐                                      │    │
│  │  │ Report     │ │ Delay      │                                      │    │
│  │  │ Service    │ │ Service    │                                      │    │
│  │  │            │ │            │                                      │    │
│  │  │ Generate   │ │ Monitor    │                                      │    │
│  │  │ Export     │ │ Alert      │                                      │    │
│  │  │ Schedule   │ │ Escalate   │                                      │    │
│  │  └────────────┘ └────────────┘                                      │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                       AI / ML ENGINE LAYER                           │    │
│  │                                                                      │    │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐         │    │
│  │  │ ETA Prediction │  │ Route          │  │ Dynamic        │         │    │
│  │  │ Model          │  │ Optimization   │  │ Pricing Model  │         │    │
│  │  │ (XGBoost/LSTM) │  │ (Graph Neural  │  │ (Regression +  │         │    │
│  │  │                │  │  Network)      │  │  Supply/Demand)│         │    │
│  │  └────────────────┘  └────────────────┘  └────────────────┘         │    │
│  │                                                                      │    │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐         │    │
│  │  │ Delay          │  │ Driver         │  │ Demand         │         │    │
│  │  │ Detection      │  │ Scoring        │  │ Forecasting    │         │    │
│  │  │ (Anomaly Det.) │  │ (Random Forest)│  │ (Time Series)  │         │    │
│  │  └────────────────┘  └────────────────┘  └────────────────┘         │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                         DATA LAYER                                   │    │
│  │                                                                      │    │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌────────────┐  │    │
│  │  │ PostgreSQL   │ │ MongoDB      │ │ Redis        │ │ InfluxDB   │  │    │
│  │  │              │ │              │ │              │ │            │  │    │
│  │  │ Users        │ │ Route Data   │ │ Session      │ │ GPS Points │  │    │
│  │  │ Trips        │ │ AI Logs      │ │ Cache        │ │ Metrics    │  │    │
│  │  │ Payments     │ │ Waypoints    │ │ Real-time    │ │ Time-series│  │    │
│  │  │ Trucks       │ │ Notifications│ │ Tracking     │ │ IoT Data   │  │    │
│  │  └──────────────┘ └──────────────┘ └──────────────┘ └────────────┘  │    │
│  │                                                                      │    │
│  │  ┌──────────────┐ ┌──────────────┐                                  │    │
│  │  │ Apache Kafka │ │ S3 / MinIO   │                                  │    │
│  │  │              │ │              │                                  │    │
│  │  │ Event Stream │ │ File Storage │                                  │    │
│  │  │ Message Queue│ │ ML Models    │                                  │    │
│  │  └──────────────┘ └──────────────┘                                  │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                    EXTERNAL SERVICES LAYER                           │    │
│  │                                                                      │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │    │
│  │  │ Google   │ │ Razorpay │ │ Twilio   │ │ OpenWea- │ │ Firebase │  │    │
│  │  │ Maps API │ │ /Stripe  │ │ SMS      │ │ ther API │ │ FCM      │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 9. DEPLOYMENT DIAGRAM

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          DEPLOYMENT DIAGRAM                                   │
│                                                                              │
│  ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐       │
│  │  CLIENT DEVICE   │    │  DRIVER DEVICE   │    │  ADMIN DEVICE    │       │
│  │  (Browser)       │    │  (Android/iOS)   │    │  (Browser)       │       │
│  │                  │    │                  │    │                  │       │
│  │  React.js SPA    │    │  React Native    │    │  React.js SPA    │       │
│  └────────┬─────────┘    └────────┬─────────┘    └────────┬─────────┘       │
│           │                       │                       │                  │
│           │         HTTPS         │         HTTPS         │                  │
│           └───────────────────────┼───────────────────────┘                  │
│                                   │                                          │
│                                   ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        CLOUD INFRASTRUCTURE                          │    │
│  │                     (AWS / Azure / GCP / Docker)                     │    │
│  │                                                                      │    │
│  │  ┌───────────────────────────────────────────────────────────┐       │    │
│  │  │                    LOAD BALANCER (Nginx)                   │       │    │
│  │  └─────────────────────────┬─────────────────────────────────┘       │    │
│  │                            │                                         │    │
│  │  ┌─────────────────────────┴─────────────────────────────────┐       │    │
│  │  │                    API GATEWAY (Kong)                      │       │    │
│  │  └─────────────────────────┬─────────────────────────────────┘       │    │
│  │                            │                                         │    │
│  │  ┌─────────────────────────┴─────────────────────────────────┐       │    │
│  │  │              KUBERNETES CLUSTER                            │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐        │       │    │
│  │  │  │ Auth    │ │ Trip    │ │ Track   │ │ Payment │        │       │    │
│  │  │  │ Pod x2  │ │ Pod x3  │ │ Pod x3  │ │ Pod x2  │        │       │    │
│  │  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘        │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐        │       │    │
│  │  │  │ Demand  │ │ Fleet   │ │ Notif   │ │ Report  │        │       │    │
│  │  │  │ Pod x2  │ │ Pod x2  │ │ Pod x2  │ │ Pod x2  │        │       │    │
│  │  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘        │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌─────────┐ ┌─────────┐                                  │       │    │
│  │  │  │ Dispatch│ │ Delay   │                                  │       │    │
│  │  │  │ Pod x2  │ │ Pod x2  │                                  │       │    │
│  │  │  └─────────┘ └─────────┘                                  │       │    │
│  │  └────────────────────────────────────────────────────────────┘       │    │
│  │                                                                      │    │
│  │  ┌────────────────────────────────────────────────────────────┐       │    │
│  │  │              AI / ML SERVER (GPU Instance)                 │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │       │    │
│  │  │  │ TensorFlow   │  │ Scikit-Learn │  │ MLflow       │    │       │    │
│  │  │  │ Serving      │  │ Models       │  │ Model Reg    │    │       │    │
│  │  │  └──────────────┘  └──────────────┘  └──────────────┘    │       │    │
│  │  └────────────────────────────────────────────────────────────┘       │    │
│  │                                                                      │    │
│  │  ┌────────────────────────────────────────────────────────────┐       │    │
│  │  │              DATABASE CLUSTER                              │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │       │    │
│  │  │  │PostgreSQL│  │ MongoDB  │  │  Redis   │  │InfluxDB  │  │       │    │
│  │  │  │ Primary  │  │ Replica  │  │ Cluster  │  │ Cluster  │  │       │    │
│  │  │  │ + Replica│  │ Set      │  │          │  │          │  │       │    │
│  │  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │       │    │
│  │  │                                                            │       │    │
│  │  │  ┌──────────┐  ┌──────────┐                                │       │    │
│  │  │  │ Kafka    │  │ S3/MinIO │                                │       │    │
│  │  │  │ Cluster  │  │ Storage  │                                │       │    │
│  │  │  └──────────┘  └──────────┘                                │       │    │
│  │  └────────────────────────────────────────────────────────────┘       │    │
│  │                                                                      │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 10. AI/ML PIPELINE DATA FLOW

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        AI/ML PIPELINE DATA FLOW                              │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                     DATA COLLECTION LAYER                            │    │
│  │                                                                      │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐            │    │
│  │  │ GPS Data │  │ Trip     │  │ Weather  │  │ Traffic  │            │    │
│  │  │ (Real-   │  │ History  │  │ Data     │  │ Data     │            │    │
│  │  │  time)   │  │          │  │          │  │          │            │    │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘            │    │
│  └───────┼──────────────┼──────────────┼──────────────┼─────────────────┘    │
│          │              │              │              │                       │
│          └──────────────┼──────────────┼──────────────┘                       │
│                         │              │                                      │
│                         ▼              ▼                                      │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                     DATA PREPROCESSING                               │    │
│  │                                                                      │    │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │    │
│  │  │ Clean &      │  │ Feature      │  │ Normalize &  │               │    │
│  │  │ Validate     │──►│ Engineering  │──►│ Transform    │               │    │
│  │  │              │  │              │  │              │               │    │
│  │  │ - Remove     │  │ - Distance   │  │ - Min-Max    │               │    │
│  │  │   nulls      │  │ - Time of Day│  │ - One-Hot    │               │    │
│  │  │ - Fix format │  │ - Day of Week│  │ - Label Enc  │               │    │
│  │  │ - Outliers   │  │ - Rush Hour  │  │              │               │    │
│  │  │              │  │ - Weather    │  │              │               │    │
│  │  │              │  │   Score      │  │              │               │    │
│  │  └──────────────┘  └──────────────┘  └──────┬───────┘               │    │
│  └─────────────────────────────────────────────┼────────────────────────┘    │
│                                                │                             │
│                                                ▼                             │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                      MODEL TRAINING & SERVING                        │    │
│  │                                                                      │    │
│  │  ┌────────────────────────────────────────────────────────────┐      │    │
│  │  │                    TRAINING PIPELINE                        │      │    │
│  │  │                                                            │      │    │
│  │  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │      │    │
│  │  │  │ Split    │  │ Train    │  │ Validate │  │ Test     │  │      │    │
│  │  │  │ Data     │──►│ Model   │──►│ (Cross-  │──►│ (Hold-  │  │      │    │
│  │  │  │ 70/15/15 │  │          │  │  Valid)  │  │  out)    │  │      │    │
│  │  │  └──────────┘  └──────────┘  └──────────┘  └────┬─────┘  │      │    │
│  │  │                                                  │         │      │    │
│  │  └──────────────────────────────────────────────────┼─────────┘      │    │
│  │                                                     │                │    │
│  │  ┌──────────────────────────────────────────────────┼─────────┐      │    │
│  │  │                   MODEL REGISTRY (MLflow)        │         │      │    │
│  │  │                                                  ▼         │      │    │
│  │  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │      │    │
│  │  │  │ ETA Model    │  │ Route Model  │  │ Pricing      │    │      │    │
│  │  │  │ v2.3         │  │ v1.8         │  │ Model v3.1   │    │      │    │
│  │  │  │ Acc: 94.2%   │  │ Acc: 91.7%   │  │ Acc: 89.5%   │    │      │    │
│  │  │  └──────────────┘  └──────────────┘  └──────────────┘    │      │    │
│  │  │                                                           │      │    │
│  │  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │      │    │
│  │  │  │ Delay Model  │  │ Driver Score │  │ Demand       │    │      │    │
│  │  │  │ v2.1         │  │ Model v1.5   │  │ Forecast v1.2│    │      │    │
│  │  │  │ Acc: 92.8%   │  │ Acc: 88.3%   │  │ Acc: 86.1%   │    │      │    │
│  │  │  └──────────────┘  └──────────────┘  └──────────────┘    │      │    │
│  │  └───────────────────────────────────────────────────────────┘      │    │
│  │                                    │                                │    │
│  │                                    ▼                                │    │
│  │  ┌───────────────────────────────────────────────────────────┐      │    │
│  │  │              MODEL SERVING (TensorFlow Serving / FastAPI)  │      │    │
│  │  │                                                           │      │    │
│  │  │  Input: Trip Data ──► Model Inference ──► Prediction      │      │    │
│  │  │                                                           │      │    │
│  │  └───────────────────────────────────────────────────────────┘      │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                         │
│                                    ▼                                         │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                      FEEDBACK LOOP (Continuous Learning)             │    │
│  │                                                                      │    │
│  │  Actual ETA ──► Compare with Prediction ──► Calculate Error          │    │
│  │       │                                           │                  │    │
│  │       └──────────────► Retrain Model ◄────────────┘                  │    │
│  │                         (Weekly / When accuracy drops)               │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 11. API ENDPOINT DESIGN

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          API ENDPOINT STRUCTURE                               │
│                                                                              │
│  BASE URL: https://api.logistics-ai.com/v1                                  │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  AUTH SERVICE                                                                │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /auth/register          → Register new user                         │
│  POST   /auth/login             → Login (returns JWT)                       │
│  POST   /auth/refresh           → Refresh token                            │
│  POST   /auth/logout            → Invalidate token                         │
│  PUT    /auth/password          → Change password                          │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  DEMAND SERVICE                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /demands                → Create new demand                         │
│  GET    /demands                → List demands (filtered)                   │
│  GET    /demands/{id}           → Get demand details                        │
│  PUT    /demands/{id}           → Update demand                             │
│  DELETE /demands/{id}           → Cancel demand                             │
│  GET    /demands/{id}/slots     → Get available time slots                  │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  TRIP SERVICE                                                                │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /trips                  → Create trip from demand                   │
│  GET    /trips                  → List all trips                            │
│  GET    /trips/{id}             → Get trip details                          │
│  PUT    /trips/{id}/status      → Update trip status                        │
│  GET    /trips/{id}/tracking    → Get live tracking data                    │
│  GET    /trips/{id}/eta         → Get AI-predicted ETA                      │
│  GET    /trips/{id}/route       → Get optimized route                       │
│  GET    /trips/{id}/delays      → Get delay history                         │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  DRIVER SERVICE                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  GET    /drivers                → List all drivers                          │
│  GET    /drivers/{id}           → Get driver profile                        │
│  PUT    /drivers/{id}           → Update driver info                        │
│  GET    /drivers/{id}/trips     → Get driver's trips                        │
│  PUT    /drivers/{id}/location  → Update driver location                    │
│  GET    /drivers/{id}/score     → Get AI performance score                  │
│  PUT    /drivers/{id}/availability → Set availability                       │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  FLEET SERVICE                                                               │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /trucks                 → Add new truck                             │
│  GET    /trucks                 → List all trucks                           │
│  GET    /trucks/{id}            → Get truck details                         │
│  PUT    /trucks/{id}            → Update truck info                         │
│  DELETE /trucks/{id}            → Remove truck                              │
│  GET    /trucks/{id}/location   → Get truck location                        │
│  GET    /trucks/available       → Get available trucks                      │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  TRACKING SERVICE (WebSocket)                                                │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  WS     /tracking/live/{tripId} → Real-time location stream                │
│  POST   /tracking/update        → Push GPS coordinates                      │
│  GET    /tracking/history/{id}  → Get tracking history                      │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  PAYMENT SERVICE                                                             │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /payments               → Process payment                           │
│  GET    /payments/{id}          → Get payment details                       │
│  GET    /payments/trip/{tripId} → Get payment for trip                      │
│  POST   /payments/{id}/refund   → Process refund                            │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  AI SERVICE                                                                  │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  POST   /ai/predict-eta         → Predict delivery ETA                      │
│  POST   /ai/optimize-route      → Get optimized route                       │
│  POST   /ai/dynamic-price       → Calculate dynamic price                   │
│  POST   /ai/detect-delay        → Check for potential delay                 │
│  GET    /ai/driver-score/{id}   → Get driver AI score                       │
│  POST   /ai/demand-forecast     → Forecast future demand                    │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  REPORT SERVICE                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  GET    /reports/dashboard       → Get dashboard data                        │
│  GET    /reports/trips           → Trip analytics report                     │
│  GET    /reports/drivers         → Driver performance report                 │
│  GET    /reports/revenue         → Revenue report                           │
│  GET    /reports/delays          → Delay analysis report                    │
│  GET    /reports/export/{type}   → Export report (PDF/Excel)                │
│                                                                              │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  NOTIFICATION SERVICE                                                        │
│  ═══════════════════════════════════════════════════════════════════════════  │
│  GET    /notifications           → Get user notifications                    │
│  PUT    /notifications/{id}/read → Mark as read                             │
│  POST   /notifications/subscribe → Subscribe to push                        │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 12. DATABASE SCHEMA (SQL)

```sql
-- =====================================================
-- DATABASE SCHEMA FOR AI LOGISTICS MANAGEMENT SYSTEM
-- =====================================================

-- ENUM TYPES
CREATE TYPE user_role AS ENUM ('ADMIN', 'CLIENT', 'DRIVER');
CREATE TYPE trip_status AS ENUM ('CREATED', 'PENDING', 'ASSIGNED', 'ACCEPTED', 
                                  'IN_TRANSIT', 'DELAYED', 'ON_HOLD', 
                                  'DELIVERED', 'COMPLETED', 'CANCELLED');
CREATE TYPE demand_source AS ENUM ('MANUAL', 'SENSOR', 'AUTOMATED');
CREATE TYPE slot_type AS ENUM ('MORNING', 'AFTERNOON', 'EVENING', 'URGENT');
CREATE TYPE payment_type AS ENUM ('DRIVER_ONLY', 'DRIVER_TRUCK');
CREATE TYPE payment_status AS ENUM ('PENDING', 'PROCESSED', 'FAILED', 'REFUNDED');

-- USERS TABLE
CREATE TABLE users (
    user_id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name            VARCHAR(100) NOT NULL,
    email           VARCHAR(150) UNIQUE NOT NULL,
    password_hash   VARCHAR(255) NOT NULL,
    phone           VARCHAR(20),
    role            user_role NOT NULL,
    is_active       BOOLEAN DEFAULT true,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DRIVER PROFILES TABLE
CREATE TABLE driver_profiles (
    profile_id       UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id          UUID REFERENCES users(user_id) ON DELETE CASCADE,
    license_number   VARCHAR(50) NOT NULL,
    license_expiry   DATE NOT NULL,
    experience_years INTEGER DEFAULT 0,
    rating           DECIMAL(3,2) DEFAULT 0.00,
    total_trips      INTEGER DEFAULT 0,
    performance_score DECIMAL(5,2) DEFAULT 0.00,
    current_lat      DECIMAL(10,7),
    current_lng      DECIMAL(10,7),
    is_available     BOOLEAN DEFAULT true,
    UNIQUE(user_id)
);

-- TRUCKS TABLE
CREATE TABLE trucks (
    truck_id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    reg_number       VARCHAR(20) UNIQUE NOT NULL,
    capacity_tons    DECIMAL(10,2) NOT NULL,
    truck_type       VARCHAR(50),
    status           VARCHAR(20) DEFAULT 'AVAILABLE',
    current_lat      DECIMAL(10,7),
    current_lng      DECIMAL(10,7),
    fuel_type        VARCHAR(20),
    mileage          DECIMAL(10,2),
    last_service     DATE,
    owner_id         UUID REFERENCES users(user_id),
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DEMANDS TABLE
CREATE TABLE demands (
    demand_id        UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    client_id        UUID REFERENCES users(user_id),
    source_type      demand_source NOT NULL,
    origin_address   TEXT NOT NULL,
    origin_lat       DECIMAL(10,7),
    origin_lng       DECIMAL(10,7),
    dest_address     TEXT NOT NULL,
    dest_lat         DECIMAL(10,7),
    dest_lng         DECIMAL(10,7),
    cargo_type       VARCHAR(100),
    weight_tons      DECIMAL(10,2),
    priority         INTEGER DEFAULT 1,
    status           VARCHAR(20) DEFAULT 'NEW',
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- TIME SLOTS TABLE
CREATE TABLE time_slots (
    slot_id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    demand_id        UUID REFERENCES demands(demand_id),
    slot_type        slot_type NOT NULL,
    start_time       TIMESTAMP NOT NULL,
    end_time         TIMESTAMP NOT NULL,
    price_multiplier DECIMAL(4,2) DEFAULT 1.00,
    is_available     BOOLEAN DEFAULT true
);

-- TRIPS TABLE
CREATE TABLE trips (
    trip_id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    demand_id        UUID REFERENCES demands(demand_id),
    client_id        UUID REFERENCES users(user_id),
    driver_id        UUID REFERENCES users(user_id),
    truck_id         UUID REFERENCES trucks(truck_id),
    slot_id          UUID REFERENCES time_slots(slot_id),
    origin_address   TEXT NOT NULL,
    origin_lat       DECIMAL(10,7),
    origin_lng       DECIMAL(10,7),
    dest_address     TEXT NOT NULL,
    dest_lat         DECIMAL(10,7),
    dest_lng         DECIMAL(10,7),
    distance_km      DECIMAL(10,2),
    base_price       DECIMAL(12,2),
    final_price      DECIMAL(12,2),
    status           trip_status DEFAULT 'CREATED',
    planned_eta      TIMESTAMP,
    actual_eta       TIMESTAMP,
    started_at       TIMESTAMP,
    completed_at     TIMESTAMP,
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- ROUTES TABLE
CREATE TABLE routes (
    route_id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    trip_id          UUID REFERENCES trips(trip_id),
    waypoints        JSONB,
    total_distance   DECIMAL(10,2),
    optimized_path   JSONB,
    traffic_data     JSONB,
    weather_data     JSONB,
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- TRACKING TABLE
CREATE TABLE tracking_points (
    tracking_id      UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    trip_id          UUID REFERENCES trips(trip_id),
    latitude         DECIMAL(10,7) NOT NULL,
    longitude        DECIMAL(10,7) NOT NULL,
    speed_kmh        DECIMAL(6,2),
    heading          DECIMAL(5,2),
    status           VARCHAR(30),
    recorded_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- DELAYS TABLE
CREATE TABLE delays (
    delay_id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    trip_id          UUID REFERENCES trips(trip_id),
    delay_minutes    INTEGER NOT NULL,
    reason           TEXT,
    detected_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    resolved_at      TIMESTAMP,
    ai_prediction    JSONB
);

-- PAYMENTS TABLE
CREATE TABLE payments (
    payment_id       UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    trip_id          UUID REFERENCES trips(trip_id),
    amount           DECIMAL(12,2) NOT NULL,
    payment_type     payment_type NOT NULL,
    status           payment_status DEFAULT 'PENDING',
    transaction_id   VARCHAR(100),
    processed_at     TIMESTAMP,
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- NOTIFICATIONS TABLE
CREATE TABLE notifications (
    notification_id  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id          UUID REFERENCES users(user_id),
    type             VARCHAR(50) NOT NULL,
    title            VARCHAR(200),
    message          TEXT NOT NULL,
    is_read          BOOLEAN DEFAULT false,
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- AI LOGS TABLE
CREATE TABLE ai_logs (
    log_id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    trip_id          UUID REFERENCES trips(trip_id),
    model_type       VARCHAR(50) NOT NULL,
    input_data       JSONB,
    prediction       JSONB,
    actual_value     JSONB,
    accuracy         DECIMAL(5,2),
    created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- INDEXES
CREATE INDEX idx_trips_client ON trips(client_id);
CREATE INDEX idx_trips_driver ON trips(driver_id);
CREATE INDEX idx_trips_status ON trips(status);
CREATE INDEX idx_tracking_trip ON tracking_points(trip_id);
CREATE INDEX idx_tracking_time ON tracking_points(recorded_at);
CREATE INDEX idx_demands_client ON demands(client_id);
CREATE INDEX idx_delays_trip ON delays(trip_id);
CREATE INDEX idx_payments_trip ON payments(trip_id);
CREATE INDEX idx_notifications_user ON notifications(user_id);
```

---

## 13. TECHNOLOGY STACK SUMMARY

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          TECHNOLOGY STACK                                     │
│                                                                              │
│  LAYER              │  TECHNOLOGY                │  PURPOSE                   │
│  ═══════════════════╪════════════════════════════╪═══════════════════════════ │
│  Frontend (Web)     │  React.js + TypeScript     │  Admin & Client Dashboard │
│  Frontend (Mobile)  │  React Native              │  Driver Mobile App        │
│  Visualization      │  Chart.js / D3.js / Power BI│ Reports & Analytics      │
│  ───────────────────┼────────────────────────────┼─────────────────────────── │
│  API Gateway        │  Kong / Nginx              │  Routing, Rate Limiting   │
│  Backend Framework  │  Node.js (Express/NestJS)  │  Microservices            │
│  Real-time          │  Socket.io / WebSocket     │  Live Tracking            │
│  Message Queue      │  Apache Kafka / RabbitMQ   │  Event Streaming          │
│  ───────────────────┼────────────────────────────┼─────────────────────────── │
│  Primary DB         │  PostgreSQL                │  Relational Data          │
│  Document DB        │  MongoDB                   │  Routes, Logs, Configs    │
│  Cache              │  Redis                     │  Sessions, Real-time      │
│  Time-Series DB     │  InfluxDB                  │  GPS, Metrics             │
│  Object Storage     │  AWS S3 / MinIO            │  Files, ML Models         │
│  ───────────────────┼────────────────────────────┼─────────────────────────── │
│  AI/ML Framework    │  Python (TensorFlow/PyTorch)│ Model Training           │
│  ML Libraries       │  Scikit-Learn, XGBoost     │  Classical ML             │
│  Model Serving      │  TensorFlow Serving/FastAPI│  Inference API            │
│  ML Ops             │  MLflow                    │  Model Registry           │
│  ───────────────────┼────────────────────────────┼─────────────────────────── │
│  Maps & Routing     │  Google Maps API           │  Distance, Geocoding      │
│  Payment            │  Razorpay / Stripe         │  Payment Processing       │
│  Notifications      │  Firebase FCM / Twilio     │  Push, SMS, Email         │
│  Weather            │  OpenWeather API           │  Weather Data             │
│  ───────────────────┼────────────────────────────┼─────────────────────────── │
│  Containerization   │  Docker                    │  App Containers           │
│  Orchestration      │  Kubernetes                │  Container Management     │
│  CI/CD              │  GitHub Actions / Jenkins  │  Automated Deploy         │
│  Monitoring         │  Prometheus + Grafana      │  System Monitoring        │
│  Logging            │  ELK Stack                 │  Centralized Logging      │
│  Cloud              │  AWS / Azure / GCP         │  Infrastructure           │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## DOCUMENT SUMMARY

This Document 1 contains:

| # | Diagram/Design | Status |
|---|---|---|
| 1 | High-Level System Architecture | ✅ Complete |
| 2 | DFD Level 0 (Context Diagram) | ✅ Complete |
| 3 | DFD Level 1 (Main Processes) | ✅ Complete |
| 4 | DFD Level 2 (AI Trip Planning) | ✅ Complete |
| 5 | DFD Level 2 (Delay Detection) | ✅ Complete |
| 6 | Entity-Relationship Diagram | ✅ Complete |
| 7 | Use Case Diagram | ✅ Complete |
| 8 | Sequence Diagram - Trip Booking | ✅ Complete |
| 9 | Sequence Diagram - Dispatch & Delivery | ✅ Complete |
| 10 | Sequence Diagram - Payment | ✅ Complete |
| 11 | Activity/Flow Diagram (Full Lifecycle) | ✅ Complete |
| 12 | State Diagram (Trip States) | ✅ Complete |
| 13 | Component Diagram | ✅ Complete |
| 14 | Deployment Diagram | ✅ Complete |
| 15 | AI/ML Pipeline Data Flow | ✅ Complete |
| 16 | API Endpoint Design | ✅ Complete |
| 17 | Database Schema (SQL) | ✅ Complete |
| 18 | Technology Stack Summary | ✅ Complete |

---
