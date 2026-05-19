# Product Requirements Document - PAXA Fintech Platform

**Version:** 2.0  
**Last Updated:** 2026  
**Status:** Approved Architectural Pivot (AI-First Conversational Assistant)

---

## 1. Executive Summary

PAXA is a pioneering AI-first conversational fintech platform that completely replaces the traditional visual user dashboard with an intelligent, chat-driven financial assistant named **SalesCoach**. 

Through natural language conversations on **WhatsApp (via Twilio)** and a **premium Glassmorphism Web Chat interface**, users can seamlessly deposit cryptocurrencies (USDT and USDC) to fund their virtual wallet, send and withdraw/remove funds (fiat bank transfers or crypto payouts), pay electricity bills, purchase mobile airtime, fund sports betting accounts, and book domestic flights. 

An **Admin Dashboard** is provided strictly for platform operations, risk management, financial auditing, and AI model oversight, keeping the customer-facing experience purely conversational.

---

## 2. Product Vision

**Mission:** Democratize access to modern banking, travel, utilities, and crypto services by replacing complex visual dashboards with a intuitive, text-based AI companion.

**Core Value Proposition:**
- **Zero-Learning-Curve Banking:** All actions are performed using normal human conversation.
- **Unified Virtual Wallet:** Deposit crypto (USDT/USDC) directly to fund a secure multi-currency virtual wallet, immediately convertible to local currency (NGN) for utility payments and flight bookings.
- **Universal Financial Utility:** Pay for electricity, buy airtime, fund betting accounts, and book flights in under 60 seconds of chat.
- **Intelligent Financial Guidance:** Built-in portfolio analytics, risk mitigation advice, and interactive spending coaches that proactively assist the user.

---

## 3. Target Users

### 3.1 Primary User Cohorts
- **Crypto-Centric Savers:** Users who prefer to keep holdings in stablecoins (USDT/USDC) and deposit them to fund their daily transactions in local currency.
- **Convenience Seekers:** Tech-savvy individuals who prefer a direct chat interface (e.g., WhatsApp) over downloading apps or signing into complex dashboards.
- **Sports Betting Enthusiasts:** Users in Nigeria who require frictionless and instant betting account funding.
- **Domestic Flight Bookers:** Passengers seeking a quick, automated, and conversational flight search and booking engine.

### 3.2 Geographic Focus
- **Primary:** Nigeria (fiat transfers, domestic flight bookings, airtime, and utility bill payments).
- **Secondary:** Global (cryptocurrency deposits, stablecoin wallets, and global AI-coaching).

---

## 4. Core Features & Conversational Modules

All features are designed to be triggered and completed entirely through natural language chat conversation. The visual user dashboard has been removed.

### 4.1 Virtual Wallet & Crypto Module

#### 4.1.1 Wallet & Auto-Conversion Engine
- **Multi-Asset Balance:** Virtual wallets support NGN, USDT, and USDC balances.
- **Crypto Deposits:** SalesCoach dynamically generates secure, user-specific deposit addresses for:
  - **USDT** (Tron TRC-20, Ethereum ERC-20, Polygon)
  - **USDC** (Polygon, Ethereum ERC-20)
- **Instant Liquidation:** Automatic exchange rate calculation to convert deposited stablecoins into NGN virtual wallet balances when initiating fiat purchases.

#### 4.1.2 Conversational Send & Withdraw
- **Send Fiat (NGN P2P & Payouts):** 
  - Users can say, "Send ₦5,000 to 0123456789 GTBank" or "Transfer ₦2,000 to Olumide."
  - Backend integration resolves the recipient's name via banking APIs and displays a glowing confirmation card in the chat window.
- **Withdraw/Remove Funds:**
  - **Fiat Withdrawal:** Instant payout to linked Nigerian bank accounts.
  - **Crypto Payout:** Transfers stablecoins (USDT/USDC) to external blockchain addresses with real-time fee calculation.

---

### 4.2 Utility Bills & Mobile Services Module

#### 4.2.1 Conversational Electricity Payments
- **Supported Providers:** Major Nigerian distribution companies (IKEDC, EKEDC, AEDC, IBEDC, KAEDCO, PHED, etc.).
- **Real-Time Validation:** Verifies meter number and outputs the registered customer name and address within the chat before processing the payment.
- **Token Delivery:** Automatically returns a 20-digit utility token within the chat along with a downloadable PDF receipt.

#### 4.2.2 Conversational Airtime Purchases
- **Supported Telecoms:** MTN, Airtel, Glo, 9mobile.
- **Instant Credit:** Top-up registered phone number (or external numbers) directly from the virtual wallet balance with zero transaction fees.

---

### 4.3 Conversational Betting Integration

- **Betting Platforms:** Direct API integrations with SportyBet, Bet9ja, BetKing, and 1xBet.
- **Validation Loop:** SalesCoach verifies the Player ID and details via the betting platform API and confirms the account owner's name in chat before depositing.
- **Direct Wallet Payout:** Instant wallet debit and immediate credit to the betting account.

---

### 4.4 WhatsApp & Web Flight Booking Module

- **Booking Flow:** Fully bot-guided conversational questionnaire extracting departure city, dest, dates, number of passengers, passenger names, and seat preferences.
- **Real-time Inventory:** Integrates with local airline APIs (Air Peace, Ibom Air, Arik Air, ValueJet) to output a formatted selection of flights with prices.
- **E-ticket Issuance:** Deducts ticket cost from the virtual wallet, registers the passenger, generates a Passenger Name Record (PNR), and delivers e-tickets as PDF attachments inside the chat interface.

---

### 4.5 AI Financial Assistant Central Interface (SalesCoach)

SalesCoach acts as the core operating system for the user. 
- **Intent Recognition:** Understands user goals (e.g., utility payment, crypto withdrawal, flight search) from unstructured conversations.
- **Slot Filling:** Interactively prompts for missing variables (e.g., asking for the meter number if the user just says "I want to pay for electricity").
- **Interactive Confirms:** Generates rich glassmorphic confirmation frames within the chat drawer, forcing the user to explicitly click or type authorization before money leaves the wallet.
- **Security Guardrails:** Demands and validates a 6-digit Two-Factor Authentication (2FA) code directly inside the chat window for high-value transactions.
- **Personalized Advice:** Provides context-aware financial coaching (e.g., analyzing spending trends, portfolio risk concentrations, and budget goals) based on wallet activities.

---

## 5. Technical Architecture

### 5.1 Technology Stack
- **Backend:** Laravel 13.8 with PHP 8.3+
- **AI Core:** Laravel AI 0.7.0 (bound to secure LLM endpoints)
- **Messaging Pipeline:** Twilio WhatsApp Business API
- **Databases:** SQLite (local development), PostgreSQL (production)
- **Frontend UI:** Glassmorphism Web Chat panel built on Blade and Vue.js with Vite
- **Security & Payouts:** Laravel Sanctum & secure banking/payment gateway APIs

### 5.2 Conversational Platform Diagram

```
                              PAXA Platform
                              ┌────────────────────────────────┐
                              │     WhatsApp / Twilio Bot      │
                              │     Glassmorphic Web Chat      │
                              └───────────────┬────────────────┘
                                              │ Natural Language
                                              ▼
                              ┌────────────────────────────────┐
                              │      Laravel AI Engine         │
                              │ ├── Intent Classification      │
                              │ ├── Slot-Filling Router        │
                              │ └── Conversational State       │
                              └───────────────┬────────────────┘
                                              │ Validated Action
                                              ▼
                              ┌────────────────────────────────┐
                              │     Core Wallet Engine         │
                              │ ├── NGN / USDT / USDC Accounts │
                              │ └── Auto-Conversion Module     │
                              └───────┬──────────────┬─────────┘
                                      │              │
              ┌───────────────────────┘              └────────────────────────┐
              ▼                                                               ▼
┌───────────────────────────────┐                              ┌───────────────────────────────┐
│     Third-Party Service APIs  │                              │      Admin Operations Panel   │
│ ├── Crypto Gateway            │                              │ ├── User Audit & KYC          │
│ ├── Banking & Payout Gateway  │                              │ ├── Transaction Oversight     │
│ ├── Utility Providers (Meter) │                              │ ├── AI Performance & Metrics  │
│ ├── Flight Inventory APIs     │                              │ └── System Config & Rates     │
│ └── Sports Betting Providers  │                              │                               │
└───────────────────────────────┘                              └───────────────────────────────┘
```

---

## 6. Database Schema

### 6.1 Database Tables
- `users` - User accounts, KYC profiles, and security preferences.
- `wallets` - Virtual wallet balances (NGN, USDT, USDC) and deposit address records.
- `transactions` - Master ledger tracking all deposits, sends, withdrawals, and purchases.
- `utility_payments` - Log of electricity bill purchases, meter details, and tokens issued.
- `betting_funding_logs` - Logs of bookmaker transactions, player IDs, and status codes.
- `flight_bookings` - Flight schedules, passenger information, PNR records, and flight statuses.
- `ai_conversations` - Conversational state log, chat transcripts, slot progress, and assistant feedback.
- `platform_settings` - Configuration variables, gas fees, conversion spreads, and vendor APIs.

### 6.2 Entity Relationships
```php
User
├── wallets (One-to-Many)
├── transactions (One-to-Many)
├── utility_payments (One-to-Many)
├── flight_bookings (One-to-Many)
├── betting_funding_logs (One-to-Many)
└── ai_conversations (One-to-Many)
```

---

## 7. Unified Conversational User Flows

SalesCoach coordinates the user journey purely through text-based multi-turn dialogues.

### 7.1 Registration & Onboarding
1. User enters their email and password via the web portal.
2. The web page immediately transitions into the glassmorphic SalesCoach interface.
3. SalesCoach greets the user, instructs them to complete email verification (code entered in chat), prompts for basic KYC (Full Name, Date of Birth), and helps them set up 2FA.
4. SalesCoach explains that it will serve as their unified wallet assistant, ready to process transfers, crypto deposits, and bill payments.

### 7.2 Conversational Transaction Execution
1. **Trigger:** The user submits a conversational request (e.g., "Buy MTN airtime for ₦1,000").
2. **Intent Analysis:** The AI engine classifies the intent and extracts active slots.
3. **Information Gathering:** If any slots are missing (e.g. meter number, phone number), SalesCoach prompts the user.
4. **Resolution Check:** The system verifies recipient names or meter details via backend APIs.
5. **Interactive Gate:** SalesCoach displays a rich, structured visual Card detailing the payment summary and transaction fees, showing a "Confirm" button.
6. **MFA Verification:** For high-value transactions, SalesCoach prompts the user to enter their 2FA code directly in the chat panel.
7. **Settlement & Receipt:** Funds are debited from the virtual wallet, vendor APIs are executed, and SalesCoach prints a success confirmation card with tokens/e-tickets.

---

## 8. Interface Specifications

### 8.1 Premium Web Chat Drawer
- **Responsive Screen Compatibility:** Seamlessly scaling from 320px (mobile chat window) up to 2560px (wide desktop panel).
- **Glassmorphic Theme:** Dark and Light mode support with saturation filters, smooth gradient cards, glow effects for pending actions, and active animations for typing states.
- **Rich Message Cards:** Success receipt tickets, flight boarding pass mockups, and structured tables are dynamically rendered as styled HTML bubbles inside the chat timeline rather than generic text.

### 8.2 WhatsApp UI Integration
- **Clean Prompts:** Standardised text layouts with clear bullet points, bold keywords, and structural dividers (`---`).
- **Twilio Media Support:** Immediate PDF ticket generation and dynamic QR codes sent directly as rich WhatsApp attachments.

---

## 9. Admin Dashboard Features

While users interact exclusively via chat, PAXA administrators manage operations using a comprehensive web-based Admin Dashboard.

### 9.1 Platform Health & Analytics
- **Conversational Metrics:** Tracks message response times, active session counts, user satisfaction ratings, and LLM token usage.
- **Financial Balance Sheet:** Real-time visibility into overall platform virtual wallet balances (fiat NGN pools, total USDT/USDC deposits, pending withdrawals).

### 9.2 Transaction Auditing & Fraud Control
- **Manual Oversight:** Ability to review and manually override pending high-value transfers, utility tokens that failed to generate, or flight cancellation refunds.
- **KYC Verification:** Admin tools to inspect submitted documents and verify/suspend users.

### 9.3 AI Agent Settings & Prompt Oversight
- **Configuration Controls:** Adjust model temperature, toggle tool-access options (such as the portfolio analyzer, market data tool), and configure system prompts.
- **Incident & Hallucination Logger:** Captures flagged or negative conversational feedback to retrain underlying intent classifiers.

---

## 10. Security & Compliance

- **AES-256 Encryption:** Secure database storage of sensitive user bank information, API keys, and wallet private credentials.
- **In-Chat Two-Factor Authentication:** Standardised secure validation frames embedded directly inside the conversational window to authorize transactions.
- **Compliance Logging:** Immutable transaction histories and conversational audit trails to comply with AML (Anti-Money Laundering) requirements in Nigerian and international operations.

---

## 11. API Endpoints

### 11.1 Authentication & Profile
- `POST /api/auth/register` - Create account
- `POST /api/auth/verify-code` - Verify onboarding code via chat interface

### 11.2 Conversational AI Core
- `POST /api/ai/message` - Submits user chat input; returns the next AI response, updated state, and rich card templates.
- `GET /api/ai/history` - Fetches conversation logs.

### 11.3 Virtual Wallet & Transactions
- `POST /api/wallet/crypto-deposit` - Dynamic deposit address spinner.
- `POST /api/wallet/transfer` - Initiates funds transfer (Naira payout or P2P).
- `POST /api/wallet/withdraw` - Processes external crypto/fiat payouts.

### 11.4 Third-Party Webhook Receivers
- `POST /webhooks/twilio` - Receives incoming WhatsApp messages from Twilio.
- `POST /webhooks/payment` - Handles payment settlement confirmations from banking providers.
- `POST /webhooks/utility` - Utility provider settlement alerts.

---

## 12. MVP Roadmap

### Phase 1 (MVP)
- [x] Onboarding & email verification via chat.
- [x] Deposit USDT/USDC into virtual wallet (Tron and Polygon network).
- [x] Send NGN to local banks & withdraw/remove wallet funds.
- [x] Conversational airtime and electricity utility bill payment.
- [x] Conversational betting account funding (SportyBet, Bet9ja).
- [x] Conversational flight booking with PNR and e-ticket receipt generation.
- [x] Admin panel to manage platform parameters, transaction ledgers, and AI settings.
- [x] Complete removal of visual user dashboard.

### Phase 2
- Multi-token deposit networks (Solana, Arbitrum, Optimism).
- Automated AI price alerts and portfolio balancing.
- Conversational international flight bookings.
- Voice message processing (user sends a voice note to book flights or send money).

---

## 13. Success Metrics

- **Conversational Accuracy:** Percentage of user messages successfully mapped to specific intents (Target: > 96%).
- **Transaction Completion Time:** Average time to complete a utility payment or money transfer via chat (Target: < 45 seconds).
- **Crypto-to-Fiat Flow Volume:** Daily volume of USDT/USDC deposits and automatic conversions into transactional fiat.
- **AI Recommendation Acceptance:** Percentage of coaching and savings tips accepted by users (Target: > 60%).
- **System Uptime:** Availability of chat webhooks, Twilio connection, and transaction handlers (Target: 99.9%).
