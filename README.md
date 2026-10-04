# Jobtrail

Job application tracker: board, company analytics, Gemini analysis, and your data in a Google Sheet you own.

```
jobtrail/
├── index.html    the entire app, including the Google Sheet sync script
├── vercel.json   hosting config and security headers
└── README.md
```

There is one codebase. The Apps Script that runs in your Google Sheet lives inside
`index.html` in `<script type="text/plain" id="sheet-script">`. The app copies it for you
with your secret filled in, so there is nothing else to keep in sync.

## Run

Open `index.html` in a browser, or deploy:

```bash
npm i -g vercel && vercel login
vercel --prod
```

Pushing this repo to GitHub and importing it in Vercel also works: there's no build step.

## Link your Google Sheet (once)

In the app: **Settings › Google Sheet**.

1. Create a sheet at https://sheets.new and open **Extensions › Apps Script**.
2. Click **Copy setup script**, paste it over the sample code and save.
3. Choose `setup` in the function menu, click **Run** and allow access.
4. **Deploy › New deployment › Web app**. Execute as **Me**, Who has access **Anyone**.
5. Paste the Web app URL into the app and click **Link sheet**.

The app checks the sheet answers correctly before it saves the link.

### Other devices
On a linked device, click **Copy link code**. On the new device, paste that code into the same
field and click **Link sheet**. Keep the code somewhere safe (a password manager), since it's
how you relink if browser storage is ever cleared.

### How the link is kept
The link (Web app URL and secret) is stored in browser storage under `jobtrail.connection`,
separate from your data. It stays until you click **Unlink** or clear the site's storage.
Importing a backup or deleting all applications doesn't touch it.

## Updating the sheet script

When you change the script inside `index.html`, bump `SCRIPT_VERSION` in it. Linked browsers
see "Script update available" and get a **Copy new script** button. Paste it over the old code
in Apps Script, then use **Deploy › Manage deployments › Edit › Version: New version › Deploy**.
The URL stays the same.

## Where data lives

| What | Where |
|---|---|
| Applications, rounds, questions, stage history, reports, resume | Your Google Sheet (source of truth) |
| Working copy for speed and offline use | Browser storage, `jobtrail.v1` |
| Sheet link | Browser storage, `jobtrail.connection` |
| Gemini API key | Browser storage only, never written to the sheet |
