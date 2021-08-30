This code can be used to cleanup email from a Google Sheet.

---

```js
/*
 * Copyright Laura Taylor
 * (https://github.com/techstreams
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in all
 * copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 * SOFTWARE.
 */


/** 
 * This function creates a menu in the toolbar
*/
function onOpen() {
  const ui = SpreadsheetApp.getUi().createMenu('Gmail Menu')
      .addItem('Archive Email', 'archiveEmail')
      .addItem('Delete Email', 'deleteEmail')
      .addItem('Add Email Label', 'addLabel')
      .addItem('Remove Email Label', 'removeLabel')
      .addToUi();
}


/** 
 * This function archives email
*/
function archiveEmail() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheets()[0];
  const searchTerm = sheet.getRange("A1").getValue();
  GmailApp.search(searchTerm).map(thread => thread.moveToArchive()); 
}


/** 
 * This function deletes email
*/
function deleteEmail() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheets()[0];
  const searchTerm = sheet.getRange("A1").getValue();
  GmailApp.search(searchTerm).map(thread => thread.moveToTrash()); 
}


/** 
 * This function adds a label to email
*/
function addLabel() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheets()[0];
  const searchTerm = sheet.getRange("A1").getValue();
  const l = sheet.getRange("B1").getValue();
  const label = GmailApp.getUserLabelByName(l);
  GmailApp.search(searchTerm).map(thread => thread.addLabel(label)); 
}


/** 
 * This function removes a label from email
*/
function removeLabel() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getSheets()[0];
  const searchTerm = sheet.getRange("A1").getValue();
  const l = sheet.getRange("B1").getValue();
  const label = GmailApp.getUserLabelByName(l);
  GmailApp.search(searchTerm).map(thread => thread.removeLabel(label)); 
}
```
