# vmas-vue-firebase-vue-eat

```
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
			allow read, write: if request.time < timestamp.date(2024, 2, 24);
    }
  }
}

service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
			allow read, write: if request.auth != null
    }
  }
}
```
