# TSCodeTalk > Gmail > Save Gmail Attachments to Google Drive


This script saves Gmail attachments to Google Drive.

---

```javascript
function saveAttachmentsToGoogleDrive() { 
  const driveFolder = "Gmail Attachments";
  const threads = GmailApp.search("has:attachment newer_than:1d"); 
  const folder = DriveApp.getFoldersByName(driveFolder);
  
  if (folder.hasNext()) {
    folder = folder.next();
  } else {
    folder = DriveApp.createFolder(driveFolder);
  }
  
  threads.forEach(thread => {
    thread.getMessages().forEach(message {
      const desc = message.getSubject() + " #" + message.getId();
      message.getAttachments().forEach(att => {
        folder.createFile(att).setDescription(desc);
      });  // end attachments.forEach()
    }); // end messages.forEach()
  }); // end threads.forEach()
}
```
