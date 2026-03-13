# 🎋 Bamboo Dine — Restaurant Table Booking System

A full-stack restaurant booking system built with **FastAPI** + **pure HTML/CSS/JS**, powered by an AI chatbot (Bamboo AI) and automated workflows via **n8n** → **Google Sheets**.
<img width="803" height="722" alt="Screenshot 2026-03-14 012629" src="https://github.com/user-attachments/assets/7775625f-e24e-497a-a6cf-49072e8d04c3" />
<img width="1479" height="747" alt="Screenshot 2026-03-14 012906" src="https://github.com/user-attachments/assets/bed85b66-985b-4a57-9b88-647f35c0526c" />




---

## ✨ Features

- 🤖 **AI Chatbot (Bamboo AI)** — Bilingual (Bengali/English) booking assistant powered by GPT-4o-mini
- 🪑 **Table Selection** — Chatbot shows available tables in real-time, customer picks their preferred one
- 📋 **Admin Dashboard** — Full booking management with stats, availability grid, manual booking & update
- 📧 **Auto Email** — Beautiful bamboo-themed confirmation email with Booking ID sent via Gmail
- 🔄 **n8n Automation** — Google Sheets as database, email via SMTP — no backend DB needed
- 📊 **Google Sheets** — 4-tab workbook: Bookings, Availability, Stats, Config

---

## 🏗️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI (Python) |
| Frontend | Pure HTML / CSS / JS |
| AI | OpenAI GPT-4o-mini |
| Automation | n8n (self-hosted) |
| Database | Google Sheets |
| Email | Gmail SMTP via n8n |

---

## 📁 Project Structure

```
bamboo-dine/
├── main.py                  # FastAPI backend — all APIs + n8n webhook calls
├── requirements.txt         # Python dependencies
├── .env.example             # Environment variables template
├── README.md
├── templates/
│   ├── index.html           # Restaurant website + AI chatbot popup
│   └── admin.html           # Admin dashboard
├── static/                  # Static assets
└── n8n/
    └── Bamboo Dine.json        # n8n automation workflow (5 webhooks)
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/r0nY-0017/Bamboo Dine.git
cd Bamboo Dine
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure environment variables
```bash
cp .env.example .env
```

Edit `.env`:
```env
OPENAI_API_KEY=sk-...
ADMIN_PASSWORD=yourpassword

N8N_CHECK_URL=http://localhost:5678/webhook/bamboo-check
N8N_BOOKING_URL=http://localhost:5678/webhook/bamboo-booking
N8N_CANCEL_URL=http://localhost:5678/webhook/bamboo-cancel
N8N_LIST_URL=http://localhost:5678/webhook/bamboo-list
N8N_UPDATE_URL=http://localhost:5678/webhook/bamboo-update
```

### 4. Run the server
```bash
uvicorn main:app --reload --port 8000
```

| URL | Page |
|---|---|
| `http://localhost:8000` | Restaurant website + chatbot |
| `http://localhost:8000/admin` | Admin dashboard |

---

## 🔄 n8n Workflow Setup

### Import the workflow
1. Open n8n → **Workflows** → **Import**
2. Upload `n8n/workflow.json`
3. Replace the following placeholders in all Google Sheets nodes:

| Placeholder | Replace with |
|---|---|
| `YOUR_SHEET_ID` | Your Google Sheet ID from the URL |
| `GSHEETS_CREDENTIAL_ID` | Your Google Sheets OAuth2 credential ID in n8n |
| `SMTP_CREDENTIAL_ID` | Your Gmail SMTP credential ID in n8n |

4. **Activate** the workflow

### Webhook Endpoints

| Webhook | Path | Function |
|---|---|---|
| Check Availability | `bamboo-check` | Count bookings for a date+time slot |
| Save Booking | `bamboo-booking` | Append row to Sheets + send email |
| Cancel Booking | `bamboo-cancel` | Delete row by Booking ID |
| List Bookings | `bamboo-list` | Read + filter bookings for admin |
| Update Booking | `bamboo-update` | Update any field by Booking ID |

---

## 📊 Google Sheets Setup

Import `bamboo_dine_sheets.xlsx` to Google Sheets:
**File → Import → Upload → Replace spreadsheet**

The workbook has **4 tabs**:

| Tab | Purpose |
|---|---|
| **Bookings** | All booking data — n8n reads/writes here |
| **Availability** | Formula-based live slot grid (1=booked, 0=free) |
| **Stats** | Auto-calculated dashboard numbers |
| **Config** | Webhook URLs, slot times, column reference |

### Bookings Sheet Columns
```
ID | Name | Phone | Email | Date | Time | Table | Guests | Status | Source | Notes | Created_At
```

---

## 🤖 AI Chatbot Flow

```
Customer: "আগামীকাল ৭টায় ৪ জনের বুকিং চাই"
    ↓
Bot asks: date, time, guests
    ↓
Bot checks availability → n8n → Google Sheets
    ↓
Bot: "T-2, T-4, T-5 খালি আছে। কোনটি নেবেন?"
    ↓
Customer picks table → Bot collects name, phone, email
    ↓
Booking saved to Sheets + Confirmation email sent
```

### Booking ID Format
`BD` + 6 random characters — e.g. `BD3F9A2C`

Customers need this ID to cancel their booking via chatbot.

---

## 🖥️ Admin Dashboard

- **Dashboard** — Today's stats + slot availability grid
- **Booking List** — Search, filter by date/status, cancel or update any booking
- **Availability** — Visual 6×6 table grid per time slot
- **New Booking** — Manual booking with live free-table selector

---

## 📧 Email Configuration

Use Gmail App Password for SMTP:
1. Google Account → **Security** → **2-Step Verification** → **App passwords**
2. Generate password for "Mail"
3. Use this password in n8n SMTP credential

---

## 🔒 Environment Variables

| Variable | Description |
|---|---|
| `OPENAI_API_KEY` | OpenAI API key for GPT-4o-mini |
| `ADMIN_PASSWORD` | Admin dashboard login password |
| `N8N_CHECK_URL` | n8n webhook for availability check |
| `N8N_BOOKING_URL` | n8n webhook for saving bookings |
| `N8N_CANCEL_URL` | n8n webhook for cancelling bookings |
| `N8N_LIST_URL` | n8n webhook for listing bookings |
| `N8N_UPDATE_URL` | n8n webhook for updating bookings |

> ⚠️ Never commit your `.env` file to version control.

---

## 🍽️ Restaurant Info

| | |
|---|---|
| **Name** | Bamboo Dine |
| **Location** | ধানমন্ডি ২৭, ঢাকা-১২০৫ |
| **Hours** | সন্ধ্যা ৬টা – রাত ১১টা (শনি–বৃহস্পতি) |
| **Tables** | 6 (T-1 to T-6) |
| **Slots** | 18:00 / 19:00 / 20:00 / 21:00 / 22:00 / 23:00 |
| **Max per slot** | 6 bookings |

---

## 📄 License

MIT License — feel free to use and modify.
