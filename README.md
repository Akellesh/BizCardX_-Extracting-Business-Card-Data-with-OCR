# BizCardX – Extracting Business Card Data with OCR 🪪

BizCardX is a **Streamlit web application** that reads a photo of a business card, extracts the printed details using **EasyOCR**, and lets a logged-in user save, view, update, and delete those contacts in a **PostgreSQL** database — image included.

Built for sales teams who collect a lot of paper business cards and want a fast, searchable digital record without manual data entry.

---

## ✨ Features

- 🔐 **User authentication** – login-protected app using `streamlit-authenticator` with bcrypt-hashed passwords.
- 📇 **OCR extraction** – upload a `.png`/`.jpg`/`.jpeg` business card image and extract text using EasyOCR.
- 🧠 **Field parsing** – regex-based logic automatically classifies extracted text into:
  - Name
  - Designation
  - Company
  - Contact number
  - Email
  - Website
  - Address
  - Pincode
- 🗄️ **PostgreSQL storage** – both the parsed text fields **and** the original card image (as bytes) are stored in a `CUSTOMERS` table.
- ✏️ **Manage records** – add a customer manually, update an existing one, or delete a record from the UI.
- 🚫 **Duplicate check** – warns if a customer (by name + designation) already exists before inserting.
- 🖥️ **Simple sidebar navigation** – Home, Upload & Manage DB, Settings (logout), Contact.

---

## 🧰 Tech Stack

| Purpose | Library |
|---|---|
| Web UI | [Streamlit](https://streamlit.io/) |
| Sidebar navigation | `streamlit-option-menu` |
| Login / auth | `streamlit-authenticator` |
| OCR engine | [EasyOCR](https://github.com/JaidedAI/EasyOCR) |
| Image handling | `Pillow`, `io` |
| Data wrangling | `pandas`, `numpy` |
| Database | PostgreSQL via `psycopg2` |
| Text parsing | `re` (regular expressions) |

---

## 📁 Project Structure

```
BizCardX_-Extracting-Business-Card-Data-with-OCR/
├── biz.py                     # Main Streamlit application (run this)
├── database.py                # Standalone script used while prototyping DB queries
├── db1.py                     # Standalone script used while prototyping DB queries
├── generate_key.py            # One-time script to create hashed_pw.pkl for login
├── hashed_pw.pkl              # Pre-generated hashed passwords used by biz.py
├── requirements.txt           # Python dependencies
├── Dataset/                   # Sample business card images (1.png - 5.png)
├── Problem Statement/         # Original project brief (PDF)
├── Project_Reference_material/
│   └── Reference_links.txt    # Helpful reference videos (auth & PostgreSQL setup)
└── .streamlit/
    └── secrets.toml           # PostgreSQL connection credentials used by st.secrets
```

> **Note:** `database.py` and `db1.py` are exploratory scripts from development (they contain hardcoded credentials and commented-out code) rather than part of the running application. The actual app logic lives entirely in `biz.py`.

---

## ⚙️ Setup & Installation

### 1. Prerequisites
- Python 3.9+
- PostgreSQL installed and running locally (or accessible remotely)

### 2. Clone the repository
```bash
git clone https://github.com/Akellesh/BizCardX_-Extracting-Business-Card-Data-with-OCR.git
cd BizCardX_-Extracting-Business-Card-Data-with-OCR
```

### 3. Create a virtual environment (recommended)
```bash
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate
```

### 4. Install dependencies
```bash
pip install -r requirements.txt
```
> EasyOCR will download its recognition model files on first run — this can take a few minutes and needs an internet connection.

### 5. Configure PostgreSQL credentials
Edit `.streamlit/secrets.toml` with your own database details:
```toml
[postgres]
host = "localhost"
port = 5432
dbname = "db1"
user = "postgres"
password = "your_password_here"
```
> ⚠️ **Do not commit real credentials to a public repo.** Treat `secrets.toml` as a local-only file (add it to `.gitignore`) and rotate any password that has already been pushed.

The `CUSTOMERS` table is created automatically on first run if it doesn't already exist.

### 6. (Optional) Regenerate login credentials
Login usernames/names are currently hardcoded in `generate_key.py` (`admin` / `manager`). To set your own passwords:
1. Edit the `passwords` list in `generate_key.py`.
2. Run it once to regenerate `hashed_pw.pkl`:
   ```bash
   python generate_key.py
   ```

### 7. Run the app
```bash
streamlit run biz.py
```
The app will open in your browser (default: `http://localhost:8501`). Log in with the `admin` or `manager` username and the corresponding password.

---

## 🚀 How to Use

1. **Login** with your username and password.
2. Go to **Upload and Manage DB → Upload BusinessCard**.
3. Upload a photo of a business card (see `Dataset/` for sample images).
4. EasyOCR extracts the text, and the app auto-fills Name, Designation, Company, Contact, Email, Website, Address, and Pincode.
5. Click **Create Business Card Contact in DB** to save the record (and the card image) to PostgreSQL.
6. Use **Add / Update Customer** to manually add a new contact or edit an existing one.
7. Use **Delete Record** to remove a contact by name.
8. Use **Settings** to log out.

---

## 🧩 Field Extraction Logic (in brief)

`biz.py` runs the OCR output through simple rule-based classification:
- Text with `+` or digit/hyphen patterns → **Contact number**
- Text containing `@` and `.com` → **Email**
- Text containing `www` → **Website**
- Text containing `Tamil Nadu`/digits → **Pincode**
- Text starting with a letter → **Company name**
- Everything else → **Address**

This heuristic works well for standard Indian business card layouts but may need tuning for other formats.

---

## 📌 Known Limitations

- Field-parsing rules are heuristic (regex-based), not ML-based, so unusual card layouts may misclassify fields.
- Login usernames/roles are hardcoded (only `admin` and `manager`).
- `database.py` and `db1.py` are leftover prototyping scripts with hardcoded local credentials — safe to ignore or remove for production use.
- No automated tests included.

## 🔮 Possible Improvements

- Replace heuristic field parsing with an NLP/NER-based extractor.
- Support multiple languages in EasyOCR (currently English only).
- Add role-based multi-user support with a proper user management table.
- Add pagination/search for large customer databases.
- Add unit tests and CI.

---

## 👤 Author

**Akellesh Vasudevan**
- LinkedIn: [linkedin.com/in/akellesh](https://www.linkedin.com/in/akellesh/)
- GitHub: [github.com/Akellesh](https://github.com/Akellesh)

## 📄 License

No license file is currently included in this repository. Add one (e.g., MIT) if you intend for others to reuse this code.
