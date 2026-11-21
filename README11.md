# Remina — Spaced Revision & Study Planner

![Remina — spaced revision and study planner](assets/poster.png)

**Log what you study in one second. Let Remina bring it back at the moment memory starts to fade — 3, 7, 15, 30, then every 30 days, forever.**

`v1.0.0` · the scheduler runs entirely on your device, your whole study log lives in `localStorage`, the app installs offline, and every topic you add is automatically slotted into a review calendar you never have to maintain.

[![Live demo](https://img.shields.io/badge/live_demo-keshav--cmd.github.io%2FRemina-D8B878?style=flat-square&labelColor=0B0D12)](https://keshav-cmd.github.io/Remina/)
[![Download APK](https://img.shields.io/badge/download-Android_APK-D72638?style=flat-square&labelColor=0B0D12)](https://github.com/Keshav-cmd/Remina/raw/refs/heads/main/app/Remina.apk)
[![Built with](https://img.shields.io/badge/built_with-HTML_%2B_CSS_%2B_JS-A78BFA?style=flat-square&labelColor=0B0D12)](#technology)
[![No dependencies](https://img.shields.io/badge/dependencies-zero-9299A9?style=flat-square&labelColor=0B0D12)](#technology)

[**Open Remina**](https://keshav-cmd.github.io/Remina/) · [Download the APK](https://github.com/Keshav-cmd/Remina/raw/refs/heads/main/app/Remina.apk) · [Screenshots](#screenshots) · [How the scheduler works](#how-the-scheduler-thinks)

---

**Remina** is a free, offline-first **spaced-revision planner** that runs entirely in your browser. You log a topic once — *"photosynthesis," "Bayes' theorem," "the causes of WWI"* — and Remina schedules its reviews at the proven **3 · 7 · 15 · 30 + 30** rhythm, surfaces them on a colour-coded calendar, and puts today's obligations at the top of the screen with a progress bar you can actually fill. It is built on a single page of HTML, CSS and JavaScript with no backend, no account, no analytics and no network calls: every date computation, every greeting, every review chip is calculated on your own device, which means it also works **offline** and keeps your study log private by construction.

## Table of contents

- [Open Remina](#open-remina)
- [Screenshots](#screenshots)
- [Features](#features)
- [How the scheduler thinks](#how-the-scheduler-thinks)
- [The 3 · 7 · 15 · 30 rhythm](#the-3--7--15--30-rhythm)
- [Theme system — three rooms, one token contract](#theme-system--three-rooms-one-token-contract)
- [Download & install](#download--install)
- [Controls & keyboard shortcuts](#controls--keyboard-shortcuts)
- [Project structure](#project-structure)
- [FAQ](#faq)
- [Roadmap](#roadmap)
- [Credits](#credits)

## Open Remina

**[keshav-cmd.github.io/Remina](https://keshav-cmd.github.io/Remina/)** — works in any modern desktop or mobile browser, installs to nothing, and needs no account. Pick a theme, tell Remina your name, and start logging.

Prefer an app icon on your home screen? Grab the Android build: **[`app/Remina.apk`](https://github.com/Keshav-cmd/Remina/raw/refs/heads/main/app/Remina.apk)**.

## Screenshots

| Desktop — Obsidian & Champagne | Mobile — bottom tab bar, sticky top bar |
| :---: | :---: |
| ![Remina desktop dashboard: named greeting, quick-add bar, stats, revision calendar, today's plan](assets/screenshot-desktop.png) | ![Remina mobile layout: thumb-sized targets, safe-area aware top bar, calendar above today's plan](assets/screenshot-mobile.png) |

| First-visit theme picker | Onboarding — tell Remina your name |
| :---: | :---: |
| ![Three theme cards: Modern Editorial, Frosted Glass, Obsidian & Champagne](assets/screenshot-themes.png) | ![Name entry with a warm, time-aware greeting preview](assets/nameset.png) |

## Features

### Smart spaced-revision scheduling

- Automatic review scheduling on the proven **3 · 7 · 15 · 30 + 30** rhythm — no sliders, no settings, no per-topic configuration.
- After day 30, reviews repeat **every 30 days** (day 60, 90, 120…) so a topic you learned in March is still being recalled in December.
- Every topic gets its own review timeline, visible at a glance on the calendar.

### One-second study logging

- **Quick-add bar**: type what you studied, pick or type a subject, press **Enter** — done in a second.
- Subjects are fully flexible; Remina remembers your last-used one and suggests the rest as you type.
- Optional notes per entry, plus a full editor for back-dating and detail.

### Your morning to-do, front and centre

- The **Today's plan** panel shows every review due today with a live progress bar.
- Big, satisfying **check buttons** — mark a review complete with one tap and watch it check off.
- Overdue reviews are flagged in crimson, so nothing slips through the cracks.

### Calendar at a glance

- Monthly calendar with **colour-coded review chips** — each subject gets its own tone.
- Select any day to see its reviews, study entries, and progress.
- Today, the selected day, and upcoming reviews are always visually distinct.

### Study log & review queue

- **Study log**: every entry with studied date, subject, notes, and next review date — searchable.
- **Review queue**: overdue, today, upcoming, and recently completed reviews, grouped clearly.
- Filter the queue by *To review*, *All*, or *Done*.

### A warm study companion

- **Time-aware greetings by name**: *"Good morning, Keshav," "Good evening," "Still up," "Burning the midnight oil"* — Remina greets you in the moment you open it.
- **Daily pace question**: *"So, what are we doing today?"* — answer **Yes, let's study** or **Not today**.
- First visit: pick your theme, then tell Remina your name — personal from the very first minute.

### Three beautiful themes

- **Modern Editorial** — daytime calm: serif headers, warm off-white paper tones.
- **Frosted Glass** — sleek, modern app feel: blurred translucent cards over a soft gradient.
- **Obsidian & Champagne** — evening luxury: matte dark greys with warm gold accents.
- Switch any time; your choice is saved, and the browser `theme-color` follows so the mobile chrome matches.

### Mobile-first, desktop-perfect

- **Native-app feel on phones**: bottom tab bar, bottom-sheet dialogs, enlarged touch targets.
- No input-zoom jumps, safe-area aware for notched phones, sticky top bar while scrolling.
- Desktop is equally polished — one codebase, every screen.

### Private & local

- Everything is stored **on your device** (`localStorage`) — no accounts, no servers, no tracking.
- The status bar simply says **Saved**. Your study data never leaves your browser.
- The whole app is cached by a service worker, so "Add to Home Screen" opens with no network.

## How the scheduler thinks

The schedule is computed entirely on your device in plain JavaScript — no server, no queue, no background job.

| Layer | Implementation |
| ----- | -------------- |
| Interval set | 3, 7, 15, 30 days after the studied date, then **+30 indefinitely** |
| Date math | Calendar-day addition in the local timezone — a review on day 3 is 3 *dates* later, not 72 hours |
| Review object | `{ stage, due: 'YYYY-MM-DD', done: boolean }` per topic |
| Due today | `due === today && !done` |
| Overdue | `due < today && !done` |
| Completed | `done === true` |
| Next review | The earliest `due` across all stages that is not yet done |
| Persistence | `localStorage`, one record per study entry, schema-versioned |
| Completion state | Per-stage — completing day 3 does not affect day 7 |

Two properties make the schedule trustworthy:

1. **It is deterministic.** The same topic logged on the same date always produces the same review dates. There is no server clock to drift and no scoring model to change under your feet.
2. **It is idempotent.** Re-opening the app, refreshing the page or re-importing the same log never creates duplicate reviews — a review is keyed by `(topic, stage)`.

Because the whole model fits in a few hundred lines of `app.js`, it is readable, testable, and portable. The roadmap includes custom intervals per subject; the data model already supports it.

## The 3 · 7 · 15 · 30 rhythm

For every topic you log, Remina schedules reviews on these days after you studied it:

| Stage | When | Purpose |
| :---: | :---: | :--- |
| **1st review** | Day 3 | First recall — catches the forgetting curve early |
| **2nd review** | Day 7 | Strengthens the memory a week later |
| **3rd review** | Day 15 | Mid-term retention |
| **4th review** | Day 30 | First long-term review |
| **Ongoing** | **+30 every month** | Day 60, 90, 120… — keeps it in long-term memory |

**The routine is simple:** log it, revisit it, check it off, and watch it grow.

## Theme system — three rooms, one token contract

Every colour in `styles.css` resolves from a per-theme token block, so switching themes is a single class change on the document root — no per-component overrides, no hardcoded hexes anywhere else in the stylesheet.

| Theme | Feel | Primary surfaces | Accent |
| ----- | ---- | ---------------- | ------ |
| **Modern Editorial** | Daytime calm | Warm off-white paper | Ink black, muted red |
| **Frosted Glass** | Modern app | Translucent white over a soft gradient | Cool blue-grey |
| **Obsidian & Champagne** | Evening luxury | Matte dark greys | Champagne gold |

The three themes are not palette swaps — each one defines its own surfaces, text, borders, and accents under the same token names, so every component stays correct regardless of which room you are in. Your chosen theme is written to `localStorage` and also drives the `<meta name="theme-color">` value, so the mobile browser chrome matches the app.

## Download & install

### Open in the browser

Open **[keshav-cmd.github.io/Remina](https://keshav-cmd.github.io/Remina/)** — nothing else required.

### Android APK

Download **[`app/Remina.apk`](https://github.com/Keshav-cmd/Remina/raw/refs/heads/main/app/Remina.apk)** and open it on your phone. Android will ask for permission to install from your browser or file manager the first time (*Settings → Install unknown apps*). The same build works offline.

### Run it locally

```bash
git clone https://github.com/Keshav-cmd/Remina.git
cd Remina
python3 -m http.server 8000
# then open http://localhost:8000
```

There is no build step and there is no `npm install` — `index.html` opens directly from disk too, which is also why the app works offline.

## Controls & keyboard shortcuts

| Key | Action |
| :---: | --- |
| `n` or `/` | Focus the quick-add bar (on the calendar view) |
| `Esc` | Close the active dialog |
| *Click / tap* | Check off a review, open an entry, pick a day |
| *Long-press chip* | Subject filter menu on the calendar (mobile) |

Full pointer navigation: every actionable target is keyboard-focusable, visible focus rings are drawn on all inputs, and `prefers-reduced-motion` disables the entrance animations on the calendar and review chips.

## Project structure

```text
Remina/
├── index.html              # app shell — dashboard, calendar, dialogs, quick-add
├── styles.css              # three themes + responsive layout (all colour is tokens)
├── app.js                  # scheduling, storage, rendering, input, companion
├── README.md               # this file
├── patch.md                # change log — every patch recorded
├── app/
│   └── Remina.apk          # Android build
└── assets/
    ├── poster.png          # README hero image
    ├── screenshot-desktop.png
    ├── screenshot-mobile.png
    ├── screenshot-themes.png
    └── …                   # fonts, icons, brand marks
```

## FAQ

**Is Remina free?**
Yes. The whole app is free to use in the browser or as an APK, with no ads, accounts or paywalls.

**Do I need to install anything to use it?**
No. It is a static site — HTML, CSS and JavaScript only — so it runs in any modern browser and even loads straight from the filesystem.

**Where is my data stored?**
In your browser's `localStorage`, on your device. Nothing is ever sent anywhere: there is no backend, no analytics, and no network call after the first page load. The status bar in the app simply says **Saved**.

**Does it work offline?**
Yes. Once the page is loaded (or the APK is installed), the whole app and your data live on your device, so no internet connection is required to log studies or check off reviews.

**What if I clear my browser data or switch devices?**
Your log will be gone — `localStorage` is per-browser and per-device. Import/export and cloud sync are on the roadmap; until then, treat the device you use as the single home of your log.

**How does the 3 · 7 · 15 · 30 schedule work?**
A topic logged on a given date gets reviews on day 3, day 7, day 15 and day 30, then every 30 days after that (day 60, 90, 120…). Each stage is tracked independently, so completing the day-3 review does not affect the day-7 one.

**Can I customise the intervals?**
Not yet. The 3 · 7 · 15 · 30 + 30 rhythm is fixed, deliberately — the point is to remove the decision. Custom intervals per subject are on the roadmap.

**Can I back-date a study entry?**
Yes. The entry editor accepts any studied date; reviews are then scheduled from that date forward.

**What's the difference between the three themes?**
Only the palette and typography. Every theme has the same layout, the same gestures, and the same data — so you can switch at any time without losing anything.

**Is there a mobile app?**
Yes — the Android APK above. On iPhone and iPad, use **Share → Add to Home Screen** to install the web app with a real icon that opens without browser chrome.

**Is my study data ever uploaded, even for analytics?**
No. There is no analytics, no error reporter, no telemetry of any kind in the shipped build.

## Roadmap

Potential future directions. These are ideas, not promises that the features are currently available.

- [ ] Cloud sync across devices (with privacy controls)
- [ ] Push reminders for due reviews
- [ ] Custom review intervals per subject
- [ ] Study statistics, streaks, and heatmaps
- [ ] Import/export for backing up your data
- [ ] Installable, offline-capable PWA

## Contributing

Bug reports, suggestions, and contributions are welcome. To propose a change:

1. Fork this repository.
2. Create a branch:

   ```bash
   git checkout -b feature/your-feature
   ```

3. Make and test your changes.
4. Commit your work:

   ```bash
   git commit -m "Add: describe your change"
   ```

5. Push the branch and open a Pull Request.

## License

Remina is released under the **MIT License** — free to use, study, modify, and distribute. See the [LICENSE](LICENSE) file for details.

## Credits

Designed and built by **Keshav Jha**.

- Live site: [keshav-cmd.github.io/Remina](https://keshav-cmd.github.io/Remina/)
- Source: [github.com/Keshav-cmd/Remina](https://github.com/Keshav-cmd/Remina)

If Remina helps you remember something, a ⭐ on the repository genuinely helps other learners find it.

---

**REMINA · MEMORY WITHOUT BOUNDARIES**

*Study once. Remember longer.*

`3 · 7 · 15 · 30 · +30` · the schedule runs on your device and never leaves it
