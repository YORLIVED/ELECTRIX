# RESQLINK
## Disaster Family Reunification & Victim Identification System

> **“During a disaster, people may move from rescue zones to camps and hospitals, but their identity should never be lost.”**

ResQLink is an offline-first, full-stack disaster-response platform built to assist rescue units, relief camp administrators, emergency hospital surgeons, disaster management authorities, and separated families in identifying victims, maintaining an immutable chain-of-custody, and facilitating verified family reunifications.

---

## 🛡️ Core Architectural Principle

```
ONE PERSON ➔ ONE PERMANENT PERSON ID (e.g. DR-2026-000458) ➔ ONE QR CODE ➔ MULTIPLE LOCATIONS ➔ ONE SYNCHRONIZED RECORD ➔ VERIFIED FAMILY REUNIFICATION
```

- **A person NEVER receives a new identity** when transferred from Rescue Zone ➔ Camp A ➔ Hospital B ➔ Camp C.
- **QR Code = Pure Identifier**: Stores ONLY the Person ID or secure token. It does **NOT** expose personal or clinical data on the physical band.
- **GPS = Operation Timestamp**: Records where the authorized officer executed an operation (registration, scan, transfer, verification). GPS does **NOT** continuously track the victim.
- **Offline-First PWA**: Operates autonomously in zero-connectivity disaster zones using IndexedDB. Pending operations are queued and automatically synchronized when connectivity returns.
- **AI-Assisted Matching with Ethical Oversight (Rule 9)**: Probabilistically correlates missing reports with rescued victims based on scars, clothing, age, and rescue location, while strictly requiring legal officer verification before confirming identity.

---

## ⚡ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 18, TypeScript, Vite, Tailwind CSS, Lucide Icons |
| **Offline Storage** | IndexedDB (Dexie.js), Progressive Web App (Service Worker, Manifest) |
| **Hardware APIs** | HTML5 Camera Streaming, Geolocation API, QR Code Canvas Engine |
| **Backend** | Node.js, Express, TypeScript, JWT, Multer (Photo uploads) |
| **Database & ORM** | Relational Database (SQLite default for zero-setup local dev / PostgreSQL production-ready), Prisma ORM |

---

## 🚀 Quick Start Guide

### Prerequisites
- Node.js (v20+ LTS recommended)
- NPM (v10+)

### 1. Backend Setup
```bash
cd resqlink/backend

# Install dependencies
npm install

# Initialize Prisma SQLite database and push schema
npm run db:push

# Seed database with realistic demo data (Kumar Family, Camps, Hospitals, AI Matches)
npm run db:seed

# Start backend server (runs on http://localhost:5000)
npm run dev
```

### 2. Frontend Setup
```bash
cd resqlink/frontend

# Install dependencies
npm install

# Start Vite dev server (runs on http://localhost:5173)
npm run dev
```

Visit **`http://localhost:5173`** in your browser.

---

## 🔑 Demo Field Profiles & Credentials

Password for all pre-seeded accounts: **`resqlink2026`**

| Role | Username | Purpose |
|---|---|---|
| **Rescue Officer** | `rahul_rescue` | Field extrication, disaster zone GPS logging |
| **Camp Officer** | `camp_officer` | Camp A shelter intake, wristband printing |
| **Hospital Officer** | `dr_sharma` | Metro Trauma Center inpatient triage & surgery updates |
| **Disaster Authority** | `authority_cmd` | Disaster Command Center, AI match verification |
| **Family User** | `family_kumar` | "Find My Family" portal & missing person filing |
| **System Admin** | `admin` | Full system audit trail and configuration |

*Note: You can also click the quick **Role Switcher** in the top navigation bar or the 1-click profiles on the login page.*

---

## 🎯 24-Step Hackathon Demonstration Script (Section 36)

ResQLink includes an interactive **Hackathon Demo Guide bar** at the top of the interface:

1. **Register Arun Kumar**: Go to `/register` or click "Pre-fill Arun Kumar (Demo)".
2. **Victim Photo**: Upload or capture camera photo.
3. **Physical Identifiers**: Age: 22, Scars: *Small scar near left eyebrow*, Moles: *Small mole on right cheek*, Identification: *Birthmark on right hand*.
4. **Permanent ID Generated**: `DR-2026-000458`.
5. **QR Code Generated**: Token linked to `DR-2026-000458`.
6. **Rescue Operation**: Initial status set to `RESCUED` at Rescue Zone A.
7. **QR Checkpoint Scan**: Go to `/scan-qr` and click "Scan Arun Kumar".
8. **GPS Location Captured**: Device coordinates (e.g. `13.0827, 80.2707`) recorded.
9. **Status Logged**: Status confirmed as `RESCUED`.
10. **Camp A Registration**: Arun checked in at Camp A (North Shelter).
11. **Hospital Checkpoint**: Later scanned at Hospital B.
12. **Transfer Updated**: Destination set to Hospital B, status set to `UNDER TREATMENT`.
13. **Movement Trajectory**: View profile at `/persons/DR-2026-000458` showing:
    ```
    Rescue Zone A ➔ Camp A ➔ Hospital B
    ```
14. **Disconnect Internet**: Click **"Simulate Cut Off"** in the top navigation bar.
15. **Offline Operation**: Update record or register an intake in offline mode.
16. **Offline GPS Capture**: Device captures location and timestamp locally.
17. **Offline Banner**: Interface shows:
    ```
    OFFLINE • Saved Locally • Pending Sync: 1 record
    ```
18. **Restore Connectivity**: Click **"Simulate Cut Off"** again to reconnect.
19. **Auto-Synchronization**: Status changes to **SYNCING...**.
20. **Server Upload**: Queue items batch-upload to central backend.
21. **Sync Confirmed**: Status updates to **✓ SYNCED**.
22. **Family Search**: Switch role to Family User and go to `/find-family`. Search `"Arun"` or `"DR-2026-000458"`.
23. **Family Result Card**: Displays Arun Kumar, Age 22, Photo, Under Treatment, Current Location: Hospital B.
24. **Official Verification**: Verified badge shows that record was legally confirmed by Commander Rajesh Varma.

---

## 📡 REST API Reference

### Authentication
- `POST /api/auth/register` - Create user
- `POST /api/auth/login` - Authenticate with JWT
- `GET /api/auth/me` - Current session profile
- `GET /api/auth/demo-users` - Demo accounts list

### Victim Management
- `GET /api/persons` - Filter persons by status, location, family
- `GET /api/persons/:id` - Complete profile with full movement timeline
- `POST /api/persons` - Register new victim with GPS & QR generation
- `PUT /api/persons/:id` - Update status or facility with movement logging
- `POST /api/persons/:id/verify` - Legal identity verification

### QR Code & Wristbands
- `POST /api/qr/generate` - Generate secure ID token
- `POST /api/qr/scan` - Checkpoint scan resolving to cloud or local DB

### Movements & Transfers
- `GET /api/movement/person/:personId` - Chronological trajectory
- `POST /api/movement` - Log relocation event
- `GET /api/transfers` - Recent inter-facility transfers
- `POST /api/transfers` - Confirm transfer between camp and hospital

### Missing Persons & AI Matcher
- `GET /api/missing` - Query missing reports
- `POST /api/missing` - File missing person report
- `POST /api/matches/search` - Run AI similarity engine
- `GET /api/matches` - Active candidate matches
- `PUT /api/matches/:id/verify` - Officer confirms or rejects match

### Family Reunification
- `GET /api/families/search?q=:query` - Search by name, person ID, family ID
- `GET /api/families/:id` - Detailed family unit
- `POST /api/families` - Create family unit

### Offline Synchronization
- `POST /api/sync/push` - Client pushes offline IndexedDB queue batch
- `GET /api/sync/pull` - Client pulls latest cloud delta
- `GET /api/sync/status` - Sync server health

### Command Dashboards
- `GET /api/dashboard/authority` - Disaster command center metrics
- `GET /api/dashboard/camp/:campId` - Camp occupancy & intake
- `GET /api/dashboard/hospital/:hospitalId` - Triage urgency & inpatient beds

---

## 📂 System Architecture Diagram

```
                 [ SEPARATED FAMILIES ]
                           |
                      ( Web / PWA )
                           |
     +---------------------+---------------------+
     |                     |                     |
 [ CAMP A ]           [ CAMP B ]          [ HOSPITAL B ]
     |                     |                     |
 (IndexedDB)          (IndexedDB)           (IndexedDB)
     |                     |                     |
     +------ Local Offline Sync Queue -----------+
                           |
                 ( Internet Restored )
                           |
               [ RESQLINK BACKEND API ]
                           |
            [ PRISMA CENTRAL RELATIONAL DB ]
                           |
     +---------------------+---------------------+
     |                     |                     |
[ QR Scanner ]       [ GPS Auditor ]     [ AI Match Engine ]
```

---

## 📜 License
Developed for Emergency Disaster Response & Humanitarian Reunification. Open for humanitarian and disaster-relief deployments.
