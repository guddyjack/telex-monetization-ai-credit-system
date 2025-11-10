# System Architecture — Telex Monetization & AI Credit System

## 🧩 Overview
The architecture of the Telex Monetization & AI Credit System is built for **scalability**, **security**, and **modularity**.  
It connects **frontend**, **backend**, **database**, and **third-party services** (Telex Core AI, Paystack, Stripe) through secure API calls.

---

## 🏗️ Architecture Components

### 1. Frontend (React + TailwindCSS)
- Built with **React.js** for dynamic user interfaces.
- Uses **Axios** for API communication.
- Displays:
  - Credit balances
  - Transaction history
  - Developer earnings
  - Purchase modals for credits

### 2. Backend (Node.js + Express.js)
- Handles business logic, authentication, and communication between frontend, database, and third-party APIs.
- Key endpoints:
  - `/api/credits/buy` — Handles payment initiation.
  - `/api/credits/deduct` — Deducts credits per AI usage.
  - `/api/developers/payout` — Processes developer revenue.
- Uses **JWT** for authentication and role-based access control.

### 3. Database (MongoDB)
- Stores all user, transaction, and developer data.
- Core Collections:
  - `Users` — user profiles, balances
  - `Transactions` — payment and usage logs
  - `Developers` — registered AI service providers
  - `Credits` — balances and deduction records

### 4. Payment Gateways
- **Paystack** (for local/Nigerian users)
- **Stripe** (for global users)
- Integrated via backend APIs using secure keys and webhooks.
- All transactions are verified and logged in the database.

### 5. AI Service Integration (Telex Core APIs)
- The backend communicates with Telex Core APIs.
- Credit deduction occurs before each AI request.
- Provides standardized endpoints for all AI modules to consume credits uniformly.

---

## 🔄 Communication Flow
```mermaid
sequenceDiagram
User ->> Frontend: Request AI Service
Frontend ->> Backend: Send API Call + Auth Token
Backend ->> MongoDB: Verify user & credit balance
Backend ->> Payment API: If needed, initiate purchase
Backend ->> Telex Core API: Send AI Request
Telex Core API ->> Backend: Return AI Response
Backend ->> MongoDB: Deduct credits & log usage
Backend ->> Frontend: Return AI Output + Updated Balance
Frontend ->> User: Display AI Response & Usage Stats
