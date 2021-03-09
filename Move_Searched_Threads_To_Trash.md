# TSCodeTalk > Gmail > Move Searched Threads To Trash


This script searches email and moves any existing threads to trash.

*See [Gmail search operator documentation](https://support.google.com/mail/answer/7190?hl=en) for more information.*

---

```javascript
const searchStr = 'in:inbox from:myaccount@gmail.com older_than:30d';

function moveSearchedThreadsToTrash() {
  // Find messages related to search string and move to trash
  const threads = GmailApp.search(searchStr).forEach(thread => thread.moveToTrash());
}


function logNumSearchedThreads() {
  // Find messages related to search string and log number of matching threads
  Logger.log(GmailApp.search(searchStr).length);
}
```
