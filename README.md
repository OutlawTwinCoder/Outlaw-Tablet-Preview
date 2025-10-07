# Outlaw Tablet for FiveM ESX

Outlaw Tablet is a full-featured job management and invoicing suite tailored for ESX-based FiveM servers. The tablet provides banking, vehicle management, and law-enforcement tooling out of the box, while the modular job app framework lets you roll out unlimited business workflows without restarts. Every element—from invoicing presets to UI colors—can be configured in `config.lua` or live through the in-game admin tools.

---

## 📺 Demo & Purchase Links

- **Product page:** <https://outlaw-twinscoder-store.tebex.io/>
- **FiveM · ESX catalog:** <https://outlaw-twinscoder-store.tebex.io/category/fivem-esx>
- **Feature showcase video:** <https://www.youtube.com/watch?v=uLU9JL1BidY>

---

## 🚀 Key Features

- **Universal invoicing** – Any job can issue invoices or service tickets that deposit directly into its society account. Use configured presets or enter ad-hoc amounts with customizable confirmation thresholds.
- **Unlimited job apps** – Register company applications from the configuration file or in-game. Tiles sit alongside Police, EMS, and Vehicle tools with per-job icons and color themes.
- **Admin-friendly editor** – Authorized admins create, edit, and delete job apps inside the Admin → Job Apps panel. Manage grade limits, billing toggles, and search/outstanding tabs without touching code.
- **Automatic database wiring** – The server layer checks for required society accounts, upgrades ticket log schemas (including the new app column), and respects custom table names.
- **Shared UI** – Banking, vehicle valet, billing, ticket logs, and business-specific workflows all run through the same polished tablet interface.

---

## 📦 Installation

1. Place the resource folder inside your server files.
2. Import `sql/outlaw_tablet.sql` (or merge the statements with your existing migration system).
3. Review `config.lua` to match your framework’s table names, default jobs, colors, and access rules.
4. Ensure the required permissions are granted (see **Admin Access**), then restart your server.

> **Note:** On first boot, the migration runner adds the `app` column to `outlaw_ticket_log` (and to custom tables) if it is missing. Check the server console to confirm the upgrade completes successfully.

---

## 🔧 Configuration Overview (`config.lua`)

| Section | Purpose |
| --- | --- |
| **Config.Open** | Controls the command, required item, notifications, and key mapping that toggle the tablet UI. |
| **Config.DB** | Defines SQL tables/columns for billing, vehicles, users, and society accounts. Adjust only if your schema differs from vanilla ESX. |
| **Config.Valet** | Enables or disables the valet, sets prices, spawn distances, parking rules, and the ped used to deliver vehicles. |
| **Config.AdminGroups / Config.AdminDatastore** | Determines which ACE/ESX groups see the Admin app and where overrides are stored. Overrides live in the configured datastore row—no extra SQL is required. |
| **Config.JobApps** | Controls the tiles on the home screen, including order, labels, descriptions, icons, job access, and locked tooltips. Duplicate the template entry, update the values, and place your icon in `html/assets/` to add new businesses. Legacy aliases (`Config.PoliceJobs`, etc.) remain for backward compatibility. |
| **Config.TicketApps** | Links job apps to the shared billing backend. Configure billing toggles, identity and ticket log tables, grade requirements, tab visibility, localized text, and preset ticket ranges. Money flows to `Config.DB.SocietyPrefix .. jobName`; missing accounts are created automatically. |
| **Config.EMS** | Legacy EMS invoicing module. Keep it enabled alongside unified ticket apps or disable it if everything moves to the new system. |
| **Config.JobShortLabels / Config.JobColors** | Defines short invoice tags and badge colors for each job so logs and UI elements match your branding. |
| **Config.BillingConfirmThreshold** and bank/society thresholds | Trigger confirmation dialogs when large transactions are attempted. |

---

## 🧑‍💼 In-Game Job Management

The Admin → Job Apps panel mirrors the configuration file and supports real-time edits:

- **Create** – Select **Add Job App**, provide a unique identifier, display name, icon path, description, job access list, order, and color/label overrides. Configure billing settings (manage/outstanding grades, search toggle, ESX billing toggle, min/max amounts, and preset reasons) before saving.
- **Edit** – Each tile is collapsed by default. Expand a tile to adjust settings or reset to defaults. Changes broadcast instantly to online players.
- **Delete** – Remove apps created through the admin UI with one click. Default config-defined apps cannot be deleted in-game but can be hidden by editing `config.lua`.

Admin-created apps are stored in the datastore and re-applied on restart. The server merges datastore overrides with `config.lua`, letting you keep default apps in source control while empowering staff to extend the roster.

---

## 🗄️ Database Requirements for New Jobs

When adding a brand-new job or company, make sure ESX has the required entries:

```sql
INSERT INTO jobs (name, label) VALUES ('newjob', 'Add New Job Name Here');

INSERT INTO job_grades (job_name, grade, name, label, salary) VALUES
  ('newjob', 0, 'employee', 'Employee', 0),
  ('newjob', 1, 'senior',   'Senior',   0),
  ('newjob', 2, 'manager',  'Manager',  0),
  ('newjob', 3, 'boss',     'Director', 0);

INSERT INTO addon_account (name, label, shared) VALUES ('society_newjob', 'Add New Job Name Here', 1);
INSERT INTO addon_account_data (account_name, money, owner) VALUES ('society_newjob', 0, NULL);
```

The tablet auto-creates the society account if it is missing, but pre-seeding the rows avoids first-run delays.

---

## 🖼️ Asset Guidelines

- Place icons in `html/assets/`.
- Use transparent 128×128 PNG or SVG files.
- Reference the asset path (for example, `assets/lawyer.png`) within the relevant job app configuration.

---

## 🧰 Troubleshooting

- **Missing app column** – Restart after updating so the migration can add the `app` column to `outlaw_ticket_log`. The server detects the schema automatically and falls back gracefully when the column is absent.
- **Invoices not depositing** – Confirm the society account exists and matches the job key (`society_<jobname>`).
- **Admin app hidden** – Verify the player belongs to one of the groups listed in `Config.AdminGroups`.

---

## 🛒 Support & Updates

- Purchase, download, and support are handled through Tebex: <https://outlaw-twinscoder-store.tebex.io/>
- The FiveM ESX catalog page lists product updates and documentation links.
- The demo video showcases the workflow and UI for staff and players.

---

## ⚖️ License & Terms

This repository contains public documentation. The full script is a commercial product distributed via Tebex and remains subject to its terms (and the license included with the package). Redistribution without written permission is prohibited.

---

© OutlawTwinCoder. All rights reserved.

