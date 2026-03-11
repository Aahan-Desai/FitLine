# 🔧 Troubleshooting: "Mark as Done" Error

## Common Causes & Solutions

### 1. Firestore Security Rules Not Deployed
**Most Common Issue**

**Problem:** Firestore rules are blocking writes.

**Solution:**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Go to **Firestore Database** → **Rules** tab
4. Deploy the rules from `firestore.rules` file:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      match /completions/{completionId} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

5. Click **Publish**

**Or use Firebase CLI:**
```bash
firebase deploy --only firestore:rules
```

---

### 2. Firebase Not Initialized
**Problem:** Missing or incorrect Firebase configuration.

**Solution:**
1. Check `.env.local` file exists
2. Verify all Firebase keys are correct:
   ```env
   VITE_FIREBASE_API_KEY=your_key
   VITE_FIREBASE_AUTH_DOMAIN=your_domain
   VITE_FIREBASE_PROJECT_ID=your_project_id
   # ... etc
   ```
3. Restart dev server after adding/changing `.env.local`

---

### 3. User Not Authenticated
**Problem:** User session expired or not logged in.

**Solution:**
1. Check browser console for auth errors
2. Try logging out and logging back in
3. Clear browser cache and cookies
4. Check if `user` object exists in AuthContext

---

### 4. Network/Firebase Connection Issues
**Problem:** Cannot reach Firebase servers.

**Solution:**
1. Check internet connection
2. Check Firebase status: https://status.firebase.google.com/
3. Try in incognito mode (to rule out extensions)
4. Check browser console for network errors

---

### 5. Date Key Format Issues
**Problem:** Date key format mismatch.

**Solution:**
The code now uses consistent ISO format: `YYYY-MM-DD`
This should work automatically, but if issues persist:
- Check browser timezone settings
- Verify date calculations in console

---

## Debug Steps

1. **Open Browser Console** (F12)
2. **Click "Mark as Done"**
3. **Check Console for Error:**
   - Look for Firebase error messages
   - Check for permission errors
   - Note the exact error code

4. **Check Network Tab:**
   - Look for failed requests to Firestore
   - Check response status codes
   - Look for 403 (permission denied) or 404 (not found)

5. **Verify Firebase Setup:**
   ```javascript
   // In browser console
   console.log(import.meta.env.VITE_FIREBASE_PROJECT_ID);
   // Should show your project ID, not undefined
   ```

---

## Quick Fix Checklist

- [ ] Firestore rules deployed
- [ ] `.env.local` file exists with correct values
- [ ] User is logged in (check AuthContext)
- [ ] Internet connection is active
- [ ] Firebase project is active (not deleted/paused)
- [ ] Browser console shows no blocking errors
- [ ] Try in incognito mode

---

## Still Not Working?

1. **Check the exact error message** in the toast notification
2. **Check browser console** for detailed error
3. **Verify Firestore rules** are deployed
4. **Test with Firebase Console** - try writing manually to verify rules work

The improved error handling will now show more specific error messages to help diagnose the issue.
