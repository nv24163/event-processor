# event-processor
Cloud Event Processor

A Python-based serverless-style event processor that simulates cloud function behavior.
The system validates, normalizes, and processes event-driven JSON inputs and returns structured JSON responses.

This project mimics how backend cloud functions (such as AWS Lambda) handle real-world events like user signups, payments, and file uploads.

📌 Project Overview

The handler processes three types of events:

USER_SIGNUP

PAYMENT

FILE_UPLOAD

For each event, the system:

Validates required fields

Normalizes input values

Applies business logic

Returns a clean, structured JSON response

📂 Project Structure
cloud-event-processor/
│
├── handler.py
├── run_local.py
├── run_proof.txt
│
├── events/
│   ├── 01_user_signup_valid.json
│   ├── 02_payment_valid.json
│   ├── 03_file_upload_valid.json
│   ├── 04_user_signup_invalid_email.json
│   ├── 05_payment_invalid_amount.json
│   ├── 06_file_upload_invalid_missing_field.json
│   ├── 07_unknown_type.json
│   └── 08_bad_json_structure.json
│
└── expected_outputs/
    ├── 01_user_signup_expected.json
    ├── 02_payment_expected.json
    └── 03_file_upload_expected.json

🚀 Supported Event Types
1️⃣ USER_SIGNUP

Required fields

user_id (int)

email (valid email string)

plan (free, pro, edu)

Normalization

Email → lowercase

Plan → lowercase

Additional Output

welcome_email_subject

2️⃣ PAYMENT

Required fields

payment_id (str)

user_id (int)

amount (> 0)

currency (BHD, USD, EUR)

Normalization

Currency → uppercase

Amount → rounded to 3 decimals

Calculations

Fee = 2% of amount

Net amount = amount - fee

Both rounded to 3 decimals

3️⃣ FILE_UPLOAD

Required fields

file_name (str)

size_bytes (int ≥ 0)

bucket (str)

uploader (valid email)

Normalization

Bucket → lowercase

File name → stripped of spaces

Uploader → lowercase

Storage Class Rules

< 1,000,000 → STANDARD

< 50,000,000 → STANDARD_IA

Otherwise → GLACIER

▶️ How to Run Locally

Run all events:

python run_local.py --all


Run a single event:

python run_local.py --file events/01_user_signup_valid.json

📤 Output Format

All responses follow this structure:

Success Response
{
  "status": "ok",
  "message": "Processed successfully",
  "data": { ... },
  "errors": []
}

Error Response
{
  "status": "error",
  "message": "Validation failed",
  "data": {},
  "errors": ["error details"]
}

🧠 What This Project Demonstrates

Input validation

Data normalization

Business logic implementation

JSON serialization

Event-driven backend design

Local testing of cloud-style handlers

📎 Submission Contents

handler.py

run_proof.txt (contains execution proof for valid and invalid events)

🛠 Technologies Used

Python 3

JSON

Command-line execution
