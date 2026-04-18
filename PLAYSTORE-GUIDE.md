# Publishing ASCII Camera to Google Play Store

Your app is a PWA. To publish on the Play Store, you wrap it as an Android app using **TWA (Trusted Web Activity)**. No native code needed.

---

## Step 1: Host Your PWA on HTTPS

Your app must be live on a real HTTPS URL. Free options:

- **GitHub Pages** — push repo, enable Pages in repo Settings
- **Netlify** — drag your folder to https://app.netlify.com/drop
- **Vercel** — run `npx vercel` in your project folder
- **Firebase Hosting** — run `firebase deploy`

After hosting, verify your PWA works by visiting the URL on your phone.

---

## Step 2: Generate App Icons

1. Open `generate-icons.html` in your browser
2. Click the download link under each icon size
3. Save all icons into the `icons/` folder

---

## Step 3: Package as Android App

### Recommended: PWABuilder (no coding)

1. Go to **https://www.pwabuilder.com**
2. Enter your hosted PWA URL
3. It validates your manifest, service worker, and icons
4. Click **"Package for stores"** then select **"Android"**
5. Fill in:
   - Package ID: `com.yourname.asciicamera`
   - App name: `ASCII Camera`
   - Theme/background colors
6. Click **"Generate"** — it downloads a ZIP containing:
   - `.aab` file (Android App Bundle) — this is what you upload to Play Store
   - `assetlinks.json` — you'll need this for Step 5
   - A signing key

---

## Step 4: Upload to Google Play Console

1. Go to **https://play.google.com/console** and create a developer account ($25 one-time fee)
2. Click **"Create app"**:
   - App name: **ASCII Camera**
   - Language: English
   - App type: App
   - Free
3. Fill in the **Store listing**:
   - Short description (80 chars): `Turn your camera into real-time ASCII art with filters`
   - Full description: describe all features (filters, controls, export)
   - Screenshots: take phone screenshots of your app in action
   - Feature graphic: 1024x500 banner
   - Icon: upload `icon-512.png`
4. Complete the **Content rating** questionnaire (camera app, no mature content)
5. Go to **Production** → **Create new release**
6. Upload the `.aab` file from PWABuilder
7. Submit for review (takes 1–3 days)

---

## Step 5: Digital Asset Links

For the app to show fullscreen (no browser URL bar), you must verify domain ownership:

1. PWABuilder gives you an `assetlinks.json` file
2. Create a `.well-known/` folder on your hosted site
3. Place the file at: `https://your-site.com/.well-known/assetlinks.json`
4. Verify it's accessible at that URL

---

## Final File Structure

```
project/
├── index.html
├── manifest.json
├── sw.js
├── generate-icons.html
├── icons/
│   ├── icon-72.png
│   ├── icon-96.png
│   ├── icon-128.png
│   ├── icon-144.png
│   ├── icon-152.png
│   ├── icon-192.png
│   ├── icon-384.png
│   └── icon-512.png
└── .well-known/
    └── assetlinks.json    (added after PWABuilder packaging)
```

---

## Security

- All camera processing happens locally — no data leaves the device
- CSP meta tag blocks XSS and data exfiltration
- `no-referrer` policy prevents URL leaking
- No cookies, no tracking, no analytics, no external API calls
- Camera uses the browser's native permission system
