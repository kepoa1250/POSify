# POSify Frontend on Vercel + Google Apps Script Backend

## Architecture
- Vercel hosts the static frontend (`index.html`, `config.js`).
- Google Apps Script remains the backend/API.
- Google Sheets remains the database.
- Google Drive remains the product-image/file storage.

Vercel does NOT host or replace the Apps Script backend.

## 1. Configure the Apps Script URL
1. Deploy the Google Apps Script project as a Web App.
2. Set **Execute as: Me**.
3. Set access according to your app's intended users (for public sign-in, typically Anyone, subject to your security design).
4. Copy the deployed Web App URL ending in `/exec`.
5. Open `config.js` and replace:
   `PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_EXEC_URL_HERE`
   with the exact `/exec` URL, keeping the quotes.
6. Save the file.

Do not place passwords, session tokens, service-account keys, or other secrets in `config.js`; it is publicly readable.

## 2. Deploy to Vercel
1. Extract this ZIP.
2. Put the `POSify_Vercel` folder in a GitHub repository (or upload/import the project using Vercel's supported Git workflow).
3. In Vercel choose **Add New → Project** and import that repository.
4. Framework Preset: **Other**.
5. Root Directory: the folder containing `index.html` (if the repository contains this folder at its root, leave it as `./`).
6. Build Command: leave empty.
7. Output Directory: `.`
8. Deploy.

Vercel will provide a public HTTPS URL.

## 3. Test
- Open the Vercel URL.
- Register/login using the Apps Script backend account.
- Test products, sales, and image upload.
- If API requests fail, inspect browser DevTools Console/Network. Apps Script cross-origin behavior or deployment access settings may need adjustment.

## Important CORS note
The frontend uses `fetch()` to call the Apps Script `/exec` URL with a `text/plain` JSON body to avoid a preflight request. Apps Script ContentService may redirect responses to a `googleusercontent.com` URL. If the browser blocks the response due to CORS, the static Vercel frontend cannot fix that by itself; a same-origin server-side proxy or a suitable backend endpoint is required. Do not disable browser security.
