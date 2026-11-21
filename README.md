# 🕌 Fouta Masdjid App

**Donation & expense management for a mosque during Ramadan.**
A Streamlit web app that tracks donor pledges, payments received and expenses, and produces daily, monthly and yearly reports.

🇫🇷 [Version française plus bas](#-français)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/status-in%20use%20locally-brightgreen?style=flat)

---

## Why this project

During Ramadan, the mosque collects pledges from donors and receives payments in several installments, while paying ongoing expenses. Tracking all of this on paper made it hard to answer simple questions: *Who still owes what? How much did we collect today? What is our balance this month?*
This app answers them in one place.

## ✨ Features

- 🔐 **Login** with authorized accounts (credentials kept out of the code)
- 👥 **Donors**: record each donor's pledge, edit or delete
- 💰 **Payments received**: log installments per donor, with notes
- 💸 **Expenses**: add, edit and list expenses
- 📊 **Dashboard**: total collected, total spent, balance, and each donor's status (paid / partially paid / remaining amount)
- 📅 **Reports**: daily, monthly and yearly summaries, with payments per month

<!-- Add screenshots here:
![Dashboard](docs/dashboard.png)
-->

## 🏗️ Architecture

```
app.py                 → entry point: login page
pages/                 → UI layer (one Streamlit page per feature)
  dashboard.py
  donateurs.py         (donors)
  encaissements.py     (payments received)
  depenses.py          (expenses)
  rapports.py          (reports)
service/               → logic layer, no UI code
  db_manager.py        → data access: SQLite CRUD
  calculs.py           → business rules: totals, balances, donor status, reports
database/database.db   → local SQLite database (not versioned)
secret.py              → credentials (not versioned, see secret.example.py)
```

The UI (`pages/`) never talks to the database directly: it goes through `service/`.
This separation keeps business rules testable and makes it possible to change the interface or the database later without rewriting everything.

## 🚀 Getting started

```bash
git clone https://github.com/mouhazba/fouta-masdjid-app.git
cd fouta-masdjid-app

python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Credentials: copy the template, then set your own users and passwords
cp secret.example.py secret.py

# Database: create the folder and the tables (first run only)
mkdir -p database
python -c "from service.db_manager import create_tables; create_tables()"

streamlit run app.py
```

## 🗺️ Roadmap

- [ ] Hash passwords (bcrypt) instead of storing them in plain text
- [ ] Create the database automatically on first launch
- [ ] Shared `require_login()` helper for all pages
- [ ] Export reports to PDF / Excel
- [ ] Unit tests for `service/calculs.py`
- [ ] Deployment (Streamlit Community Cloud with `st.secrets`)

---

## 🇫🇷 Français

**Application de gestion des dons et des dépenses d'une mosquée pendant le Ramadan.**

Pendant le Ramadan, la mosquée reçoit des promesses de dons payées en plusieurs fois, tout en réglant des dépenses. Cette application centralise tout : **donateurs**, **encaissements**, **dépenses**, **tableau de bord** (total encaissé, dépenses, solde, statut de chaque donateur) et **rapports** journaliers, mensuels et annuels.

Architecture en couches : l'interface (`pages/`) passe toujours par la couche `service/` (accès aux données et calculs), jamais directement par la base SQLite.

Installation : voir la section *Getting started* ci-dessus.

---

👤 Built by [Mamadou (Mahadiou) Ba](https://github.com/mouhazba)
