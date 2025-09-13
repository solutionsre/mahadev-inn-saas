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
