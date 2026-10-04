# Network Engineering 30-Day Study Plan — PWA

A mobile-friendly, installable Progressive Web App (PWA) for tracking a 30-day Network Engineering study plan.

The study plan covers networking fundamentals through a CCNA-level foundation, including OSI/TCP-IP, Ethernet, IPv4, subnetting, DHCP, ARP, TCP/UDP, DNS, routing, VLANs, NAT, ACLs, Linux troubleshooting, and a final mini-project.

## Live Website

**https://razaan16ali.github.io/network-engineering-webapp/**

The application is hosted using GitHub Pages.

---

## Features

### Study Checklist

* Day-by-day Network Engineering study plan
* Individual checkbox for every task
* Daily completion tracking
* Weekly completion percentages
* Overall completion percentage
* 28 days of structured study tasks
* Final completion checklist
* "Jump to next incomplete task" functionality
* Collapse/expand weekly sections

### Progress Tracking

* Progress is saved locally on the device.
* When logged in, progress is synchronized with Supabase.
* The same account can be used on multiple devices.
* Progress checked on a laptop can appear on an Android phone and vice versa.
* Logging out clears the displayed checklist without deleting the account's saved cloud progress.
* Logging back into the same account restores the saved cloud progress.
* The **Reset Progress** button permanently resets the user's checklist progress locally and in Supabase.

### Authentication

* User registration
* Email/password login
* Email verification support
* Password reset
* Logout
* Supabase authentication
* Per-user cloud progress

### PWA

* Installable on supported Android devices and browsers
* Home-screen app icon
* Standalone application experience
* Service-worker based caching
* Mobile-responsive interface
* Can load the application shell from cache

---

## Technology Stack

### Frontend

* HTML
* CSS
* JavaScript
* Responsive/mobile-first interface

### Hosting

* GitHub Pages

### Backend

* Supabase

### Authentication

* Supabase Auth

### Database

* Supabase PostgreSQL

### Offline/PWA

* Web App Manifest
* Service Worker
* Browser LocalStorage

---

## Project Structure

```text
network-engineering-webapp/
│
├── index.html
├── config.js
├── manifest.json
├── service-worker.js
├── README.md
│
└── icons/
    ├── icon-192.png
    └── icon-512.png
```

### `index.html`

Contains the main application interface, study plan, checklist logic, authentication UI, progress tracking, and Supabase integration.

### `config.js`

Contains the Supabase project URL and browser-safe publishable key.

The Supabase publishable key is intended for use in the frontend. Database security is provided through Supabase Row Level Security (RLS).

**Never put a Supabase secret key, service-role key, database password, or other server-side credentials in this file.**

### `manifest.json`

Defines the PWA metadata, application name, icons, display mode, and installation behavior.

### `service-worker.js`

Provides application-shell caching and PWA functionality.

### `icons/`

Contains the application icons used by the PWA.

---

## Supabase Database

The application uses a table named:

```text
study_progress
```

The table stores individual task/milestone completion records.

The relevant fields are:

```text
id
user_id
task_id
completed
updated_at
```

A unique constraint exists on:

```text
(user_id, task_id)
```

This allows the application to safely use Supabase `upsert()` operations without creating duplicate records.

---

## Row Level Security

Row Level Security (RLS) is enabled on the `study_progress` table.

Users are restricted to their own records.

The policies allow authenticated users to:

* SELECT their own progress
* INSERT their own progress
* UPDATE their own progress
* DELETE their own progress

The application relies on these policies to protect user data.

---

## How Synchronization Works

When a user checks a task while logged in:

```text
Checkbox
   ↓
Local application state
   ↓
LocalStorage
   ↓
Supabase study_progress
```

When the user logs into the same account on another device:

```text
Login
   ↓
Supabase Authentication
   ↓
Load study_progress
   ↓
Application state
   ↓
Checklist restored
```

Therefore, the same Supabase account can maintain the same study progress across supported devices.

---

## Logout Behavior

Logging out does **not** delete the user's cloud progress.

Instead:

```text
Logout
   ↓
Clear displayed/local checklist state
   ↓
Supabase session ends
```

When the same account logs in again:

```text
Login
   ↓
Cloud progress loaded
   ↓
Previous checklist state restored
```

This keeps logout separate from the **Reset Progress** function.

---

## Reset Progress

The **Reset Progress** button is different from logout.

Resetting progress:

1. Clears the local checklist state.
2. Clears the user's saved `study_progress` records in Supabase.
3. Starts the checklist from zero.

This action should only be used when the user intentionally wants to restart the study plan.

---

## Installation on Android

On a supported Android browser such as Chrome:

1. Open the live website.
2. Open the browser menu.
3. Select **Install app** or **Add to Home screen**.
4. Confirm the installation.
5. Launch the application from the Android home screen.

The installed application uses the same website and Supabase account as the browser version.

---

## Important Notes

### Internet Connection

The application shell can be cached by the service worker, but authentication and cloud synchronization require access to Supabase.

For reliable synchronization, use the application while connected to the internet.

### Multiple Devices

To synchronize progress between devices:

1. Open the application on each device.
2. Log into the **same account**.
3. The saved progress will be retrieved from Supabase.

Different accounts have separate progress.

### GitHub Pages

The repository must keep `index.html` in the repository root for the current GitHub Pages configuration.

Do not upload the ZIP package instead of the individual website files.

---

## Security

The frontend contains the Supabase **publishable key**.

This is expected for a client-side Supabase application.

Security must be enforced using:

* Supabase Authentication
* Row Level Security
* Correct database policies

Never expose:

```text
sb_secret_...
service_role
database passwords
private server credentials
```

in the frontend or GitHub repository.

---

## Updating the Website

To update the application:

1. Edit the required files in the GitHub repository.
2. Commit the changes.
3. GitHub Pages automatically publishes the updated version.
4. The service worker may temporarily continue serving a cached version.
5. Refresh the website or reopen the PWA after the new version becomes available.

If a major frontend update appears not to be loading, clear the site's cached data or reinstall/update the PWA.

---

## Study Plan

The application is based on a 30-day Network Engineering study plan designed around:

**Computer Networks — Andrew S. Tanenbaum**

The primary structured study period contains Days 1–28, followed by a completion checklist and next-step recommendations.

### Main topics

* OSI and TCP/IP models
* Encapsulation and decapsulation
* Ethernet
* MAC addresses
* Switching
* Binary and IPv4 addressing
* Subnetting
* DHCP
* ARP
* TCP and UDP
* Ports
* DNS
* Routing
* Cisco Packet Tracer
* VLANs
* Access ports
* Trunking / 802.1Q
* Inter-VLAN routing
* NAT/PAT
* ACLs
* Linux networking
* Network troubleshooting
* Final mini-project

---

## Future Improvements

Possible future improvements include:

* More detailed sync status
* Improved offline synchronization
* Better PWA update handling
* User profile/settings
* Study streak tracking
* Notes for individual tasks
* Study session timers
* Additional CCNA material
* Network automation exercises using Python
* Netmiko/API/Ansible exercises
* More advanced networking labs

---

## License

This project is intended as a personal educational study tool.

The underlying study material and textbook remain the property of their respective authors/publishers.
