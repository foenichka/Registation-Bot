# Telegram Registration Bot with Google Sheets Sync

A production-ready asynchronous Telegram bot built with **Aiogram 3** and **SQLite3** that automates user registration and handles background synchronization with the **Google Sheets API** via a Service Account.

---

## 🚀 Features

* **User Registration:** Seamlessly collects Full Name (FIO) and phone number. Includes built-in numerical validation for phone inputs.
* **Telegram Metadata:** Automatically captures and binds the user's Telegram username (`usrname`) to their profile.
* **Deep Linking Support:** Features a dedicated registration workflow triggered strictly via a deep link: `t.me/your_bot?start=reg`.
* **Background Synchronization:** Powered by an asynchronous background task (`asyncio`) that periodically (every 5 minutes) checks for new database rows and appends them to a Google Sheet without overwriting old data.
* **Production Security:** Zero risk of credential leaks. All sensitive tokens, API keys, and environment configs are isolated from the Git history using tailored `.gitignore` rules.

---

## 🛠️ Tech Stack

* **Core Framework:** Python 3.11+
* **Bot API:** Aiogram 3.x (Asynchronous)
* **Database:** SQLite3
* **Cloud Integration:** Google Sheets API & Google Drive API
* **Authentication:** Google Service Account (OAuth2)

---

## 📦 Installation & Setup

1. Clone the repository and move into the project directory:
   git clone https://github.com/foenichka/Rate_botik.git
   cd Rate_botik

2. Install all required dependencies from the requirements file:
   pip install -r requirements.txt

3. Create a `.env` file in the root directory of your project and configure your environment variables:
   BOT_T=YOUR_TELEGRAM_BOT_TOKEN
   GSHEET_ID=YOUR_GOOGLE_SPREADSHEET_ID
   GSHEET_DATA_SHEET=Sheet1
   GSHEET_META_SHEET=meta

---

## 🔑 Google Service Account Configuration

To securely connect the bot to your target Google Sheet, you can choose **one of the two supported methods**:

### Method 1: Local JSON File (Recommended for Local Development)
Download your Service Account keys from the Google Cloud Console as a JSON file. Save it to one of these paths (both are automatically ignored by Git):
* `.secrets/cred.json` (Preferred)
* `sheets/cred.json` (Fallback)

### Method 2: Environment Variables (Recommended for Production/Hosting)
Alternatively, you can append your raw Google credentials directly into your local `.env` file:
   GOOGLE_TYPE=service_account
   GOOGLE_PROJECT_ID=your_project_id
   GOOGLE_PRIVATE_KEY_ID=your_private_key_id
   GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
   GOOGLE_CLIENT_EMAIL=your_service_account_email
   GOOGLE_CLIENT_ID=your_client_id
   GOOGLE_AUTH_URI=https://accounts.google.com/o/oauth2/auth
   GOOGLE_TOKEN_URI=https://oauth2.googleapis.com/token
   GOOGLE_AUTH_PROVIDER_X509_CERT_URL=https://www.googleapis.com/0auth2/v1/certs
   GOOGLE_CLIENT_X509_CERT_URL=your_cert_url
   GOOGLE_UNIVERSE_DOMAIN=googleapis.com

---

## ⚡ Running the Bot

To spin up the application, execute:
   python main.py

Upon startup, the script will automatically:
1. Initialize the SQLite database structure inside `database/registrs.db` if it doesn't exist.
2. Launch the long-polling bot client.
3. Start the background sync loop, utilizing Python's native `logging` library instead of raw prints for better production visibility.