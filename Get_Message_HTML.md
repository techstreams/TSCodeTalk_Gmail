# TSCodeTalk > Gmail > Get Message HTML

This function retrieves the HTML of a Gmail message and writes it to a Google Doc.

---

```js
/**
 * Get email html
 */
function getGmailHtml() {
  
  const gmailId, msg, txt;
  
  msg = GmailApp.search('from:somebody@domain.com)[0].getMessages()[0];
  
  gmailId = msg.getId();
  
  txt = msg.getBody();
  
  doc = DocumentApp.getActiveDocument().getBody();
  doc.appendParagraph('Gmail ID: ' + gmailId);
  doc.appendParagraph(txt);
  
}
```
