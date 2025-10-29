---
Persisting Data in Google Sheet
---

###### Set up a google sheet
* This (tutorial)[https://github.com/levinunnink/html-form-to-google-sheet]
* This (tutorial)[https://github.com/jamiewilson/form-to-google-sheets/blob/master/README.md]
* Create a new google sheet, with the header being in `Date, Email, Name`
* Create a google app script
```js
// Replace the below script 
// Original code from https://github.com/jamiewilson/form-to-google-sheets
// Updated for 2021 and ES6 standards

const sheetName = 'Sheet1'
const scriptProp = PropertiesService.getScriptProperties()

function initialSetup () {
  const activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function myFunction (e) {
  const lock = LockService.getScriptLock()
  lock.tryLock(10000)

  try {
    const doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    const sheet = doc.getSheetByName(sheetName)

    const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
    const nextRow = sheet.getLastRow() + 1

    const newRow = headers.map(function(header) {
      return header === 'Date' ? new Date() : e.parameter[header]
    })

    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  finally {
    lock.releaseLock()
  }
}
```

```js
// Original code from https://github.com/jamiewilson/form-to-google-sheets
// Updated for 2021 and ES6 standards

const sheetName = 'Sheet1'
const scriptProp = PropertiesService.getScriptProperties()

function initialSetup () {
  const activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}

function doPost (e) {
  const lock = LockService.getScriptLock()
  lock.tryLock(10000)

  try {
    const doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    const sheet = doc.getSheetByName(sheetName)

    const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
    const nextRow = sheet.getLastRow() + 1

    const newRow = headers.map(function(header) {
      return header === 'Date' ? new Date() : e.parameter[header]
    })

    sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
      .setMimeType(ContentService.MimeType.JSON)
  }

  finally {
    lock.releaseLock()
  }
}

```

###### Run the initialSetup Function

###### Add Trigger for the script

###### Publish the project

###### Configure your HTML Form

```bash
https://script.google.com/macros/s/AKfycbz_kJgWq2paaMQgUnK7RdeBnqP7sJbg1UJBIPYbjqVDBA5h_0qxAzFZisJehUfLjPNv/exec

Deployment ID: AKfycbygvTSL7RfKiODxO-dJqyv84Bz58Eayp_KckeEYaqe2DgLvxbP2uNu5F704HOxgDAfABQ

Web App URL: https://script.google.com/macros/s/AKfycbygvTSL7RfKiODxO-dJqyv84Bz58Eayp_KckeEYaqe2DgLvxbP2uNu5F704HOxgDAfABQ/exec
```