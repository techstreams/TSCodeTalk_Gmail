# TSCodeTalk > Gmail > Filter Gmail


This script manages GMail Filters from a Google Sheet.

*Script runs on a daily timer near 8:00 AM.*

---

**SETUP:**

* Complete row of spreadsheet with filter criteria and filter actions.
* Run ***Start*** menu to configure trigger.

:exclamation: ***IMPORTANT: Remember to "add label" so it doesn't show up in search next time!***

---

```javascript
/*
 * Add Sheets Menu
 */
function onOpen() {
  
  SpreadsheetApp.getUi().createMenu('GMail Filters')
  .addItem('🕜 Start', 'start')
  .addItem('❌ Stop', 'deleteExistingClockTriggers')
  .addToUi()
  
}


/*
 * Delete existing clock triggers
 * Configure a new clock trigger every 1 day near 8:00am
 */
function start() {
  deleteExistingClockTriggers();  // Delete Any Existing Time Triggers
  ScriptApp.newTrigger('applyGmailFilters').timeBased().everyDays(1).atHour(8).create();  // Create New Time Trigger Every 1 Day near 8:00AM
}

/*
 * Delete existing clock based triggers
 */
function deleteExistingClockTriggers() {
   ScriptApp.getProjectTriggers().forEach(trigger => {
     if (trigger.getEventType() == ScriptApp.EventType.CLOCK) {
       ScriptApp.deleteTrigger(trigger);
     }
   });
}

/*
 * Apply GMail Filters
 */
function applyGmailFilters() {
  const rules = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Filters')
                              .getDataRange().getValues();  // Get Filter Entries from 'Filter' Sheet
                                                            // continue if we have rules
  if (rules.length > 1) {
    rules.forEach((row, r) => {  // iterate through rows of sheet
      if (r > 0) {  // Skip title row
        if (row[1] && row[1] != '') {  // Check for search value in row else throw error
          const msgs = GmailApp.search(row[1]);
          if (row[2] == 'Yes') { msgs.forEach(msg => msg.moveToArchive()) }  // Archive
          if (row[3] == 'Yes') { msgs.forEach(msg => msg.markRead()) }  // Mark As Read
          if (row[4] == 'Yes') {  // Star It
            msgs.forEach(msg => { 
              if (!msg.hasStarredMessages()) { msg.getMessages()[0].star()}; 
            }); 
          }
          if (row[5] && row[5] != '') {  // Label It
            const label = GmailApp.getUserLabelByName(row[5]);
            if (!label) { label = GmailApp.createLabel(row[5]) };
            msgs.forEach(msg => msg.addLabel(label));
          }
          if (row[6] && row[6] != '') {  // Forward It
            msgs.forEach( msg => msg.getMessages()[0].forward(row[6]) );
          }
          if (row[7] == 'Yes') {  // Remove From Spam
            msgs.forEach( msg => {
              if (msg.isInSpam()) {
                if (row[2] == 'Yes') { msg.moveToArchive() } else { msg.moveToInbox() };
              }
            });
          }
          if (row[8] == 'Yes') {  // Mark As Important
            msgs.forEach( msg => msg.markImportant() );
          }
          
        } else { throw new Error('No GMail Search for Row ' + (r + 1)) }
      }
    });
  }
}
```
