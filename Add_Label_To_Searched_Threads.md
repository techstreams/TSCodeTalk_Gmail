# TSCodeTalk > Gmail > Add Label To Searched Threads


This script searches email and:

1. Creates a label *(if indicated)*
2. Adds the label to searched threads
3. Archives threads *(if indicated)*
4. Marks thread as read *(if indicated)*

*See [Gmail search operator documentation](https://support.google.com/mail/answer/7190?hl=en) for more information.*

---

```javascript
function addLabelToSearchedThreads() {
  const searchStr = 'from:noreply@medium.com subject:-"Stats for your stories" older_than:30d';
  const label = '!Medium/dailydigest';
  const newLabel = true;
  const archive = true;
  const markRead = true;

  // Create new label if 'newLabel' flag indicates true
  if (newLabel) {
    GmailApp.createLabel(label);
  }

  // Get User Label
  const userLabel = GmailApp.getUserLabelByName(label);

  // Add label to searched threads and archives/marks as read if indicated
  GmailApp.search(searchStr).forEach(thread => {
    thread.addLabel(userLabel);
    if (archive) {
      thread.moveToArchive();
    }
    if (markRead) {
      thread.markRead();
    }
  });
}
```
