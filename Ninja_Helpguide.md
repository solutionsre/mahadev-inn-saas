# 🏨 Mahadev Inn — SaaS Hotel Booking System

A **multi-tenant, multi-branch SaaS booking platform** built with:

- **Backend** → NestJS + Prisma + PostgreSQL  
- **Frontend** → Next.js + TailwindCSS  
- **Auth** → JWT with Role-Based Access Control (RBAC)  
- **Export** → PDF & Excel (jsPDF, XLSX)  

---

## 1️⃣ Main Files (Pointwise)

👉 Think of the repo in **3 layers:**

### 🖥️ Backend (NestJS + Prisma)

- `server/prisma/schema.prisma` → Core DB definition (tenants, branches, users, bookings, room inventory).  
  Every feature (branch isolation, occupancy, bookingNo) must align here.  
- `server/prisma/seed.ts` → Seeds demo tenant, branches, and users (owner, manager, viewer). Essential for first run.  
- `server/src/prisma/prisma.service.ts` → Shared Prisma client for DB access.  
- `server/src/auth/*` → Login, JWT issue/validation, role injection into requests.  
- `server/src/users/users.service.ts` → Fetch users, needed by auth.  
- `server/src/bookings/*` → **Business heart**: create bookings, generate bookingNo, enforce branch/role rules, calculate occupancy per day.  
- `server/src/inventory/*` → Owner defines per-branch room capacity; used for occupancy % calculation.  
- `server/src/app.module.ts` & `server/src/main.ts` → Bootstraps NestJS app, configures modules, starts server.  

### 💻 Frontend (Next.js + Tailwind)

- `client/src/lib/api.ts` → Axios wrapper with JWT injection.  
- `client/src/lib/auth.tsx` → Handles login/logout, user context.  
- `client/src/app/login/page.tsx` → Login page.  
- `client/src/app/(dashboard)/page.tsx` → Dashboard: new bookings, today’s bookings, PDF printing.  
- `client/src/app/(dashboard)/calendar/page.tsx` → Calendar: occupancy grid, popup per day.  
- `client/src/app/(dashboard)/inventory/page.tsx` → Inventory editor (**Owner only**).  
- `client/src/app/layout.tsx` → Wraps everything in `AuthProvider`.  

---

## 2️⃣ Business Logic

### 🏨 Multi-Tenant & Branches
- Each **hotel = Tenant**.  
- Each **tenant can have multiple branches** (e.g., Kathmandu-1, Pokhara).  

### 👥 Users & Roles
- **Owner** → Sees all branches, can edit anything.  
- **Manager** → Restricted to one branch, can CRUD.  
- **Viewer** → Restricted to one branch, can view only.  

### 📑 Booking Flow
- User enters booking → backend auto-generates  
  `BookingNo = BranchName-YYYYMMDDHHMMSS-UID`.  
- Check-in/out validated (`nights >= 1`), contact must be numeric.  
- Booking stored with tenant + branch enforced by server.  
- `bookingstat` defaults to **Confirmed** but can be updated.  

### 📊 Occupancy Flow
- Per-day occupancy:  
- occupancy% = (rooms booked / branch capacity) * 100
- Uses branch inventory (single, double, triple, quard)
- **Owner** can see **all branches combined**.  

### 🛏️ Inventory Flow
- **Owner sets branch capacities** → drives occupancy % calculation.  

---

## 3️⃣ Business Advantages (SaaS)

### For Hotels / Chains
- ✅ Centralized booking + occupancy tracking across multiple branches.  
- ✅ Role-based access (owners, branch managers, staff).  
- ✅ Automated `BookingNo` & PDF confirmation for professionalism.  
- ✅ Occupancy analytics → maximize room utilization.  
- ✅ Cloud-based (multi-tenant) → accessible anywhere, anytime.  

### For SaaS Provider
- ✅ Single codebase → serve unlimited hotels (multi-tenant isolation).  
- ✅ Easy onboarding → just create new tenant + branches.  
- ✅ Scalable pricing → per tenant, per branch, or per booking.  
- ✅ Value-add upsells → analytics dashboards, channel manager integrations, payment gateways.  

---

## 4️⃣ Consistency (Master Prompt to Lock Scope)

⚠️ To **regenerate or extend** this system without drifting, always reuse this **master prompt**:
```
We are building a Hotel SaaS Booking System (multi-tenant, multi-branch).
Each hotel = Tenant, each Tenant has multiple Branches (Kathmandu-1, Pokhara, etc.).
Users = Owner, Manager, Viewer.

Owner → can see all branches of their tenant, can CRUD everything, can edit inventory.
Manager → can CRUD only their branch.
Viewer → can only view their branch.

Core requirements (never change):

Bookings stored branch-wise, enforced by server via JWT.

BookingNo = BranchName-YYYYMMDDHHMMSS-UID.

Each booking has bookingstat (default Confirmed).

After new booking → downloadable PDF confirmation.

Occupancy calculated daily = rooms booked / branch capacity.

Owner can see occupancy of all branches combined.

Branch capacity stored in RoomInventory (single,double,triple,quard).

Prisma schema, API, and Frontend must stay aligned.

Client = Next.js + Tailwind; Server = NestJS + Prisma.

Provide full repo layout (server, client, prisma, seed, main modules).

Never change this logic unless explicitly told. Always regenerate full aligned codebase (schema + server + frontend) with these core features intact.
```


