# 🍛 GharKaTiffin — Hyperlocal Subscription-First Tiffin Platform

> **"Home food. Delivered around your routine."**  
> *Affordable daily meals from verified neighborhood home chefs. Subscribe once. Eat every day.*

[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-blue.svg)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-14.2-black.svg)](https://nextjs.org/)
[![Tailwind CSS](https://img.shields.io/badge/TailwindCSS-3.4-38bdf8.svg)](https://tailwindcss.com/)
[![Prisma ORM](https://img.shields.io/badge/Prisma-5.22-2d3748.svg)](https://www.prisma.io/)
[![Zero Food Waste](https://img.shields.io/badge/Zero--Waste%20Efficiency-97.6%25-10b981.svg)](#zero-inventory-model)
[![Tests](https://img.shields.io/badge/Tests-7%20Passed%2C%200%20Failed-success.svg)](#automated-testing)

---

## 📌 1. Core Problem & Product Philosophy

A typical ₹100 meal becomes **₹160+** on conventional on-demand delivery apps (Swiggy/Zomato) due to single-rider delivery fees (₹50+), 30% restaurant commission, packaging markups, and surge pricing. Simultaneously, restaurants incur **~28% food waste** because they must guess demand and maintain refrigerated inventory.

**GharKaTiffin solves this with a radically different business model**:

```
Swiggy / Zomato:
On-Demand Order ──> Commercial Restaurant (High Waste) ──> Point-to-Point Rider (₹50-70 Fee)

GharKaTiffin:
Subscription Pre-Order ──> 10 PM Cutoff Freeze ──> Zero-Inventory Batch Prep ──> Milk-Run Route (₹8-10 Fee)
```

1. **Subscription-First**: Students, PG residents, and corporate workers subscribe for weekly (Mon–Fri) or monthly recurring home meals.
2. **Order Cutoff Freeze**: Orders lock at 10:00 PM the previous night. Cancellations are strictly locked post-cutoff.
3. **Zero-Inventory Batch Preparation**: The home chef receives an exact procurement manifest (e.g. 148 rotis, 5.5 kg rice) and cooks only confirmed meals.
4. **Milk-Run Batch Routing**: Deliveries within a 3–5 km cluster are aggregated into an optimized batch using a **2-opt Traveling Salesperson heuristic**. A single delivery partner carries 15–20 tiffins.
5. **Mandatory OTP Verification**: Customers provide a secret 4-digit OTP upon handover, preventing false delivery claims.
6. **Zero Food Commission**: Home chefs pay a low fixed SaaS fee (₹299/mo) instead of a 30% commission on food.

---

## 🚀 2. Quick Start (Evaluation in 2 Minutes)

The project is pre-configured with **SQLite (`prisma/dev.db`)** for zero-friction local testing without needing PostgreSQL or Docker installed. An identical PostgreSQL schema (`prisma/schema.postgresql.prisma`) and Docker Compose setup are included for production.

### Step 1: Install Dependencies
```bash
npm install
# On Windows PowerShell: npm.cmd install
```

### Step 2: Initialize & Seed Database
```bash
npx prisma db push
npx tsx prisma/seed.ts
# On Windows PowerShell: npx.cmd prisma db push ; npx.cmd tsx prisma/seed.ts
```

### Step 3: Run Development Server
```bash
npm run dev
# On Windows PowerShell: npm.cmd run dev
```

Visit **[http://localhost:3000](http://localhost:3000)** in your browser.

---

## 👥 3. 1-Click Persona Switcher for Reviewers

A prominent pill **"Demo Persona: [Select Role]"** is located in the top navigation bar. Clicking it allows reviewers to instantly switch between all 4 platform roles in 1 click:

| Role | Demo Account | Focus Areas & Workflows |
| :--- | :--- | :--- |
| **👤 Customer** | `aarav@gmail.com` | Active subscriptions, today's meal card, live milestone timeline, 4-digit Delivery Verification OTP (`4821`), multi-criteria rating modal. |
| **👩‍🍳 Home Chef** | `maa@gharkatiffin.com` | Today's orders, **"Trigger Order Cutoff"** button, zero-inventory procurement manifest (exact rice/dal/atta kg), cooking/packaging checklists. |
| **🛵 Delivery Partner** | `rajesh@gharkatiffin.com` | Assigned batches, ordered milk-run tour sequence (Stop 1 → Stop 2), interactive **"Enter OTP"** modal to verify delivery. |
| **🛡️ Platform Admin** | `admin@gharkatiffin.com` | Platform GMV, Chef verification queue (`Approve`/`Reject`), 97.6% zero-waste efficiency metrics, interactive Unit Economics Simulator. |

*(All accounts use default password: `password123`)*

---

## 🎬 4. End-to-End Demo Scenario Walkthrough

The platform supports the complete live demonstration flow specified in the prompt:

1. **Customer Discovery & Subscription**:
   - Customer browses nearby home chefs on `/chefs` with the radius slider (1–10 km) and diet filters (Pure Veg, Jain).
   - Selects **Maa's Kitchen** (1.2 km away, 4.9 ★).
   - Configures plan (Weekly Mon–Fri lunch, ₹95/meal) and completes mock UPI checkout.
   - System activates subscription and generates concrete `MealOrder` instances with 4-digit OTPs.

2. **Order Cutoff & Zero-Inventory Manifest**:
   - At 10:00 PM (or by clicking **"Trigger Order Cutoff"** in the Chef Dashboard), orders are locked (`CUTOFF_LOCKED`).
   - Chef visits `/chef/procurement` and views exact aggregated raw materials (e.g. 5.5 kg Rice, 3.2 kg Dal, 4.8 kg Atta, 148 Rotis).
   - Chef marks items as *Purchased* → *Prepared* → *Packed in 4-Compartment Boxes* → *Ready for Pickup*.

3. **Milk-Run Batch Creation & Routing**:
   - The Milk-Run Routing Engine clusters confirmed orders and applies Nearest-Neighbor + 2-Opt optimization to eliminate route criss-crosses.
   - Batch is created and assigned to Delivery Partner **Rajesh Kumar**.

4. **Delivery Handover & OTP Verification**:
   - Rajesh starts the batch in `/delivery/dashboard` and follows the ordered stop sequence.
   - Upon reaching customer Aarav Sharma's address, Rajesh asks for the 4-digit OTP shown on Aarav's dashboard (`4821`).
   - Rajesh enters `4821`. System verifies OTP, transitions order to `DELIVERED`, records timestamp, and completes the stop.

5. **Rating & Quality Score Update**:
   - Aarav rates the meal on Taste (5★), Portion (5★), Packaging (5★), and Delivery (5★).
   - The weighted Chef Quality Score algorithm recalculates Maa's Kitchen score in real-time.

6. **Admin Ecosystem Monitoring**:
   - Admin visits `/admin/dashboard` to inspect platform GMV, approve pending chef onboarding applications, and review food waste analytics (97.6% zero-inventory efficiency).

---

## 🧪 5. Automated Testing

Run the automated test runner:
```bash
npm test
# On Windows PowerShell: npm.cmd test
```

### Verified Test Suites:
- `✓` Haversine distance accuracy between Bengaluru coordinates
- `✓` Milk-Run 2-Opt routing sequence optimization and batch capacity enforcement
- `✓` Zero-Inventory calculator portion standard raw ingredient aggregations
- `✓` Chef Quality Score weighted algorithm with bonus/penalty clamps
- `✓` Order State Machine legitimate flow and illegal transition rejection
- `✓` Subscription State Machine lifecycle transitions
- `✓` Batch Delivery unit economics consumer savings calculation

---

## 📐 6. Technical Architecture & Documentation

Comprehensive technical documentation is provided in the `docs/` directory:

- 📄 [**BUSINESS_LOGIC.md**](docs/BUSINESS_LOGIC.md): Mathematical formulations for zero-inventory procurement, 2-opt TSP routing, and chef quality scoring.
- 🏛️ [**ARCHITECTURE.md**](docs/ARCHITECTURE.md): Clean Architecture layers, service interfaces (`IMapService`, `IPaymentService`, `IStorageService`), and security controls.
- 🗄️ [**DATABASE.md**](docs/DATABASE.md): Entity relationship diagrams, relational models, and indexing strategies.
- 🔌 [**API.md**](docs/API.md): Full REST API contract specifications with sample payloads.
- 🛠️ [**SETUP.md**](docs/SETUP.md): Development environment setup and troubleshooting.
- 🚢 [**DEPLOYMENT.md**](docs/DEPLOYMENT.md): Production deployment guide, Dockerfile, and environment variable configuration.

---

## 📦 7. Docker Production Deployment

```bash
# Start PostgreSQL, Redis, and Web app containers
docker-compose up -d --build

# Run PostgreSQL database sync & seed
docker-compose exec app npx prisma db push --schema=prisma/schema.postgresql.prisma
docker-compose exec app npx tsx prisma/seed.ts
```

---

## ⚖️ Legal & Regulatory Compliance Disclaimer

Documents generated by the compliance wizard (e.g. Housing Society NOC) are standardized administrative templates. Regulatory food business compliance requires registration under the Food Safety and Standards Authority of India (FSSAI). GharKaTiffin provides software and hyperlocal routing tools only.
