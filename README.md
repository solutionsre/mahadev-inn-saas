# 🏨 Mahadev Inn — SaaS Hotel Booking System

A **multi-tenant, multi-branch SaaS booking platform** built with:

* **Backend** → [NestJS](https://nestjs.com/) + [Prisma](https://www.prisma.io/) + PostgreSQL  
* **Frontend** → [Next.js](https://nextjs.org/) + [TailwindCSS](https://tailwindcss.com/)  
* **Auth** → JWT with Role-Based Access Control (RBAC)  
* **Export** → PDF & Excel using jsPDF and XLSX  

---

## ✨ Core Business Logic (Fixed Rules)

### 🎯 Tenants & Branches
* Each **Hotel = Tenant**  
* Each **Tenant** can have multiple **Branches** (e.g., Kathmandu-1, Pokhara, Bhairawaha).  

### 👥 User Roles
* **Owner**
  * Full control of all branches (within their tenant).
  * Can manage inventory (room capacities).  
* **Manager**
  * Restricted to **their branch only**.
  * Can create, edit, and delete bookings.  
* **Viewer**
  * Restricted to **their branch only**.
  * Can only **view bookings & occupancy**, no edits.  

### 📝 Booking Rules
* **Branch-wise enforced** → Managers/Viewers cannot see other branches.  
* **Owner** can view all branches combined.  
* **BookingNo format** → `BranchName-YYYYMMDDHHMMSS-UID`  
* **Booking Status** → `bookingstat` (default = `Confirmed`).  
* **Validation** →  
  - Agent contact must be numeric.  
  - Check-out must be after check-in.  
  - If Meal Plan = `EPKitchen`, selfCooking (numeric) is mandatory.  

### 📊 Occupancy
* Calculated **per day per branch**:
- occupancy% = (rooms booked / branch capacity) * 100
* Branch capacity stored in `RoomInventory` (single, double, triple, quard).  
* Owners see **aggregate occupancy** across all branches.  

### 📄 PDF / Excel
* After each booking → **downloadable PDF confirmation**.  
* Calendar & Dashboard support **monthly and daily exports** (PDF + Excel).  

---

## 📂 Repo Structure
```
mahadev-inn-saas/
├─ 📦 server/ # NestJS API + Prisma
│ ├─ 📄 prisma/schema.prisma # Core DB models
│ ├─ 🌱 prisma/seed.ts # Seeds demo data
│ └─ 📂 src/
│ ├─ 🔑 auth/ # JWT Auth + Guards
│ ├─ 👥 users/ # User management
│ ├─ 🏨 bookings/ # Booking APIs + Occupancy logic
│ ├─ 📊 inventory/ # Room capacity mgmt (Owner only)
│ └─ ⚙️ prisma/prisma.service.ts
└─ 💻 client/ # Next.js + Tailwind frontend
├─ 🔌 src/lib/api.ts # Axios wrapper with JWT
├─ 🔐 src/lib/auth.tsx # Auth context provider
└─ 📂 src/app/
├─ 🔑 login/page.tsx # Login page
├─ 📊 (dashboard)/page.tsx # Dashboard: bookings + new entry
├─ 🗓️ (dashboard)/calendar/ # Calendar occupancy view
└─ 🛏️ (dashboard)/inventory # RoomInventory editor
```

## 🚀 Getting Started
## Diagram Plan (textual first):
```
   ┌───────────────────────────┐
   │         Tenant            │
   │     (Hotel Group)         │
   └─────────────┬─────────────┘
                 │
   ┌─────────────┴─────────────┐
   │                           │
┌──▼───────┐              ┌────▼───────┐
│ Branch 1 │              │ Branch 2   │   ...
│ Kathmandu │              │ Pokhara    │
└───┬──────┘              └─────┬──────┘
    │                            │
 ┌──▼───────────┐          ┌─────▼──────────┐
 │ Users        │          │ Users          │
 │ - Owner      │          │ - Manager      │
 │ - Manager    │          │ - Viewer       │
 │ - Viewer     │          │                │
 └──┬───────────┘          └─────┬──────────┘
     │                            │
┌────▼───────────┐        ┌───────▼───────────┐
│ Bookings       │        │ Bookings          │
│ - BookingNo    │        │ - BookingNo       │
│ - Agent        │        │ - Status          │
│ - CheckIn/Out  │        │ - Occupancy calc  │
└────────────────┘        └───────────────────┘
```

### 1. Clone repo & install
```bash
git clone https://github.com/your-org/mahadev-inn-saas.git
cd mahadev-inn-saas
cd server # Running inside the Server Folder 
npx prisma migrate reset
npx prisma generate
npm run seed
```

### 📈 Business Advantages

- Hotels → Centralized booking & occupancy tracking across branches.
- Chain Owners → Clear visibility across all locations.
- SaaS Provider → One codebase, unlimited tenants.
- Scalable Pricing → per tenant, per branch, or per booking.

### 🛡️ Core Features Checklist

✅ Multi-tenant architecture (hotel isolation)
✅ Branch isolation (data restricted by JWT)
✅ Role-based permissions (Owner/Manager/Viewer)
✅ Auto BookingNo generation
✅ Occupancy dashboard & calendar
✅ PDF/Excel exports
✅ Seeded demo users (owner/manager/viewer)
