# CardSnap — Business Card Scanner
### Internship Project · Ulavi Technologies

A PWA (Progressive Web App) that scans business cards, extracts contact info using Claude AI Vision, syncs to Zoho CRM, and auto-sends WhatsApp + Email — even offline.

---

## 🚀 Quick Start (Week 1–2)

### Option A: Run locally (no server needed)
1. Download all 3 files: `index.html`, `manifest.json`, `sw.js`
2. Open `index.html` in Chrome on your phone or laptop
3. The app works immediately — OCR uses a mock demo until you add your API keys

### Option B: Deploy online (recommended for team testing)
Upload to any of these free hosts:
- **Netlify**: Drag-and-drop the folder at netlify.com/drop
- **Vercel**: Run `npx vercel` in the folder
- **GitHub Pages**: Push to a repo, enable Pages in Settings
- **Firebase Hosting**: `firebase deploy` after `firebase init hosting`

After deploying, open the URL on your phone → tap "Add to Home Screen" = it becomes a native-like app.

---

## 🔑 API Keys Setup (Configuration screen in-app)

Tap the ⚙️ gear icon in the app header to open Configuration.

### 1. Claude AI (OCR — Card Reading)
- Go to: https://console.anthropic.com
- Create an API key
- Paste it in **Anthropic API Key** field
- ✅ This powers the card text extraction

### 2. Zoho CRM
- Go to: https://api-console.zoho.com
- Create a Self Client
- Scope required: `ZohoCRM.modules.Leads.CREATE,ZohoCRM.modules.Leads.READ`
- Generate an access token
- Paste in **Zoho OAuth Access Token**
- ⚠️ Zoho tokens expire — for production use OAuth 2.0 refresh token flow

### 3. WhatsApp Business (Meta Cloud API)
- Go to: https://developers.facebook.com
- Create an app → Add "WhatsApp" product
- Get your **Access Token** and **Phone Number ID**
- Create and get approval for a **message template** (e.g. template name: `nice_meeting_you`)
  - Template body example: `Hi {{1}}, great meeting you at {{2}}! Let's stay in touch.`
  - Parameters: {{1}} = name, {{2}} = event name
- Paste credentials in WhatsApp section of config

### 4. Email (EmailJS — no backend needed)
- Go to: https://www.emailjs.com (free tier: 200 emails/month)
- Create account → Add Email Service (Gmail, Outlook, etc.)
- Create an Email Template with variables:
  - `{{to_name}}`, `{{to_email}}`, `{{contact_company}}`, `{{event_name}}`
- Copy Service ID, Template ID, and Public Key
- Paste in Email section of config

---

## 📱 Features Checklist

| Feature | Status |
|---------|--------|
| 📷 Photo upload / camera capture | ✅ |
| 🤖 AI OCR extraction (Claude Vision) | ✅ (mock if no key) |
| ✏️ Review & edit form | ✅ |
| 💾 Save contacts locally | ✅ |
| 🗃️ Zoho CRM sync | ✅ |
| 💬 WhatsApp auto-message | ✅ |
| 📧 Email auto-send (EmailJS) | ✅ |
| 📴 Offline queue (auto-retry on reconnect) | ✅ |
| ⚠️ Duplicate detection | ✅ |
| 🔍 Contact search & filter | ✅ |
| 📊 Stats dashboard | ✅ |
| 📱 PWA / installable on phone | ✅ |

---

## 🏗️ Architecture

```
index.html (single file PWA)
├── Screen 1: Scan        → camera/file upload → Claude Vision OCR
├── Screen 2: Review      → editable form + duplicate check
├── Screen 3: Contacts    → searchable list with queue status
│
├── Services:
│   ├── Claude API        → /v1/messages with vision (OCR)
│   ├── Zoho CRM API      → /crm/v2/Leads (POST)
│   ├── WhatsApp API      → graph.facebook.com (Cloud API)
│   └── EmailJS           → browser-side email (no backend)
│
├── Offline:
│   ├── LocalStorage      → contacts + queue persistence
│   ├── Service Worker    → app shell caching
│   └── Queue flush       → auto-retry on navigator.onLine
```

---

## 👥 Team Role Assignments

| Role | Files/Functions to own |
|------|------------------------|
| **App Builder** (Vibe Coding lead) | `index.html` overall, UI polish |
| **App Flow / Data Design** | `saveContact()`, `checkDuplicate()`, LocalStorage schema |
| **Offline Queue** | `queueMessages()`, `tryFlushQueue()`, `sw.js` |
| **WhatsApp + Email** | `sendWhatsApp()`, `sendEmail()`, template config |
| **Testing + Demo** | End-to-end test script, offline scenario, presentation |

---

## 🧪 Testing Checklist (Week 4)

### Functional tests
- [ ] Scan a real business card with camera
- [ ] Scan a card as uploaded image
- [ ] All 5 fields extracted correctly (or fixable)
- [ ] Duplicate detected when same phone scanned twice
- [ ] Contact saved and appears in list
- [ ] WhatsApp message received on test number
- [ ] Email received in inbox

### Offline queue test
1. Turn OFF WiFi + mobile data
2. Scan and save a card
3. Verify status shows "QUEUED" (yellow badge)
4. Turn WiFi back ON
5. Wait ~5 seconds
6. Verify status changes to "SENT" automatically ✅

### CRM test
- [ ] Saved contact appears as Lead in Zoho CRM
- [ ] "CRM" badge shown on contact card in app

---

## 📦 Tech Stack

- **Frontend**: Vanilla HTML/CSS/JS (no framework — runs everywhere)
- **AI OCR**: Claude claude-opus-4-5 Vision API
- **CRM**: Zoho CRM REST API v2
- **Messaging**: WhatsApp Business Cloud API (Meta)
- **Email**: EmailJS (browser-side, no backend)
- **Storage**: localStorage (offline-first)
- **PWA**: Service Worker + Web App Manifest
- **Fonts**: Syne + DM Sans (Google Fonts)

---

## 🔒 Security Notes

- API keys stored in localStorage — acceptable for internal team app
- For production: move keys to a backend/proxy server
- Zoho token refresh should be automated for long-term use
- WhatsApp template must be pre-approved by Meta before use

---

*Built as part of Ulavi Technologies internship program.*
*All code and materials are the intellectual property of Ulavi Technologies.*
