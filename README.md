# Provision vs. Food Hybrid Expense Tracker

A lightweight, single-file mobile web app designed to track daily expenses, automatically categorize items using either **Gemini AI** or **100% Offline Smart Keyword Detection**, calculate the spending difference between **Provision** (groceries/essentials) and **Food** (dining/delivery), and back up records to **Google Sheets**.

## ✨ Features

* **Hybrid Input Modes (AI + Offline):**
  * **✨ AI Add & 📷 Scan Bill:** Uses the Google Gemini API (`gemini-2.5-flash`) to read camera receipt photos or parse natural sentences into structured items.
  * **⚡ Offline Add (Zero Quota / No Internet Needed):** Works 100% offline using a built-in Indian grocery & food keyword classifier plus a manual category selector (`Auto-Detect`, `Provision`, `Food`, `Bills`, `Travel`, `Medical`, `Other`).
* **Provision vs. Food Difference:** Automatically calculates totals in Indian Rupees (`₹`) and highlights the exact spending difference between home groceries and outside food.
* **Monthly Filter:** View all-time totals or filter expenses by month (`MMM YYYY`).
* **Local Storage Persistence:** Saves all expenses, API keys, and Google Sheet URLs privately inside your device's browser (`localStorage`).
* **Google Sheets Backup & CSV Export:** One-tap cloud sync to Google Sheets via Apps Script, plus offline `.csv` download for Microsoft Excel.

## 🚀 Setup & Usage

1. **Open on Mobile:** Visit your live GitHub Pages link (`https://<your-username>.github.io/expense-tracker/`) and tap **Add to Home Screen** in Chrome or Safari.
2. **Configure Keys (Optional):** Tap **⚙️ Setup Keys** in the top-right corner:
   * **Gemini API Key:** Optional — get a free key from [Google AI Studio](https://google.com) to unlock **✨ AI Add** and **📷 Scan Bill**.
   * **Google Sheets Web App URL:** Optional — paste your deployed Google Apps Script URL to enable **☁️ Backup Sheets**.
3. **Log Expenses:**
   * Type items (e.g., `Rice 350, milk 60, biryani 280`) and tap **⚡ Offline Add** anytime without internet or API limits.
   * Or snap a receipt photo / type messy text and tap **✨ AI Add**.

## 📊 Google Sheets Backup Script

To enable cloud backup, create a Google Sheet with the headers **`Date` | `Month` | `Item` | `Category` | `Amount`**, go to **Extensions → Apps Script**, and deploy this code as a Web App (`Execute as: Me`, `Who has access: Anyone`):

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

## 🔒 Privacy

All expenses, API keys, and spreadsheet links stay strictly in your mobile browser's `localStorage` and your private Google Sheet—nothing is stored on GitHub.
