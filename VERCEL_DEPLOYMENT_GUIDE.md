# 🚀 Vercel Deployment Guide - Firebase Authentication Fix

## Problem: "auth/unauthorized" Error on Google Sign-In

When deploying to Vercel, you may encounter `auth/unauthorized` or `auth/unauthorized-domain` errors when trying to sign in with Google. This happens because Firebase needs to know which domains are allowed to use authentication.

---

## ✅ Solution: Configure Firebase Authorized Domains

### Step 1: Add Your Vercel Domain to Firebase

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Navigate to **Authentication** → **Settings** tab
4. Scroll down to **Authorized domains** section
5. Click **Add domain**
6. Add your Vercel domain(s):
   - Your production domain: `your-app.vercel.app`
   - Your custom domain (if you have one): `yourdomain.com`
   - Click **Add**

**Note:** Firebase automatically includes:
- `localhost` (for local development)
- `your-project.firebaseapp.com`
- `your-project.web.app`

But you **must manually add** your Vercel deployment domain!

---

### Step 2: Configure Environment Variables in Vercel

1. Go to your Vercel project dashboard
2. Navigate to **Settings** → **Environment Variables**
3. Add all required Firebase environment variables:

```
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id
```

4. **Important:** Make sure to set these for:
   - ✅ **Production**
   - ✅ **Preview** (optional, but recommended)
   - ✅ **Development** (optional)

5. Click **Save**

---

### Step 3: Verify Google Sign-In is Enabled

1. In Firebase Console, go to **Authentication** → **Sign-in method**
2. Find **Google** in the list
3. Click on it
4. Make sure **Enable** toggle is **ON**
5. Enter a **Project support email** (required)
6. Click **Save**

---

### Step 4: Redeploy Your Vercel Application

After making changes:

1. **Option A:** Push a new commit to trigger automatic deployment
   ```bash
   git commit --allow-empty -m "Trigger redeploy for Firebase config"
   git push
   ```

2. **Option B:** Manually redeploy in Vercel dashboard
   - Go to **Deployments** tab
   - Click the **⋯** menu on latest deployment
   - Select **Redeploy**

---

## 🔍 Troubleshooting

### Still Getting "auth/unauthorized" Error?

1. **Check the exact domain:**
   - Open your Vercel app in browser
   - Check the URL in address bar
   - Make sure this **exact domain** is in Firebase authorized domains
   - Include both `your-app.vercel.app` AND any custom domains

2. **Verify environment variables:**
   - In Vercel, go to **Settings** → **Environment Variables**
   - Confirm all `VITE_FIREBASE_*` variables are set
   - Make sure `VITE_FIREBASE_AUTH_DOMAIN` matches your Firebase project

3. **Check Firebase Console:**
   - Go to **Authentication** → **Settings** → **Authorized domains**
   - Verify your Vercel domain appears in the list
   - It may take a few minutes to propagate

4. **Clear browser cache:**
   - Sometimes cached auth state can cause issues
   - Try incognito/private browsing mode
   - Clear cookies for your domain

5. **Check browser console:**
   - Open Developer Tools (F12)
   - Look for specific error codes
   - The improved error messages should now show the exact domain that needs to be added

---

## 📝 Quick Checklist

Before deploying to Vercel, ensure:

- [ ] Firebase project created
- [ ] Google Sign-In enabled in Firebase Console
- [ ] Firestore database created
- [ ] Firestore security rules deployed
- [ ] All environment variables set in Vercel
- [ ] Vercel domain added to Firebase authorized domains
- [ ] Custom domain (if any) added to Firebase authorized domains
- [ ] Application redeployed after configuration changes

---

## 🎯 Common Domain Patterns

Your Vercel domain will typically be one of:
- `your-app-name.vercel.app` (default Vercel domain)
- `your-app-name-git-main-your-team.vercel.app` (preview deployments)
- `your-custom-domain.com` (if you added a custom domain)

**Add ALL of these to Firebase authorized domains!**

---

## 💡 Pro Tips

1. **Use wildcard domains (if supported):**
   - Some Firebase projects allow `*.vercel.app` pattern
   - Check Firebase Console for this option

2. **Test in preview deployments:**
   - Vercel creates unique preview URLs for each PR
   - You may need to add these individually, or use a wildcard

3. **Monitor Firebase Console:**
   - Check **Authentication** → **Users** to see if sign-ins are working
   - Check browser console for detailed error messages

4. **Environment variable naming:**
   - Vite requires `VITE_` prefix for client-side variables
   - Don't forget the prefix when setting in Vercel!

---

## 🔗 Useful Links

- [Firebase Console](https://console.firebase.google.com/)
- [Vercel Environment Variables Docs](https://vercel.com/docs/concepts/projects/environment-variables)
- [Firebase Auth Domain Configuration](https://firebase.google.com/docs/auth/web/start#set_the_authentication_domain)

---

## ✅ After Configuration

Once configured correctly:
1. The error message will now show the exact domain that needs authorization
2. Google Sign-In should work seamlessly
3. Users can authenticate from your Vercel deployment

If you still encounter issues, check the browser console for the specific error code and message, which will help identify the exact problem.

