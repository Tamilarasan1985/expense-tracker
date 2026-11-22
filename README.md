# Provision vs. Food AI Expense Tracker

A lightweight, single-file mobile web app designed to track daily expenses, automatically categorize items using AI, calculate the spending difference between **Provision** (groceries/essentials) and **Food** (dining/delivery), and back up records to **Google Sheets**.

## ✨ Features

* **AI Text & Receipt Scanning:** Type expenses in plain English (e.g., *"Rice 350, milk 60, Zomato biryani 280"*) or snap a photo of a bill using your phone's camera.
* **Provision vs. Food Difference:** Automatically calculates total spending in Indian Rupees (`₹`) and highlights the exact spending difference between home groceries and outside food.
* **Expanded Categories:** Sorts items into `Provision`, `Food`, `Bills`, `Travel`, `Medical`, and `Other`.
* **Monthly Filter:** View all-time totals or filter expenses by month.
* **Offline Local Storage:** Saves all expenses and API keys privately inside your browser's `localStorage`.
* **Google Sheets Backup & CSV Export:** One-tap cloud sync to Google Sheets via Apps Script, plus offline `.csv` download.
* **PIN Lock Protection:** Optional 4-digit PIN lock screen for quick privacy on mobile devices.

## 🚀 Setup Instructions

1. **Open the App:** Launch your GitHub Pages link on your mobile browser and tap **Add to Home Screen** to install it as an app.
2. **Configure Keys:** Tap **⚙️ Setup Keys** in the top-right corner:
   * **Gemini API Key:** Get a free key from [Google AI Studio](https://google.com) to enable AI text parsing and camera bill scanning.
   * **Google Sheets Web App URL:** Paste your deployed Google Apps Script URL to enable cloud backup.
3. **Start Tracking:** Enter an expense or scan a receipt, then tap **✨ AI Add**.

## 📊 Google Sheets Backup Script

To enable cloud backup, create a Google Sheet with the headers **`Date` | `Month` | `Item` | `Category` | `Amount`**, open **Extensions → Apps Script**, and deploy this code as a Web App (`Execute as: Me`, `Who has access: Anyone`):

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const rows = JSON.parse(e.postData.contents);
  
  rows.forEach(row => {
    sheet.appendRow([row.date, row.month, row.item, row.category, row.amount]);
  });

  return ContentService.createTextOutput(JSON.stringify({ status: "success" }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

## 🔒 Privacy Note

All financial entries, PIN settings, and API keys are stored strictly in your device's local browser storage (`localStorage`) and your personal Google Sheet. No data is sent to any external server other than the official Gemini API and your own Google Apps Script endpoint.
