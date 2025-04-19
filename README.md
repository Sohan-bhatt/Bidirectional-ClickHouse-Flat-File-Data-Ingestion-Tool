# 🧠 Bidirectional ClickHouse ⇄ Flat File Ingestion Tool

This project provides a complete web-based tool using Flask to ingest data **to and from ClickHouse** using `.csv` flat files.

> Built as part of the **Software Engineer Intern Assignment 2** for **ZEOTAP**, this tool supports full bidirectional ingestion, preprocessing, cleaning, previewing, and exporting.

---

## ✅ Features Implemented

- ✅ **ClickHouse → CSV** (table + column selection)
- ✅ **Flat File → ClickHouse** (upload, clean, insert)
- ✅ Form-based ClickHouse connection UI
- ✅ Previews 100 rows before export
- ✅ Handles NaNs, NULLs, binary/int/float types
- ✅ Flash messages for success and error
- ✅ Preserves UI state between actions
- ✅ Fully container-compatible via Docker (for local ClickHouse)

---

## 🧩 Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/clickhouse-ingestion-tool
cd clickhouse-ingestion-tool
```

### 2. Set Up Python Environment
```bash
python -m venv venv
source venv/bin/activate  # Or venv\Scripts\activate on Windows
pip install -r requirements.txt
```

### 3. Start ClickHouse Locally (No Password)
```bash
docker run -d --name clickhouse-server \
  -p 8123:8123 -p 9000:9000 \
  -e CLICKHOUSE_ALLOW_EMPTY_PASSWORD=1 \
  --ulimit nofile=262144:262144 \
  -v $(pwd)/clickhouse_data:/var/lib/clickhouse \
  clickhouse/clickhouse-server
```

> If you're using a password-protected server:
```bash
docker run -d --name clickhouse-server \
  -p 8123:8123 -p 9000:9000 \
  -e CLICKHOUSE_PASSWORD='your_password' \
  --ulimit nofile=262144:262144 \
  -v $(pwd)/clickhouse_data:/var/lib/clickhouse \
  clickhouse/clickhouse-server
```

---

## 🚀 How to Run

```bash
python app.py
```
Visit [http://localhost:5001](http://localhost:5001) in your browser.

---

## 🔁 Usage Flow

### 🔄 Flat File → ClickHouse
1. Select "Flat File → ClickHouse"
2. Fill connection details
3. Upload `.csv` file + specify table name
4. Click "Import" → inserts cleaned + converted data into ClickHouse

### ↩️ ClickHouse → Flat File
1. Select "ClickHouse → Flat File"
2. Connect to ClickHouse
3. Choose table + columns
4. (Optional) Click Preview
5. Click "Export" → downloads `.csv`

---

## 🔍 Preview Feature
- Displays top 100 rows from selected columns before final export
- Prevents invalid queries when no columns are selected

---

## 🧼 Data Cleaning Logic
| Type        | Handling                 |
|-------------|--------------------------|
| `int`       | Fill missing with mean   |
| `0/1` cols  | Fill with most frequent  |
| `string`    | Fill with most frequent or `"NULL"` |
| Empty cols  | Dropped or defaulted     |

---

## 🔐 Using JWT Tokens (Optional)
If you're using ClickHouse Cloud or a secured ClickHouse instance with token-based authentication, you can use a **JWT token** instead of a password.

### When to use JWT:
- ✅ You're connecting to **ClickHouse Cloud** with token-based security
- ✅ Your admin provided a JWT/API token instead of a password
- ❌ Not needed for local Docker ClickHouse setup

### How to use:
1. In your ClickHouse Cloud console, navigate to `Settings` → `API Access` or `Tokens`
2. Generate a JWT or API Token
3. Copy the token
4. In the web app, paste it in the **JWT token field**
5. You can leave the password field empty

> The application will prioritize JWT over password: `jwt_token OR password`

---

## 📦 Sample Test Data
Use ClickHouse sample data:
```sql
CREATE DATABASE IF NOT EXISTS testdata;
USE testdata;
CREATE TABLE ontime (...);
INSERT INTO ontime SELECT * FROM s3(...) SETTINGS max_insert_threads = 40;
```

---

## 📹 Demo Video Submission
- Includes:
  - CSV upload
  - Table export
  - Preview data
  - Real-time connection

### 🔗 [[Video Demo Here](link-to-video)](https://drive.google.com/file/d/1T4Rzoma4GpTrMVzElKgZPyBmi6nM6Gn1/view?usp=drivesdk)

---

## 📜 Notes & Helpful Links
- [ClickHouse Python Integration](https://clickhouse.com/docs/en/integrations/python)
- [ClickHouse Cloud Setup](https://clickhouse.com/cloud/)
- Use `clickhouse-client` or Docker to debug manually

---

## 👨‍💻 Author
Developed by **SOHAN KUMAR**
