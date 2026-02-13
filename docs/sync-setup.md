# Cloud Sync Setup

littlemoments uses Google Apps Script as a free, simple cloud backend so both parents can capture and view moments from their own devices.

## How It Works

1. A Google Apps Script acts as a simple JSON store
2. The app pushes data to the script on every change (debounced)
3. The app pulls data when the tab gains focus
4. Conflicts are resolved by timestamp — most recent edit wins
5. Deletions are synced — if either device deletes a moment, it stays deleted

## Setup Steps

### 1. Create a Google Sheet

1. Go to [Google Sheets](https://sheets.google.com) and create a new spreadsheet
2. Name it something like "littlemoments-sync"
3. You won't use the sheet directly — it's just the host for the script

### 2. Open Apps Script

1. In the sheet, go to **Extensions → Apps Script**
2. Delete any existing code in `Code.gs`
3. Paste the following:

```javascript
const SHEET_NAME = 'Data';

function doGet(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME);
    if (!sheet || sheet.getLastRow() < 1) {
      return ContentService.createTextOutput(JSON.stringify({ items: [], tags: [] }))
        .setMimeType(ContentService.MimeType.JSON);
    }
    const raw = sheet.getRange('A1').getValue();
    return ContentService.createTextOutput(raw).setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ error: err.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doPost(e) {
  try {
    const data = JSON.parse(e.postData.contents);
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(SHEET_NAME)
      || SpreadsheetApp.getActiveSpreadsheet().insertSheet(SHEET_NAME);
    sheet.clear();
    sheet.getRange('A1').setValue(JSON.stringify(data));
    return ContentService.createTextOutput(JSON.stringify({ success: true }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ error: err.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

### 3. Deploy

1. Click **Deploy → New deployment**
2. Click the gear icon → select **Web app**
3. Set:
   - **Description**: littlemoments sync
   - **Execute as**: Me
   - **Who has access**: Anyone
4. Click **Deploy**
5. Authorise the script when prompted
6. Copy the **Web app URL** (it starts with `https://script.google.com/macros/s/...`)

### 4. Connect the App

1. Open littlemoments
2. Click **Sync Settings** in the footer
3. Paste the Web app URL
4. Click **Save**
5. The sync indicator should turn green

### 5. Share with Your Partner

Give your partner the same Web app URL. When they paste it into their littlemoments app, both devices will sync to the same data store.

## Troubleshooting

**"Pull failed" or "Push failed"**
- Check that the URL is correct and starts with `https://script.google.com/macros/s/`
- Make sure you deployed with "Anyone" access
- Try redeploying the script

**Data not appearing on the other device**
- Tap the Sync button manually
- Switch tabs and come back (sync triggers on tab focus)
- Check that both devices have the same URL

**"Invalid sync URL"**
- The app only accepts Google Apps Script URLs for security
- Make sure the full URL is pasted including `https://`

## Security Notes

- The Apps Script URL is stored in your browser's localStorage
- Data is transmitted over HTTPS
- The Google Sheet is only accessible to your Google account
- No authentication is required to read/write via the URL — treat it like a shared secret between you and your partner
- For additional security, you can restrict access in the Apps Script settings
