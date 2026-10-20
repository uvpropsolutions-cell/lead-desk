# GPSS Lead Desk

A free app plus a free lead-finding robot for General Public Solutions Services.

- **The app** (`index.html`): find prospects with one-tap searches, track every lead and follow-up date, and copy ready-made outreach and reply messages. Installs on a phone or computer and works offline.
- **The robot** (`tools/leadfinder.py` + `.github/workflows/find-leads.yml`): runs every night on GitHub's free servers while you sleep, collects new business leads from public sources, and saves them to `data/leads.csv`. The app pulls in anything new each time you open it.

Nothing is sent to anyone automatically. The robot only gathers public business listings. You choose who to contact and send every message yourself.

## Set up in about 20 minutes

### 1. Put the files on GitHub (free)
1. Make a free account at [github.com](https://github.com), then create a **new public repository**, for example `gpss-leads`. It must be public so the app can read the leads file. It will hold only public business listings, so keep your private notes out of it.
2. Add all the files from this folder to it: Add file, then Upload files. The workflow file is at `.github/workflows/find-leads.yml`. If your phone can't upload a folder, use Add file, then Create new file, type that exact path as the name, and paste the contents.

### 2. Tell the robot what to look for
Open `config.json` on GitHub and use the pencil icon to edit it. Set your email under `contact`. In each search you want, replace the placeholder and change `"enabled": false` to `"enabled": true`.

| Type | What it finds | What you fill in |
| --- | --- | --- |
| `osm` | Businesses in a city, by category | `city` like `"Rochester, NY"`, and `category` (run `python3 tools/leadfinder.py --list-categories` to see them). Add `"no_website": true` to keep only businesses with no website. |
| `rss` | New openings and mentions from [Google Alerts](https://www.google.com/alerts) | Create an alert such as `"grand opening" Rochester`, choose Show options, set Deliver to RSS feed, and paste that feed link as `url`. |
| `socrata` | New business licenses and permits from a city, county, or state open-data site | Search "[your city] open data business licenses". On the dataset page choose API, copy the JSON endpoint as `url`, and map its column names under `fields`. |

You can add as many searches as you like. Copy an existing block and change it.

### 3. Run it once
In your repository open the Actions tab. If asked, tap the button to enable workflows. Choose Find leads, then Run workflow. After a minute `data/leads.csv` fills in. From then on it runs by itself every night at about 4 a.m. Eastern.

### 4. Turn on your app link (GitHub Pages, free)
1. In your repository open Settings, then Pages. Under Build and deployment, set Source to Deploy from a branch, pick `main` and `/ (root)`, and Save.
2. After about a minute your app link appears at the top of that page. It looks like `https://YOUR-USERNAME.github.io/YOUR-REPO/`.
3. Open that link on your phone and install it. On iPhone use Safari, Share, Add to Home Screen. On Android use Chrome, menu, Install app.
4. Nothing else to connect. The app and the nightly leads file live on the same site, so Pipeline, then Auto-import, already points at `data/leads.csv`. Every time you open the app, new leads appear in your pipeline. Leads you delete never come back.

## Updating the app without reinstalling

You never reinstall. To add a feature, upload the new files over the old ones in your repository (Add file, Upload files, same names). GitHub republishes in about a minute. The installed app checks for changes each time you open it, shows "Updated. Reloading...", and loads the new version. Your leads stay on your device through every update.

If you connect GitHub to Claude (claude.ai Settings, Connectors), Claude can also push updates to the repository for you, so you skip the upload step.

## Appointment setter

The Setter tab turns a lead's reply into the best next message, aimed at a booked call.
1. Pick the lead and the channel, paste what they said, and tap Draft my reply.
2. It recognizes what they meant (interested, asked about price, already has a provider, asked who you are, not now, wrong person, not interested, asked to stop, or proposed a time) and writes a reply with real open times from your schedule.
3. Copy the message and send it yourself. Copying moves the lead along and sets the next follow-up date.
4. When they pick a time, tap Save meeting to move the lead to Meeting and download a calendar invite.
5. If someone asks you to stop, tap Mark do not contact. The lead is flagged and the setter will not draft messages to them.

Set your name, booking link, meeting length, days, and hours under Your availability. Nothing is sent for you.

## Find leads boxes

Area, keywords, a business or owner name, and a street address all feed the searches on the Find leads tab. Enter a name or address and a section for that business appears at the top, with a button to add it to your pipeline.

## Using the pieces by hand

```
python3 tools/leadfinder.py --city "Atlanta, GA" --category barber --no-website
python3 tools/leadfinder.py --config config.json --out data/leads.csv
```

Needs Python 3.8 or newer and nothing else. In the app, Pipeline, Backup and import lets you import any CSV by hand and download backups.

## Good to know

- **Freshness.** OpenStreetMap is volunteer-maintained, so it has gaps and stale entries. Google Alerts and city license data are your best sources for brand-new businesses. Check every lead before you contact them.
- **Delays.** GitHub's scheduled runs can start late, and GitHub pauses them after about 60 days with no activity in the repository. If that happens, open the Actions tab and re-enable the workflow.
- **Be polite to free services.** The robot waits between requests. Don't schedule it more than once a day.
- **Back up.** The app stores leads on your device. Use Download backup now and then.
- **Sending messages.** Email needs your real name and address and a working opt-out (CAN-SPAM). Only text people who agreed to texts (TCPA). Name who provides each service and never promise savings.

Installed copies refresh themselves when you upload new files, so there is no version number to change.
