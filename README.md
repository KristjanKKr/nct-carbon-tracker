# NCT – Nordic Carbon Tracker
## Azure Static Web Apps Deployment Guide

---

## What's in this folder

| File | Purpose |
|---|---|
| `index.html` | The dashboard — update the RAW data array here when new submissions arrive |
| `staticwebapp.config.json` | Azure config — allows SharePoint to embed the page in an iframe |

---

## Step-by-step deployment

### Step 1 — Create a free Azure account (if you don't have one)
Go to https://portal.azure.com and sign in with your Microsoft 365 / work account.
Nordic Office of Architecture likely already has an Azure tenant — ask your IT department.

---

### Step 2 — Create a Storage Account (simplest method, no GitHub needed)

1. In the Azure Portal, search for **"Static Web Apps"** in the top search bar
2. Click **+ Create**
3. Fill in:
   - **Subscription**: your company subscription
   - **Resource Group**: create new → name it `nct-dashboard`
   - **Name**: `nct-carbon-tracker` (must be globally unique, add your initials if needed)
   - **Plan type**: Free
   - **Region**: North Europe (closest to Norway)
   - **Deployment source**: select **"Other"** (we'll upload manually)
4. Click **Review + Create** → **Create**
5. Wait ~1 minute for deployment

---

### Step 3 — Upload the files

1. Once created, go to your new Static Web App resource
2. In the left menu click **"Deployment"** → then look for the deployment token or use the **Azure CLI**

#### Easiest: Use Azure Static Web Apps CLI
Open a terminal and run:
```
npm install -g @azure/static-web-apps-cli
swa deploy ./nct-azure --deployment-token YOUR_TOKEN --env production
```
Get `YOUR_TOKEN` from: Azure Portal → your Static Web App → **Manage deployment token**

#### Alternative: Drag & drop via Azure Portal
Some Azure subscriptions allow uploading via the portal Storage browser.

---

### Step 4 — Get your URL

After deployment your app will be live at something like:
```
https://nct-carbon-tracker.azurestaticapps.net
```
You'll see this URL in the Azure Portal on your Static Web App's Overview page.

---

### Step 5 — Embed in SharePoint

1. Go to your SharePoint page → click **Edit**
2. Click **+** to add a new web part → search for **"Embed"**
3. Paste your Azure URL:
   ```
   https://nct-carbon-tracker.azurestaticapps.net
   ```
4. Set height to **900px**
5. Click **Publish**

The `staticwebapp.config.json` file already sets the correct headers
(`X-Frame-Options: ALLOWALL`) so SharePoint can embed it without issues.

---

## Updating the data

When new CO₂ submissions arrive:
1. Open `index.html` in any text editor (Notepad, VS Code, etc.)
2. Find the `RAW` array near the top of the `<script>` section
3. Add new rows following the same format:
   ```
   ["DD.MM.YYYY","Country","Department","ProjNo","Project Name",BTA,CO2e,"25%"],
   ```
4. Save the file and re-deploy (repeat Step 3)

---

## Need help?

Contact your IT department or Azure administrator with this guide.
The free tier of Azure Static Web Apps supports up to 100GB bandwidth/month —
more than enough for this dashboard.
