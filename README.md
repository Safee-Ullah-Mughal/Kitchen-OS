<div align="center">
  <img src=".github/assets/hero-banner.png" alt="KitchenOS by Boldise Technologies" width="100%" />

  # KitchenOS 🍳
  **The Complete Enterprise POS & Cloud Kitchen Management Platform**

  [![Live Demo](https://img.shields.io/badge/Live_Storefront-test.boldise.com-00C7B7?style=for-the-badge)](https://test.boldise.com)
  [![Built By](https://img.shields.io/badge/Built_By-Boldise_Technologies-2b3137?style=for-the-badge)](https://boldise.com)
  [![Status](https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge)]()

  *KitchenOS is a proprietary, subscription-based enterprise solution. This repository is a technical showcase and portfolio overview of the platform's architecture and capabilities. The source code is closed.*
</div>

---

## 📑 Table of Contents
- [Overview](#-overview)
- [Core Ecosystem](#-core-ecosystem)
- [System Interface](#️-system-interface)
- [Technical Architecture](#️-technical-architecture)
- [How a Sale Flows Through the System](#-how-a-sale-flows-through-the-system)
- [Why KitchenOS](#-why-kitchenos)
- [Licensing & Demo](#-licensing--demo)
- [Connect](#-connect)

---

## 📖 Overview
KitchenOS is a full-stack restaurant and cloud-kitchen operating system built to handle everything from front-of-house sales to back-of-house logistics. Powered by a Python backend, it connects four areas that most kitchens run as separate tools:

- **Sales:** a web storefront and a touch-optimized POS feeding one order pipeline.
- **Inventory:** recipe-level BOM tracking that deducts raw materials per dish sold.
- **Customer retention:** a built-in CRM with WhatsApp marketing broadcasts.
- **Finance & compliance:** ledgers, daily reconciliation, and localized tax handling.

One platform, one database, one source of truth.

---

## ✨ Core Ecosystem

### 🏪 Front-of-House & Customer Experience
| Capability | Description |
|---|---|
| **Live Storefront** | Fully integrated web storefront ([test.boldise.com](https://test.boldise.com)) where customers place orders directly into the KitchenOS ecosystem. |
| **Lightning-Fast POS** | Touch-optimized point-of-sale interface designed for rapid order entry in high-volume environments. |
| **Live Order Tracking** | Real-time status updates for both kitchen staff and customers. |
| **CRM & WhatsApp Marketing** | Built-in customer relationship manager integrated with WhatsApp for targeted marketing broadcasts. |
| **Automated Alerts** | Email notifications for order statuses and End-of-Day (EOD) ledgers. |

### ⚙️ Back-of-House & Operations
| Capability | Description |
|---|---|
| **Intelligent Dashboard** | High-level overview of daily operations, sales metrics, and real-time expense management. |
| **Dynamic Recipe BOM** | Inventory tracking that deducts raw materials based on the exact Bill of Materials of each dish sold. |
| **HR & Staff Management** | Workforce portal for attendance tracking, payroll automation, and strict role-based access control. |
| **Smart Supplier Management** | Automated Purchase Order (PO) generation triggered by low-stock thresholds. |
| **Delivery Fleet Management** | End-to-end tracking and assignment for in-house delivery fleets. |
| **Account & Ledger Management** | Financial tracking for expenses, daily reconciliations, and supplier payments. |

---

## 🖥️ System Interface

### The Live Storefront
> *Customer-facing web portal for seamless online ordering.*

<img src=".github/assets/storefront.gif" alt="Customer Storefront" width="100%" />

### The Intelligent Dashboard
> *A centralized command center for owners to track expenses, sales, and automated ledgers.*

<img src=".github/assets/dashboard.gif" alt="KitchenOS Dashboard" width="100%" />

### Point of Sale Terminal
> *Streamlined checkout flow and live order management.*

<img src=".github/assets/pos-terminal.png" alt="POS Terminal" width="100%" />

### CRM & WhatsApp Automation
> *Engage your customer base directly with targeted WhatsApp marketing campaigns.*

<img src=".github/assets/crm-whatsapp.png" alt="CRM and WhatsApp Marketing" width="100%" />

### HR & Staff Management
> *Complete oversight of employee attendance, payroll, and system access rights.*

<img src=".github/assets/hr-staff.png" alt="HR and Staff Management Dashboard" width="100%" />

### Automated BOM & Inventory
> *Precision tracking that turns finished sales into raw material deductions and automated supplier POs.*

<img src=".github/assets/inventory-bom.png" alt="Inventory and BOM System" width="100%" />

---

## 🛠️ Technical Architecture

### Tech Stack
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Flask](https://img.shields.io/badge/flask-%23000.svg?style=for-the-badge&logo=flask&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)

| Layer | Technology |
|---|---|
| Backend | Python, Flask (modular blueprints) |
| Database | PostgreSQL via SQLAlchemy |
| Frontend | JavaScript |
| Hosting | Ubuntu server, WSGI deployment |
| Background jobs | Scheduled CRON tasks |

The system is engineered with modern, scalable web technologies to keep service stable during peak hours.

### Backend Organization (Mock Structure)
The application uses a modular blueprint architecture that separates POS operations, storefront APIs, and background tasks.

```text
kitchenos-core/
├── app/
│   ├── pos/              # Point of Sale touch interface routes
│   ├── storefront/       # Public-facing web ordering endpoints
│   ├── inventory/        # BOM deduction and supplier PO logic
│   ├── crm/              # WhatsApp API integrations and marketing
│   └── models/           # SQLAlchemy database schemas
├── tasks/                # Automated CRON jobs (EOD reports, low-stock alerts)
├── config/               # Environment and deployment configurations
└── run.py                # WSGI application entry point
```

### System Entity Relationship (High-Level)
The diagram below shows the relational integrity linking front-of-house sales to back-of-house logistics.

```mermaid
erDiagram
    %% Core
    RESTAURANT ||--o{ STOREFRONT : hosts
    STOREFRONT ||--o{ ORDER : receives

    %% Staff & HR
    RESTAURANT ||--o{ EMPLOYEE : employs
    EMPLOYEE ||--o{ ROLE : has
    EMPLOYEE ||--o{ ATTENDANCE : logs

    %% Customer & CRM
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER ||--o{ WHATSAPP_CAMPAIGN : targeted_by

    %% Sales & Inventory
    ORDER ||--|{ ORDER_ITEM : contains
    ORDER_ITEM ||--|| RECIPE : mapped_to
    RECIPE ||--|{ RECIPE_BOM : consists_of
    RECIPE_BOM }o--|| RAW_MATERIAL : deducts

    %% Supply Chain
    RAW_MATERIAL }o--|| SUPPLIER : sourced_from
    RAW_MATERIAL ||--o{ PURCHASE_ORDER : triggers_when_low
    SUPPLIER ||--o{ PURCHASE_ORDER : receives

    %% Finance
    ORDER ||--|| LEDGER_ENTRY : generates_revenue
    PURCHASE_ORDER ||--|| LEDGER_ENTRY : generates_expense

    %% Sampled attributes
    ORDER {
        uuid id
        string status "PENDING, PREPARING, DISPATCHED"
        float total_amount
    }

    RECIPE_BOM {
        uuid recipe_id
        uuid material_id
        float quantity_required
    }

    RAW_MATERIAL {
        uuid id
        string name
        float current_stock
    }
```

---

## 🔄 How a Sale Flows Through the System

```mermaid
flowchart LR
    A[Customer orders on Storefront or POS] --> B[Order created: PENDING]
    B --> C[Kitchen prepares: PREPARING]
    C --> D[Dispatched via delivery fleet: DISPATCHED]
    B --> E[BOM deducts raw materials]
    E --> F{Stock below threshold?}
    F -- Yes --> G[Auto-generate Purchase Order]
    F -- No --> H[Continue]
    B --> I[Ledger entry: revenue]
    G --> J[Ledger entry: expense]
    I --> K[EOD report emailed]
    J --> K
```

---

## 🎯 Why KitchenOS
- **Unified data:** orders, stock, staff, suppliers, and money live in one system instead of five.
- **Accurate costing:** per-dish BOM deduction shows real ingredient consumption.
- **Less manual work:** low-stock POs, EOD ledgers, and alerts run automatically.
- **Customer retention:** CRM and WhatsApp broadcasts are built in, not bolted on.
- **Controlled access:** strict role-based permissions across staff.

---

## 💼 Licensing & Demo
KitchenOS is a subscription-based enterprise product. For a live demonstration, an architecture discussion, or enterprise licensing, get in touch using the details below.

## 📬 Connect
KitchenOS is a flagship product developed by **[Boldise Technologies](https://boldise.com)**.

- **Developer / Architect:** Safee Ullah Mughal
- **LinkedIn:** [Your LinkedIn URL]
- **Email:** boldisetechnology@gmail.com
- **Company:** [boldise.com](https://boldise.com)
- **Live Storefront:** [test.boldise.com](https://test.boldise.com)

---

<div align="center">
  <sub>© Boldise Technologies. All rights reserved. This repository is a showcase only and does not grant any license to the proprietary source code.</sub>
</div>
