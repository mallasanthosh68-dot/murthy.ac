# Murthy AC Mechanics & Engineering — AC Service & Booking Platform

[![Next.js](https://img.shields.io/badge/Next.js-16.3-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![React](https://img.shields.io/badge/React-19-blue?style=flat-square&logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind-v4-38bdf8?style=flat-square&logo=tailwindcss)](https://tailwindcss.com/)
[![Prisma](https://img.shields.io/badge/Prisma-6-2d3748?style=flat-square&logo=prisma)](https://www.prisma.io/)
[![Status](https://img.shields.io/badge/Tests-38%20Passed-emerald?style=flat-square)]()

A complete, production-ready, full-stack AC Service, Repair, Maintenance, and New AC Consultation booking web platform designed for **Murthy AC Mechanics & Engineering**. Features a progressive booking wizard, dual-channel admin approval (WhatsApp & Admin Portal), role-based authentication, and an administrative command center.

---

## 🔑 Quick Access & Credentials

| Role | Access URL | Identifier / Username | Password | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Admin** | [http://localhost:3000/admin/login](http://localhost:3000/admin/login) | `murthi` *(or `murthi@ac.in`)* | `murthy@ac.in` | Review bookings, approve slots, manage technicians, calendar & leads |
| **Customer** | [http://localhost:3000/login](http://localhost:3000/login) | `9876543210` | `Customer@123` | Track appointments, view service history & manage profile |

> **Official Admin WhatsApp Number**: **`8985817969`** (`+91 89858 17969`)  
> **Admin Bottom Tile**: A dedicated tile is located in the website footer on every page for 1-click admin login.

---

## ⚡ Core Operational Workflow

```
Customer visits website
        │
        ▼
Fills Progressive Booking Wizard (/book-service)
(Selects Service, Property, AC count, Problem Description, Date & Time Slot)
        │
        ▼
Submits Booking on Website
        │
        ├──▶ 1. Saved to Database with status: PENDING (queued for Admin review)
        └──▶ 2. Dispatched to Admin WhatsApp (+91 89858 17969)
                (Includes Customer Name, Phone, Problem Description, & 1-Click Approve Link)
        │
        ▼
Admin Review & Approval (Supported via Dual Channels)
        │
        ├── Option A: 1-Click Approve directly inside WhatsApp Link
        │             (Taps http://localhost:3000/api/bookings/<id>/quick-approve -> status becomes CONFIRMED)
        │
        └── Option B: Approve inside Admin Portal (/admin/bookings)
                      (Click "✓ Approve" or "Custom" to assign specific mechanics)
```

---

## 🚀 Quick Start & Local Setup

### 1. Prerequisites
- **Node.js**: v18.0.0 or higher
- **Package Manager**: `npm` or `pnpm`

### 2. Installation
```bash
git clone <repository-url>
cd murthy
npm install
```

### 3. Environment Setup
The project includes a pre-configured `.env` file for local development:
```env
DATABASE_URL="file:./dev.db"
AUTH_SECRET="coolbreeze-ac-service-secret-key-change-in-production-2026"
NEXT_PUBLIC_SITE_URL="http://localhost:3000"
NEXT_PUBLIC_BUSINESS_NAME="Murthy AC Mechanics & Engineering"
NEXT_PUBLIC_BUSINESS_PHONE="+91 89858 17969"
NEXT_PUBLIC_WHATSAPP_NUMBER="918985817969"
WHATSAPP_NUMBER="918985817969"
```

### 4. Database Initialization & Seeding
```bash
# Push schema changes to SQLite dev database
npx prisma db push

# Seed Admin (murthi), Demo Customer, Services, and Time Slots
npx tsx prisma/seed.ts
```

### 5. Run Automated Tests
```bash
npm run test
```
*Executes all 38 automated test cases covering authentication, OTP lifecycle, bookings, admin credentials, and WhatsApp formatting.*

### 6. Start Server
```bash
# Development mode:
npm run dev

# Production build and run:
npm run build
npm run start
```
Visit **[http://localhost:3000](http://localhost:3000)** in your browser.

---

- **Customer Portal (`/customer/portal`)**:
  - Dedicated Customer Menu Bar with **ONLY 4 Items**:
    1. **Book Service**: 3-step progressive booking wizard directly embedded inside the customer portal.
    2. **History**: Comprehensive past service and repair booking records with status badges and detail views.
    3. **Confirmation**: Live Admin Approval & Confirmation Box displaying real-time approval status ("Murthi Garu (Admin) has officially approved your AC service appointment!"), assigned technician, slot, address, and WhatsApp contact.
    4. **Settings**: Customer profile management, doorstep service address (house, street, area, landmark, pincode), and password security.
- **Admin Command Center**:
  - `/admin/login` — Isolated administrator authentication (`murthi` / `murthy@ac.in`).
  - `/admin/dashboard` — Live operational overview and KPI metrics.
  - `/admin/bookings` — Booking management with 1-click Quick-Approve, rejection, and technician assignment.
  - `/admin/calendar` — Visual slot capacity meter and booking blackout manager.
  - `/admin/customers` — Registered customer accounts and direct WhatsApp links.
  - `/admin/services` — Service catalog management and pricing controls.
  - `/admin/new-ac-requests` — New AC consultation pipeline.
  - `/admin/settings` — Business profile, contact numbers, and service areas.

---

## 🛠 Tech Stack Details

| Layer | Technologies Used |
| :--- | :--- |
| **Frontend** | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS v4, Lucide Icons |
| **Backend** | Next.js Server Actions & API Route Handlers |
| **Database** | SQLite (Dev) / PostgreSQL (Prod) with Prisma ORM 6 |
| **Security** | `bcryptjs` password hashing, `jose` signed JWTs with HTTP-only secure cookies |
| **Integrations** | WhatsApp Web/App Link Engine + Provider Abstraction, OTP SMS Provider Abstraction |
| **SEO** | Sitemap (`/sitemap.xml`), Robots (`/robots.txt`), OpenGraph meta, LocalBusiness Schema |

---

## 🧪 Verification & Health Check

Run the comprehensive end-to-end check anytime:
```bash
node scratch/test-e2e.mjs
```
Expected output:
```
--- 1. Testing Admin Login with username murthi & password murthy@ac.in ---
Login Status: 200 Admin: Murthi (Chief AC Technician & Admin)

--- 2. Submitting Customer AC Booking on Website ---
Booking Number: BK-202610-XXXX
Initial Status: PENDING (Must be PENDING)
WhatsApp Target URL has 918985817969: true
WhatsApp Message includes problem: true
WhatsApp Message includes 1-Click Approve: true

--- 3. Testing WhatsApp 1-Click Quick-Approve Link ---
Approve Status: 200
Approve Message: Booking has been approved via WhatsApp.
Updated Status: CONFIRMED (Must be CONFIRMED)

✅ All End-to-End Verifications Succeeded!
```
