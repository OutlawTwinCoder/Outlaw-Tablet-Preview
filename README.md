Readme - Outlaw Tablet (five M Esx)
Outlaw Tablet

Buy / Learn more:
Main Tebex store → https://outlaw-twinscoder-store.tebex.io/
FiveM · ESX category → https://outlaw-twinscoder-store.tebex.io/category/fivem-esx
🎬 Demo video → https://www.youtube.com/watch?v=uLU9JL1BidY

🎥 Demo




Outlaw Tablet is an all-in-one job management and invoicing suite for ESX servers. The tablet ships with banking, vehicle, and law-enforcement tooling, but its real strength is the job app framework: you can register unlimited company apps, tune their ticket presets, and control who can issue invoices. Everything can be configured from config.lua or on the fly through the Admin → Apps métiers panel, letting staff roll out new businesses without restarting the resource.

🚀 Highlights

Universal invoicing – Any job can issue invoices or service tickets that deposit into its society account. Use the preset list or ad-hoc amounts with confirmation limits.

Unlimited job apps – Duplicate the config template or create apps directly in-game. Tiles sit alongside Police, EMS, and Vehicles with per-job colors/icons.

Admin-friendly editor – Authorized admins can create, edit, and delete job apps in-game. Manage grade limits, billing toggles, search tabs, and outstanding invoice access without touching Lua.

Automatic database wiring – The server side ensures society accounts exist, upgrades ticket log schemas (including the new app column), and respects custom table names.

Shared UI – The same tablet covers bank transfers, vehicle valet, billing, ticket logs, and any business-specific workflow you configure.

📦 Installation

Drop the resource folder into your server files.

Import sql/outlaw_tablet.sql (or copy sections into your migration system).

Review config.lua to match your framework table names, default jobs, and colors.

Ensure required permissions (see Admin access) and restart your server.

The migration runner adds the app column to outlaw_ticket_log (and custom tables) if it is missing. Check your server console on first boot to confirm the upgrade succeeded.

🔧 Configuration Reference

All configurable values live in config.lua. The following sections summarize each block.

1) Opening the tablet (Config.Open)

Controls the command, required item, notification messages, and key mapping that toggle the UI.

2) Database tables (Config.DB)

Defines which SQL tables/columns store billing, vehicles, users, and society accounts. Adjust only if your schema differs from vanilla ESX.

3) Valet service (Config.Valet)

Enable or disable the valet, set prices, spawn distances, parking rules, and the ped used to deliver vehicles.

4) Admin access (Config.AdminGroups, Config.AdminDatastore)

Sets which ACE/ESX groups see the Admin app and where admin overrides persist. No extra SQL tables are required; overrides are stored in the configured datastore row.

5) Job apps (Config.JobApps)

Controls the tiles shown on the home screen:

order – determines placement relative to other job apps (lower = earlier). Custom tiles automatically insert after Vehicles and before Admin.

title, label, description – UI text.

icon – path under html/assets/.

jobs – job names that unlock the tile.

lockedText – tooltip for unauthorized players.

Duplicate the newjob template, update the values, and drop your icon in html/assets/ to add a new business. Legacy aliases (Config.PoliceJobs, etc.) remain for backward compatibility.

6) Ticket / invoicing modules (Config.TicketApps)

Pairs job apps with the shared billing backend. Each entry mirrors the job app key and adds:

Billing.Enabled – disable to keep the UI without touching ESX billing rows.

IdentityTable / TicketLogTable – optional custom tables for lookups/logging.

ManageGrade, OutstandingGrade – minimum grades for the Manage and Outstanding tabs.

SearchEnabled, OutstandingEnabled – toggle UI sections per job.

Text overrides (TicketNoun, TicketTitle, etc.) for localized vocabulary.

Tickets – allowed range and preset reasons/amounts.

Money flows to Config.DB.SocietyPrefix .. jobName. Missing accounts are created on first use.

7) EMS shortcut (Config.EMS)

Legacy EMS invoicing module. You can keep it enabled alongside your new job apps or disable it if everything moves to the unified ticket system.

8) Visual helpers (Config.JobShortLabels, Config.JobColors)

Short invoice tags and badge colors for each job. Add entries for new companies so the logs and UI match your branding.

9) Confirmation thresholds

Config.BillingConfirmThreshold and the bank/society thresholds trigger confirmation dialogs when large transactions are attempted.

🧑‍💼 In-Game Job Management

The Admin → Apps métiers panel mirrors the config and adds real-time editing:

Create a new job app – Click Ajouter une app métier. Provide a unique identifier, display name, icon path, description, job access list, ordering, and color/label overrides. Configure billing options (manage/outstanding grades, search toggle, ESX billing toggle, min/max amounts, and preset reasons). Submit to save.

Edit existing apps – Each tile in the list is collapsed by default. Click the header to expand settings, adjust fields, or reset to defaults. Changes broadcast instantly to online players.

Delete custom apps – Remove apps that were created through the admin UI with one click. The default config-defined apps cannot be deleted in-game but can be hidden by removing them from config.lua.

All admin-created apps are stored in the datastore and reapplied on restart. The server merges datastore overrides with config.lua so you can keep default apps in source control while letting staff extend the roster.

🗄️ Database Requirements for New Jobs

When adding a brand-new job/company, ensure ESX knows about it:

INSERT INTO jobs (name, label) VALUES ('newjob', 'Add New Job Name Here');
INSERT INTO job_grades (job_name, grade, name, label, salary) VALUES
  ('newjob', 0, 'employee', 'Employee', 0),
  ('newjob', 1, 'senior',   'Senior',   0),
  ('newjob', 2, 'manager',  'Manager',  0),
  ('newjob', 3, 'boss',     'Director', 0);


INSERT INTO addon_account (name, label, shared) VALUES ('society_newjob', 'Add New Job Name Here', 1);
INSERT INTO addon_account_data (account_name, money, owner) VALUES ('society_newjob', 0, NULL);

The tablet auto-creates the society account if it is missing, but pre-seeding the rows avoids first-run delays.

🖼️ Assets

Icons live in html/assets/. Use transparent 128×128 PNG/SVG files and reference them in config/job entries (e.g., assets/lawyer.png).

🧰 Troubleshooting

Missing app column – Restart after updating to let the migration add the column to outlaw_ticket_log. The server detects the schema and falls back gracefully if the column is absent.

Invoices not depositing – Confirm the society account exists and matches the job key (society_<jobname>).

Admin app hidden – Verify your player has one of the groups in Config.AdminGroups.

🛒 Links & Support

Buy / Download / Support → https://outlaw-twinscoder-store.tebex.io/

ESX Category → https://outlaw-twinscoder-store.tebex.io/category/fivem-esx

Demo Video → https://www.youtube.com/watch?v=uLU9JL1BidY

Updates and support are provided via Tebex (and/or the channel listed on the product page).

⚖️ License & Terms

This repository contains public README/docs. The full script is a commercial product distributed via Tebex and subject to its terms (and the license included in the package). No redistribution without permission.

—

© OutlawTwinCoder. All rights reserved.
