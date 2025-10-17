# Dwilson Haircuts Website - Complete Setup Guide

## 📋 Project Overview

A complete, professional website for Dwilson Haircuts barbershop featuring:
- Modern, responsive design with smooth animations
- Online booking system with Firebase integration
- Payment receipt upload functionality
- Separate admin portal for managing bookings
- Contact form with Firebase storage
- Services and products showcase pages

## 🗂️ File Structure

```
Dwilson Haircuts/
├── index.html          # Homepage
├── services.html       # Services page
├── products.html       # Products page
├── book.html          # Online booking page
├── contact.html       # Contact page
└── admin.html         # Admin portal (password-protected)
```

## 🔥 Firebase Setup Instructions

### Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add Project"
3. Enter project name: `Dwilson Haircuts`
4. Disable Google Analytics (optional)
5. Click "Create Project"

### Step 2: Enable Firebase Services

#### A. Realtime Database
1. In Firebase Console, go to "Build" → "Realtime Database"
2. Click "Create Database"
3. Select location (choose closest to Nigeria)
4. Start in **Test Mode** (we'll secure it later)
5. Click "Enable"

#### B. Firebase Storage
1. Go to "Build" → "Storage"
2. Click "Get Started"
3. Start in **Test Mode**
4. Click "Done"

### Step 3: Get Firebase Configuration

1. In Firebase Console, click the gear icon → "Project settings"
2. Scroll down to "Your apps"
3. Click the web icon `</>`
4. Register your app (name: "Dwilson Haircuts Website")
5. Copy the `firebaseConfig` object

### Step 4: Update Your HTML Files

Replace the Firebase configuration in these files:
- `book.html`
- `contact.html`
- `admin.html`

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY_HERE",
    authDomain: "your-project.firebaseapp.com",
    databaseURL: "https://your-project.firebaseio.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

## 🔒 Firebase Security Rules

### Realtime Database Rules

Go to Firebase Console → Realtime Database → Rules tab and replace with:

```json
{
  "rules": {
    "bookings": {
      ".read": false,
      ".write": true,
      "$bookingId": {
        ".read": false,
        ".write": true,
        ".validate": "newData.hasChildren(['clientName', 'clientEmail', 'clientPhone', 'serviceType', 'appointmentDate', 'appointmentTime', 'locationType', 'timestamp', 'status'])"
      }
    },
    "contacts": {
      ".read": false,
      ".write": true,
      "$contactId": {
        ".read": false,
        ".write": true,
        ".validate": "newData.hasChildren(['name', 'email', 'subject', 'message', 'timestamp', 'status'])"
      }
    }
  }
}
```

### Storage Rules

Go to Firebase Console → Storage → Rules tab and replace with:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /receipts/{fileName} {
      // Allow uploads up to 5MB
      allow write: if request.resource.size < 5 * 1024 * 1024
                   && request.resource.contentType.matches('image/.*|application/pdf');
      allow read: if true;
    }
  }
}
```

## 📊 Database Structure

### Bookings Node
```json
{
  "bookings": {
    "uniqueBookingId": {
      "clientName": "John Doe",
      "clientEmail": "john@example.com",
      "clientPhone": "+234XXXXXXXXXX",
      "serviceType": "Fade Haircut",
      "appointmentDate": "2025-10-15",
      "appointmentTime": "14:00",
      "additionalNotes": "Optional notes",
      "locationType": "in-shop or home-service",
      "serviceAddress": "Address if home-service",
      "paymentStatus": "Paid or Pay Later",
      "receiptUploaded": true/false,
      "receiptFileName": "filename.jpg",
      "receiptURL": "https://...",
      "timestamp": "2025-10-06T10:30:00.000Z",
      "status": "Pending"
    }
  }
}
```

### Contacts Node
```json
{
  "contacts": {
    "uniqueContactId": {
      "name": "Jane Smith",
      "email": "jane@example.com",
      "phone": "+234XXXXXXXXXX",
      "subject": "General Inquiry",
      "message": "Contact message here...",
      "timestamp": "2025-10-06T10:30:00.000Z",
      "status": "Unread"
    }
  }
}
```

## 🔐 Admin Portal Access

### Default Admin Password
The admin portal (`admin.html`) is protected by a password.

**Default Password:** `Dwilson2025`

**To Change:**
1. Dwilson `admin.html`
2. Find line: `const ADMIN_PASSWORD = "Dwilson2025";`
3. Change to your desired password
4. Save the file

### Accessing Admin Portal
1. Navigate directly to `admin.html` (no links from main site)
2. Enter admin password
3. View and manage all bookings and contacts

## 💳 Payment Configuration

### Bank Account Details
Update the bank details in `book.html`:

```html
<div class="bank-details">
    <p><strong>Bank Name:</strong> <span>Your Bank Name</span></p>
    <p><strong>Account Name:</strong> <span>Your Business Name</span></p>
    <p><strong>Account Number:</strong> <span>Your Account Number</span></p>
</div>
```

## 🎨 Customization Guide

### Colors
Update CSS variables in each HTML file:

```css
:root {
    --primary-color: #C9A55B;     /* Gold accent */
    --secondary-color: #2C2C2C;   /* Dark grey */
    --accent-color: #1a1a1a;      /* Darker grey */
    --light-bg: #f8f8f8;          /* Light background */
    --text-dark: #333;            /* Text color */
}
```

### Business Information
Update in footer and contact sections:
- Phone number: `+234 XXX XXX XXXX`
- Email: `info@Dwilson Haircuts.com`
- Address: Update to your actual location
- Social media links

### Business Hours
Update in footer:
```html
<li>Monday - Friday: 9:00 AM - 7:00 PM</li>
<li>Saturday: 9:00 AM - 8:00 PM</li>
<li>Sunday: 10:00 AM - 5:00 PM</li>
```

### Service Prices
Update prices in `services.html`:
```html
<div class="service-price">From ₦3,500</div>
```

## 🚀 Deployment Options

### Option 1: Firebase Hosting (Recommended)
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase in your project folder
firebase init hosting

# Deploy
firebase deploy
```

### Option 2: Netlify
1. Create account at [Netlify](https://www.netlify.com/)
2. Drag and drop your folder
3. Site will be live instantly

### Option 3: Traditional Hosting
Upload all HTML files to your web hosting via FTP

## ⚠️ Important Notes

### Receipt Upload Limitation
The current implementation stores receipt information in Firebase, but actual file upload requires:
- Firebase Storage (configured above)
- Files are uploaded to `/receipts/` folder
- Maximum file size: 5MB
- Accepted formats: JPG, PNG, PDF

### Browser Compatibility
- Tested on: Chrome, Firefox, Safari, Edge
- Mobile responsive on all devices
- Requires JavaScript enabled

### Security Considerations
1. **Change the admin password** immediately
2. Never share `admin.html` URL publicly
3. Keep Firebase config keys secure
4. Regularly backup your Firebase database
5. Monitor Firebase usage to stay within free tier limits

## 📱 Firebase Free Tier Limits

- **Realtime Database:** 1GB storage, 10GB/month downloads
- **Storage:** 5GB storage, 1GB/day downloads
- **Hosting:** 10GB storage, 360MB/day bandwidth

For most small businesses, these limits are more than sufficient.

## 🆘 Troubleshooting

### Bookings Not Saving
1. Check Firebase configuration is correct
2. Verify Realtime Database is enabled
3. Check browser console for errors
4. Ensure Database Rules allow writes

### Receipt Upload Fails
1. Verify Firebase Storage is enabled
2. Check Storage Rules allow writes
3. Ensure file size is under 5MB
4. Check file format (JPG, PNG, PDF only)

### Admin Portal Won't Login
1. Verify password is correct
2. Check browser console for errors
3. Ensure Firebase config is correct

## 📞 Support

For technical issues with Firebase:
- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase Support](https://firebase.google.com/support)

## 📄 License

This is a custom-built website for Dwilson Haircuts. All rights reserved.

---

**Built with:** HTML5, CSS3, JavaScript, Bootstrap 5, Firebase, AOS Animation Library

**Version:** 1.0.0

**Last Updated:** October 2025