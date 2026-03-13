Send to reMarkable — Calibre Plugin
A Calibre InterfaceAction plugin that sends selected books from your Calibre library directly to your reMarkable tablet via the reMarkable cloud API. No USB cable or shared Wi-Fi network required.
Author: David Koepsell  
Minimum Calibre version: 5.0.0  
Platforms: Windows, macOS, Linux
---
Features
Send any EPUB in your Calibre library to your reMarkable with one click
Non-EPUB formats (PDF, MOBI, etc.) are automatically converted to EPUB first using Calibre's built-in converter
Uses the current reMarkable sync15 cloud API — compatible with reMarkable 1 and reMarkable 2
Device token stored locally in Calibre's config; never transmitted anywhere except the reMarkable API
Progress dialog with per-book status and cancel support
---
Installation
Download `SendToRemarkable.zip`
Open Calibre and go to Preferences → Plugins → Load plugin from file
Select the ZIP file and confirm the install
Restart Calibre when prompted
Add the button to your toolbar: Preferences → Toolbars & Menus → Main Toolbar, find Send to reMarkable in the left list, and click the arrow to add it
---
First-Time Setup (Device Registration)
You need to register the plugin as a device with your reMarkable account once. The device token it generates never expires.
Log into my.remarkable.com in your browser
Go to my.remarkable.com/device/browser/connect
You will see an 8-character one-time code (e.g. `abcdefgh`)
In Calibre, go to Preferences → Plugins, find Send to reMarkable, and click Customize
Paste the code into the One-time code field and click Register Device
On success, the stored token field will show a masked token
> **Important:** Each code is single-use. If registration fails for any reason, return to step 2 and generate a new code before trying again.
---
Sending Books
Select one or more books in your Calibre library
Click the Send to reMarkable toolbar button (or find it in the right-click context menu)
If any selected books are not in EPUB format, you will be asked to confirm conversion first
A progress dialog tracks each upload; you can cancel mid-batch
A summary dialog reports successes and any errors
Books appear in the root of your reMarkable library under their Calibre title.
---
Technical Notes
API endpoints used
Purpose	Endpoint
Device registration	`webapp-prod.cloud.remarkable.engineering/token/json/2/device/new`
User token (per session)	`webapp-prod.cloud.remarkable.engineering/token/json/2/user/new`
Upload	`internal.cloud.remarkable.com/doc/v2/files`
The plugin uses Python's `http.client` directly rather than `urllib`, to ensure that HTTP redirects are followed with `POST` intact (Python's `urllib` silently converts `POST` to `GET` on redirect, which causes 405 errors with the reMarkable API).
Upload format
Files are uploaded as raw EPUB bytes with `Content-Type: application/epub+zip` and a base64-encoded JSON metadata header (`rm-meta`), using the sync15 protocol. No ZIP packaging is required.
Token storage
The device token is stored in Calibre's plugin config directory as `plugins/send_to_remarkable.json`. It is only sent to `webapp-prod.cloud.remarkable.engineering` to obtain a short-lived session token for each upload session.
---
Troubleshooting
"Not configured" error on send  
The plugin has no stored device token. Complete the First-Time Setup steps above.
Registration fails with HTTP 405 or 404  
The one-time code has already been consumed (codes are consumed even on failure). Go back to `my.remarkable.com/device/browser/connect` and generate a fresh code.
"Auth failed" / HTTP 308 on send  
This usually means a redirect is not being followed correctly. Make sure you are running the latest version of the plugin.
Book sent successfully but not appearing on tablet  
Sync may take a minute. Pull down on the reMarkable home screen to force a sync. If the tablet is offline, documents will appear once it reconnects.
Non-EPUB conversion fails  
Calibre must have a conversion path for the source format. Check that Calibre can convert the file manually via Convert books before using this plugin.
---
Known Limitations
No folder/collection targeting — books are always sent to the root of the reMarkable library
No duplicate detection — sending the same book twice will create two copies on the device
reMarkable's cloud API is unofficial and reverse-engineered; it may change without notice
---
License
MIT License. Not affiliated with or endorsed by reMarkable AS.
